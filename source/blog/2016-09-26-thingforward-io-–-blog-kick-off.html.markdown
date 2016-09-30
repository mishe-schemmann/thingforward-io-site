---
title: ThingForward.io – Blog Kick-off
date: 2016-09-26 00:00 UTC
tags: general
author: Andreas
authortwitter: aschmidt75
---


Every blog has to start with a first post, so this one kicks off our blog journey into the software development of IoT – bringing together software development patterns from the Internet world and the world of Things!

We really started this journey back in 2014 when we started designing and developing software for small, embedded devices. It was like a trip back in time. The majority of embedded IoT devices we came across were really “constrained”: small, 32-bit MCUs with only a few hundred KBs or at most 1-2 MBs of Flash, and sometimes even less SRAM to work with.

Given the equipment and configuration of chip modules, using C or C++ was and still is the primary way to program these devices. Of course there are a number of projects aiming to bring scripted languages such as JS and Python in the pipeline, but coding for professional products at the moment is still most likely done in plain C or C++. However, when it comes to optimization (whether for speed or battery consumption), the results can be quite sub-standard. It reminds me of the phrase “security by obscurity”, where you hide things with the aim of making your solution more secure - because nobody understands how to access it. As you might imagine, this is not very fruitful. When I look at highly optimized C code, I can see a lot of similarities.

Unfortunately, this kind of optimization makes a number of things worse, i.e., when writing unit tests for your code, or even when trying to integrate new team members to the stack. Coming from the software development for the web world, we were used to having fully automated build and integration testing systems, with code using third party libraries from the OSS sphere, each one with its documentation and unit tests and so forth.
Now, developing for the embedded world, we find ourselves once again thrown back to an ancient IT era, where many things still need to be done manually.  

Our primary concern is about communication within the Internet of Things. In this (probably exponentially) growing network, devices need to be able to communicate with each other. This not only includes the capabilities to “speak” the same radio protocol (WiFi, ZigBee, LoRa, etc), but also touches on other aspects of the software application layer: Protocols and Data Formats, covering layers 5 to 7 of the OSI stack.

Our aim is to improve software development for the latter aspects. We feel that IoT solutions need to be open and compatible when it comes to data communication. We want to give IoT developers and IoT architects valuable tools that help them simplify their embedded projects and thus speed up their development lifecycles.

In this blog we’re going to explore technical issues around embedded software development, testing devices, creating service layers and using embedded protocols and data formats. We hope we’re able to deliver valuable assets for IoT and embedded developers. Let us know what you think!

Best,
Andreas and Oliver
