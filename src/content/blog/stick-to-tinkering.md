---
author: Walid Ziouche
title: 'Stick to Tinkering'
pubDatetime: 2022-07-18T20:59:39+01:00
draft: false
tags:
    - systems 
    - problem solving
featured: true
description: 'Random thoughts about the importance of understanding the underlying system when coding & developing'

# Social media metadata
# socialImage: '/images/image.jpg'
hackerNewsId: '32621863'
lobstersId: '7tzjgv'
---

If you ask me how I would describe the solo-practice of software engineering, I'd sum it up to 03 skills:

- **Thinking rationally**: The ability to reason about a given problem in abstraction in a logical manner,
- **Conveying intent**: The communication skill with a mental precision to transfer that reasoning to the machine in a way it can process it as expected, 
- **Understanding systems**: This is what's often overlooked. And is what this post is mostly about. 

And I'm saying the "solo" practice because I'm intentionally removing the people/management vector from the equation. 


What got us -developers- to programming and computer science is often times the passion to create something and see it in-action, probably on a screen. You are [self] taught how to fullfil this passion. Little attention is given to where that "something" you build is going to run, be deployed or behave within an environment.

With the rapid development happening in our industry, we've got "layers of abstraction" acting like piles of convenience that shorten the path to that end goal... build something. 

That *convenience* is even advertised. Like "X is being taken care of for you, focus on what matters", "make ship happen", "Forget X, focus on Y". That might lead you to think you're only valuable when you ship, build or help build something. Which is true, especially to businesses. But are you building on solid grounds? Are you correctly investing in yourself this way?

Those layers of abstraction are now packed. Providing entire dev "stacks". Each layer is trying to hide its **underlying system** so "you don't have to deal with it"... Again, not necessarily always harmful or bad, but I do think there are times to pause and reflect, are you mentally challenged or stagnant? are you valuable past a certain build/ship threshold? 


Most developers, juniors but also some experienced ones, that I had the chance to mentor or work with tend to focus on dev stacks, tools, framework, and how to get things done. Often, very few are aware of the underlying operating system, runtime or environment. I came to the surprising conclusion that very few developers -in my experience- are actually aware of stuff like: file descriptors, text-encoding, heap, stack, timezones, even sterr/stdin/stdout or the pipe, just to name a few. 


They might be "aware" but not with enough understanding. 

I'm witnessing a generation proficient with NextJS, but not much interaction with Node, the runtime, or, the system in our case. Heck, I've seen some devs unable to start a project without a ready-made boilerplate, generator or template, and they're really good "builders". Web developers lacking a good understating around HTTP verbs, statelessness, status codes, routing and url parts, .. the behind the scene mechanics of the web!

I can't count the number of devs trying to adopt container-based development or dealing with docker-compose, but have no idea about [Linux] namespacing, file permissions, mounting a filesystem, hostnames, or even what the heck is stateless and stateful, mix app and persistence containers. They often run into simple problems their brain literally hang at. Again, a lack of understanding of the underlying system.

It might be astonishing to you, the reader, but some developers I came across can't tell the difference between stdout and stderr, or not even aware of exit codes. You might attribute their ignorance to being "juniors", but I'm afraid to tell it's not always the case. 


Again, all is my personal findings. Very little education is given to teaching this kind of stuff, most "tutorials" are how to build things, or learn new things. Yet I find tinkering with your runtime environment or the underlying system of your app very crucial to debugging and problem solving. It's even inspirational knowing how your OS, or the runtime is solving certain problems or what facilities is offering you to achieve certain tasks.  


As a result, entire projects (thus businesses) were built with poor logging (i.e. no log rotation, blocking log writes .. ), poor time handling, let alone timezones, long/heavy processing when handling HTTP requests (then stuck at handling timeouts), unbound memory consumption - even with garbage collected languages- but they learnt not to bother with free-ing up resources, it's taken care of. Oh and CI is broken, someone introduced an interactive -blocking- process to the pipeline, what the heck is no-tty anyway? 


It's not always about learning new things, it's more important to know on what ground you're standing on. It's worthwhile studying it.

I might be biased, but I always find myself converging to pushing people to learn Unix/Linux, stay patient and tinker with its casual problems. Some of the best developers I've worked with are actually devops, not full stack, not backend, not frontend, maybe mobile/embedded devs. And some of the best devops are actually Linux/Unix gurus, and scripting/automation wizards. 

I personally didn't wait for Linux on the Desktop year, yes I use Arch BTW, [for years](https://meribold.org/2022/08/16/same-arch-linux-installation-for-a-decade/). I just didn't trade the desktop convenience over tinkering, and I'm thankful I auto-chose that path. I can't attribute the number of findings and debugging skills I attribute to Arch, its Wiki, its manual installation process, its rolling release model and having to fix problems without ever formatting, I learned a ton, even when I had peer-pressure pushing me to ditch it for some other alternative "convenience". 

So, my advice to young developers, please stick to tinkering whenever possible. You won't learn much reading the User Manual of your TV, only when you actually tinker with it you become a master. Tinkering will help you connect some dots in your brain that can't be connected otherwise. It increases your "courage", widens your perspective, and teach you how to fearlessly get out some tough situations.

Building and shipping makes you a good developer, tinkering makes you a great problem solver. 

As a builder, you might be easily disposable. As a problem solver, you're always valuable. 
 
- Further recommended reading from Scott Hanselman: [Systems Thinking as important as ever for new coders](https://www.hanselman.com/blog/systems-thinking-as-important-as-ever-for-new-coders)