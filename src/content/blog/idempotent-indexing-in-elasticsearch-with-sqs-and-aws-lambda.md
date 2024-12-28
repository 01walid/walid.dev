---
author: Walid Ziouche
title: 'Idempotent indexing in Elasticsearch with SQS and AWS Lambda'
pubDatetime: 2020-05-14T09:55:56+01:00
draft: false
tags: ['elasticsearch', 'sqs', 'lambda', 'aws']
description: 'Idempotent indexing in Elasticsearch with SQS and AWS Lambda'
hackerNewsId: ''
lobstersId: ''
---


This stayed like a draft blog post for months (in my mind at least), but here I am sharing it finally. 

But hey, such a scary title? don't be. [idempotent](https://en.wikipedia.org/wiki/Idempotence#Computer_science_meaning) is just a fancy word, meaning, repeating the same action again and again would yield the same [expected] result.

Months ago, I (at work) had to refactor an architecture of an existing Python program, that simply does:

- Continuously fetch some data from a DB. 
- [ETL](https://en.wikipedia.org/wiki/Extract,_transform,_load)-it, and index it in Elasticsearch.


Simple right? right. 

Well, we had 02 Elasticsearch clusters (in two different AWS regions). So that program was deployed twice (2x servers, one in each region). More importantly, each instance was basically doing the same SQL queries, putting double the load on that -already busy- database. That didn't feel right, we're talking about millions of records each day.


We then agreed on the following architecture instead, so picture this:
- Keep one instance (server), but instead of talking to Elasticsearch, just send records to a [AWS SNS](https://aws.amazon.com/sns/) topic instead.
- Have two [SQS](https://aws.amazon.com/sqs/)s queues subscribed to that SNS topic (one for each target AWS region).
- Have a [Lambda](https://aws.amazon.com/lambda/) function that consumes from each SQS queue and do the indexing to the target Elasticsearch in that region. 


AWS SNS, SQS, and Lambda are cheap. And they did fit nicely. 

# The issue

The SQS queue chosen is not ordered (not a FIFO queue, mainly because a FIFO can't subscribe to SNS in AWS), so insertion order was not guaranteed. 

This would mean a more recent version of a given record coming from the DB, could get indexed first and later overwritten by an older version of the **same** record. Enter [eventual consistency](https://en.wikipedia.org/wiki/Eventual_consistency).



How to make Elastcsearch index the record if only it does not exist or is a more recent version of the one already indexed? In other words, make Elasticsearch indexing idempotent?

# The fix

Naturally, you'd explore Elasticsearch [Update API](https://www.elastic.co/guide/en/elasticsearch/reference/6.6/docs-update.html), especially its conditional update capabilities via scripting, you'd do something like: 

```json
POST test/_doc/1/_update
{
    "scripted_upsert": true,
    "script" : {
        "inline": "if (ctx._source.timestamp >= params.timestamp) { ctx.op = 'none' } else { ctx._source = params.doc }",
        "lang": "painless",
        "params": {
            "timestamp": 1478219029000,
            "doc": {}
        }
    }
}
```

This was a bit clumsy-looking to me, it'd require embedding it in our bulk action indexing in Lambda. Also that would increase the message payload in SNS/SQS because of its verbosity (I was compacting and trimming the payload to fit as many records as we can in 256KB, the message size limit in SNS/SQS).

I kept looking for something more like "blind" indexing, thought about versioning, then quickly found out Elasticsearch does [support versioning](https://www.elastic.co/blog/elasticsearch-versioning-support).


In a nutshell, a document in Elasticsearch can have a `_version` number, starts at `1` and increments with each update. But, index operations can be instructed to use `external` versioning, which is also a number, but **externally handled** by the caller/client, it can hold as big numbers as up to `9.2e+18` and can be provided with each index operation, if the version number given by a new index operation is lower than the existing doc version, a `409 CONFLICT` status will be returned and the update will be refused. Versioning is also supported in Elasticsearch bulk operations which we were using. 

Internally, all Elasticsearch has to do is compare the two version numbers. Elasticsearch folks call this Optimistic Locking. This is much lighter than acquiring and releasing a lock as it'd do with the Update API.

### timestamp as a version number

`9.2e+18` is more than enough space to hold [Unix epoch](https://en.wikipedia.org/wiki/Unix_time) timestamps, .. and for each record, we do have a kind of "updated at" timestamp field to convert to epoch and use as a version number. If an insert comes after an update, simply nothing will happen (we discard 409 responses). 

We tested this and this is how we roll now. We have that same Lambda doing the indexing in parallel (a lambda instance per incoming message). All doing
bulk indexing, without issues. Both clusters are crunching data while being healthy. We've attached DLQs (Dead Letter Queues) to the existing SQS queues, where Lambda would redrive the records that failed to index, but rarely anything is landing there.
