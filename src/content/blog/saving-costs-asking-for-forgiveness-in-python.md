---
author: Walid Ziouche
title: 'Saving costs asking for Forgiveness in Python'
pubDatetime: 2021-09-02T18:56:21+01:00
tags: 
    - python
    - engineering
    - performance
    - threading
description: 'Desc'
draft: false
# Removing Hugo-specific metadata
# Converting image path to work with Astro
# image: 
#     url: '/images/image.jpg'
#     alt: 'Blog post image'
---

I was having a chat with a dear friend about a couple coding styles, known as [EAFP](https://docs.python.org/3.9/glossary.html#term-eafp) and [LBYL](https://docs.python.org/3.9/glossary.html#term-lbyl). 

- EAFP being: Easier to ask for forgiveness than permission,
- and LBYL being: Look before you leap. 


EAFP is a common coding idiom and style in Python, in a nutshell, it encourages letting bits of code fail and catch that failure and handle it (ask for forgiveness), instead of checking -with if statements- if the code may fail or execute correctly (asking permission). 

The saying “Ask forgiveness, not permission” is attributed to [Grace Hopper](https://en.wikipedia.org/wiki/Grace_Hopper) but there might [more to the story](https://changelog.com/posts/what-admiral-grace-hopper-really-meant) if you're interested. 

Where in contrast "Look before you leap", is a common proverb for "you shouldn't act without first considering the possible consequences or dangers". And it's also another coding style common to many other languages such as C.

Quoting from Python's docs: 

> In a multi-threaded environment, the LBYL approach can risk introducing a race condition between “the looking” and “the leaping”. For example, the code, if key in mapping: return mapping[key] can fail if another thread removes key from mapping after the test, but before the lookup. This issue can be solved with locks or by using the EAFP approach.

On top of that great example above on why EAFP might be preferred, chatting with the friend made me recall another experience I had with EAFP that I want to share in this blog post. This time EAFP didn't make us avoid a race condition, but induced cost-saving. 

About a year ago, one of the projects I'm maintaining had to send messages over [AWS SNS](https://aws.amazon.com/sns/), hundreds of millions messages a month, occasionally, few messages exceed the size limit of [256KB enforced by SNS](https://aws.amazon.com/about-aws/whats-new/2013/06/18/amazon-sqs-announces-256KB-large-payloads/).

To circumvent this limit, we've found that 256KB is well enough room if we compress these kind of messages. But compressing produces bytes (binary data), and AWS SNS accepts text-based messages, so we had to encode the data with base64 before sending over to SNS.


On the receiving side, there was a lambda function consuming the messages off that SNS topic, and it has now to deal with two types of messages, normal (uncompressed, non-base64) messages, which is like 99.8% of the messages, and few compressed (base64, binary) messages that'd appear once in a while. 

Since Lambda functions execution time is crucial costs-wise (billed by the millisecond, again, for hundreds of millions of messages ..), checking and handling compressed messages each time can be time (thus costs) consuming. 

This specific use case was a perfect example of "Ask forgiveness, not permission". 

Instead of checking the type of message if it's base64 within the lambda function for each and every message received, like: 

```python
if message_is_compressed(message):
    # handle compressed messages
else:
    # handle normal messages
```

Which might be obvious to many developers, but it's not cost-effective. Instead, I thought I'd ask for forgiveness:



```python
try:
    content = json.loads(message) # 99% of the cases this will pass, no checks needed
except json.decoder.JSONDecodeError:
    log.info("Failed to decode the message. Probably compressed, trying to decompress...")
    message = b64decode(message)
    message = decompress(message).decode("utf-8")
    content = json.loads(message)
    log.info("Message successfully decompressed.")
```
99.8% of the cases, the code within the `try` block will succeed, and without any checks (string scans) or if statements, so no extra runtime overhead, no extra costs! 

Only the rest of 0.02% of the messages will make the code enter the `except` block, and might induce a slight runtime overhead, but it's worth it. Since they're few, the sum of the overheads for these kind of messages is negligible. 


It's another reason for you to -maybe- you want to consider EAFP for such good cases.