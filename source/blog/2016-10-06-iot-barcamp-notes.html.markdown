---
title: IoT Barcamp Notes
date: 2016-10-06 11:26 UTC
tags: meetup
author: Andreas
authortwitter: aschmidt75
---

On Tuesday, 4th October, Oliver and I participated in our first German bar camp
in D&uuml;sseldorf on the hype topic Internet of Things (IoT): [www.iotbarcamp.de](http://www.iotbarcamp.de/).
I would like to share my personal notes on the talks I heard and my general
impressions of the camp.

![w100](/img/iotbarcamp_IMG_0249.jpeg)

The venue encompassed the entire floor in a modern office building located
in D&uuml;sseldorf Golzheim, in the heart of [D&uuml;sseldorf's fashion district](https://www.google.de/maps/place/Bennigsen-Platz,+40474+D%C3%BCsseldorf/@51.2465369,6.7680377,17z/data=!3m1!4b1!4m5!3m4!1s0x47b8c9efeef2d93d:0x552bc10c24260333!8m2!3d51.2465369!4d6.7702264]).
It was a large open space with enough room to move around, a number of tables
and chairs for taking breaks or to talk and work together with colleagues.
So far so good.

The day progressed according to the time schedule starting with an introduction
by the organisers and the opportunity for everyone in the audience to introduce
himself/herself, together with three hashtag-like topics of one's own interests.
With 130 people I was quite worried about how long that would take, but everyone
managed to keep it short.

The introduction round revealed a diverse set of interests around IoT: from business topics
(business models, ventures) through to UX design, platform building, hardware,
radio, smart cities, autonomous vehicles, security, prototyping, LORA.  

Session Planning was quite straight-forward: 23 session submissions, with 20
available slots. After a short debate, two sessions joined, and two more were
placed during the lunch break.

![w100](/img/iotbarcamp_IMG_0252.jpeg)

Sascha Pallenberg’s keynote was intended to be live streamed (because of Sascha
being in Taipeh), but due to technical problems it was recorded and played back.
Sascha showed what "Smart City" really means being in Taipeh: autonoumous underground
trains, NFC payment being the norm and so on. Really interesting, especially when
comparing the situation to - lets say - countries like Germany.
Watch the recording [on the barcamp live page](http://www.iotbarcamp.de/live/), German-only.

I had to hurry to finish my last slides for a session I was to give at 11:00,
about "Data formats for the IoT: Developing for embedded and web".
We researched different data formats and their efficiency with regards to size a
and compared to binary formats. For example, parsing JSON or even XML on an
embedded device can be really hard, whereas parsing the same structure in binary
form can be quite easy to code AND reduces data sizes when transmitting or storing.

I wondered if this topic would fire up discussions in the audience: If it makes
sense, if it makes a difference, whether it is needed because it has already been
solved? I was not disappointed. We had a lively discussion covering a lot of
aspects, so obviously we raised a relevant topic.

You can take a look at [my slides here](https://speakerdeck.com/aschmidt75/iot-barcamp-data-formats-for-the-internet-of-things).
This and other material will be the basis of our upcoming projects, so stay tuned :)

The next session I visited was "IoT Service Kit" presented by [Michael Hufelschulte](https://twitter.com/drbits)
from Cassini. The core idea is to have an offline "playground" to prototype mobile
and IoT solutions. The IoT is not only about software behind a web interface, and
not only about hardware gadgets either: It’s also about technology becoming ubiquitous,
so we should model and design the usage of it as well. The IoT service kit does that,
and Michael invited the audience around a table to design and run through a sample case.

I was also happy to find a session about radio technology: Crowd sourced LPWAN,
LoraWAN and The Things Network,by [@kgbvax](https://twitter.com/kgbvax). I already
knew a bit about LORA and read about the underlying radio technology in the slide
presentation ["Reversing LORA"](http://static1.squarespace.com/static/54cecce7e4b054df1848b5f9/t/57489e6e07eaa0105215dc6c/1464376943218/Reversing-Lora-Knight.pdf))
but I still had some questions, such as what nodes to attach to a gateway, how to
code against it, costs, regulations etc. All answered, nice session!

The last session i visited was "Selecting the right radio technology for IoT".
Lyn Matten from mm1 Technology introduced the various pitfalls that may arise
when selecting a radio technology for your own project, be it security,
energy consumption, regulations, internationalisation etc.
Bottom line: Know your requirements (as always :-) Slides: to be determined.

![w100](/img/iotbarcamp_IMG_0256.jpeg)

I'm still waiting for the session notes and the full list of links to the slides.
I hope they're going to be published by the barcamp team, so i can supply them to you.

Looking forward to the next IoT Barcamp!

Andreas
