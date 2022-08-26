+++
title = 'Stick to Tinkering'
date = 2022-07-18T20:59:39+01:00
draft = false
tags = ["engineering", "tinkering", "systems", "problem solving"]
description = "Desc"

# For twitter cards, see https://github.com/mtn/cocoa-eh-hugo-theme/wiki/Twitter-cards
# meta_img = "/images/image.jpg"

# For hacker news and lobsters builtin links, see github.com/mtn/cocoa-eh-hugo-theme/wiki/Social-Links
hacker_news_id = ""
lobsters_id = ""
+++

If you ask me how I would describe the solo-practice of software engineering, I'd sum it up to 03 skills:

- **Thinking rationally**: The ability to reason about a given problem in abstraction in a logical manner,
- **Conveying intent**: The communication skill with a mental precision to transfer that reasoning to the machine in a way it can process it as expected, 
- **Understanding systems**: This is what's often overlooked. And is what this post is mostly about. 

And I'm saying the "solo" practice because I'm intentionally removing the people/management vector from the equation. 


What got us -developers- to programming and computer science is often times the passion to create something and see it in-action, probably on a screen. You are [self] taught how to fullfil this passion. Little attention is given to where that "something" you build is going to run, be deployed or behave within an environment.

With the rapid development happening in our industry, we've got "layers of abstraction" acting like piles of convenience that shorten the path to that end goal... build something. 

That *convenience* is even advertised. Like "X is being taken care of for you, focus on what matters", "make ship happen", "Forget X, focus on Y". That might lead you to think you're only valuable when you ship, build or help build something. Which is true, especially to businesses. But are you building on solid grounds? Are you correctly investing in yourself this way?

Those layers of abstraction are now packed. Providing entire dev "stacks". Each layer is trying to hide its underlying system so "you don't have to deal with it"... Again, not necessarily harmful or bad, but I do think there are times to pause and reflect, are you mentally challenged or stagnant? are you valuable past a certain build/ship threshold? 


Most developers, juniors but also some experienced ones, that I had the chance to mentor or work with tend to focus on dev stacks, tools, framework, and how to get things done. Often, very few are aware of the underlying operating system, runtime or environment. I came to the surprising conclusion that very few developers -in my experience- are actually aware of stuff like: file descriptors, text-encoding, heap, stack, timezones, or even sterr/stdin/stdout or the tty in general, just to name a few. 


They might be "aware" but not with enough understanding. 

I'm witnessing a generation proficient with NextJS, but not much interaction with Node (the runtime). Heck, I've seen some devs unable to start a project without a ready-made boilerplate, generator or template, and they're really good "builders". Web developers lacking a good understating around HTTP verbs, statelessness, status codes, routing and url parts, .. yes!

I can't count the number of devs trying to adopt container-based development or dealing with docker-compose, but have no idea about [Linux] namespacing, file permissions, mounting a filesystem, hostnames, or even what the heck is stateless and stateful, mix app and persistence containers. They often run into simple problems their brain literally hang at. 

It might be astonishing to you, the reader, but some developers I came across can't tell the difference between stdout and stderr, or not even aware of exit codes. You might attribute their ignorance to being "juniors", but I'm afraid to tell it's not always the case. Again, all is my personal findings. 


As a result, entire projects (thus businesses) were built with poor logging (i.e. no log rotation, blocking log writes .. ), poor time handling, let alone timezones, long/heavy processing when handling HTTP request (then stuck at handling timeouts), unbound memory consumption with garbage collected languages, but they learnt not to bother with free-ing up resources, it's taken care of. Oh and CI is broken


it's sad, and tiring. 


It's not always about learning new things, it's more important to know on what ground you're standing on. Deep understanding, dense little circle of knowledge is often times better than a sparse and bigger one. 


I might be biased, but I always find myself converging to pushing people to learn Unix/Linux, stay patient and tinker with its casual problems. Some of the best developers I've worked with are actually devops, not full stack, not backend, not frontend, and maybe mobile/embedded devs. And some of the best devops are actually Linux/Unix gurus, and scripting/automation wizards. 


So, my advice to young developers, please stick to tinkering whenever possible. You won't learn much reading the User Manual of your TV, only when you actually tinker with it you become a master.

Building and shipping makes you a good developer, tinkering makes you a great problem solver. 

However, as a builder, you might be easily disposable. As a problem solver, you're valuable. 
