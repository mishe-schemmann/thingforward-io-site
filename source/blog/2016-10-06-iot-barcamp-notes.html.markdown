---
title: IoT Barcamp Notes
date: 2016-10-06 11:26 UTC
tags: meetup
---

Last Tuesday, 4th of October, Germany had its first bar camp regarding the
hype topic Internet of Things or short: IoT. Oliver and i visited this
[IoTcamp](www.iotbarcamp.de), located in D&uuml;sseldorf and this blog post
is about my personal notes on the talks that i visited and the impressions
i had.

The venue was a complete floor in a modern office building located
in (D&uuml;sseldorf Golzheim)[https://www.google.de/maps/place/Bennigsen-Platz,+40474+D%C3%BCsseldorf/@51.2465369,6.7680377,17z/data=!3m1!4b1!4m5!3m4!1s0x47b8c9efeef2d93d:0x552bc10c24260333!8m2!3d51.2465369!4d6.7702264], D&uuml;sseldorfs fashion quarter with all the fashions showrooms.
The floor offered a quite large conference and exhibition space, so there
was enough room around, with many tables and chairs to rest, to talk to
or work together with colleagues. All nice, so far, so good.

  The timeline worked out pretty well as planned. First up, the introduction
  of the organizers, followed by a round of introductions of the audience. Yes,
  everyone in the audience had the chance to introduce himself/herself, together
  with three hashtag-like topics of one's own interests. Given that there were
  about 130 people, i feared that it could possibly get out of hand, but
  everyone managed to keep short.

  To me, this round of introductions revealed that interests about the Internet
  of Things are quite diversified: From business topics (business models, ventures)
  over UX design, to platform builders and finally hardware and radio enthusiasts.
  Smart city, autonoumous vehicles, security, prototying, LORA: many interesting
  topics, so i was really curious about the upcoming talks.

  Session Planning was quite straight-forward: 23 session submissions, with 20
  slots being available. After a short debate, two sessions joined, and
  two more were placed in the time slots of lunch, and it all fit on the
  time table:

  (image)

  Next up: The [keynote of Sascha Pallenberg](http://www.iotbarcamp.de/live/), originally
  intended to be live streamed (because of Sascha being in Taipeh), but due to
  technical problems it has been recorded and played back. Sascha showed what
  "Smart City" really means, being in Taipeh: autonoumous underground trains,
  nfc payment being the norm and so on. Really interesting, especially when comparing
  the situation to - lets say - countries like Germany. Watch the recording [on the barcamp live page](http://www.iotbarcamp.de/live/),
  but german-only.

  I had to hurry to finish my last slides, because i was to give a session starting
  at 1100h, about Data formats for the IoT. Developing for Embedded and Web,
  we invested some time in research on data formats, and their efficiency regarding
  size, compared to binary formats. For example, parsing JSON or even XML on
  an embedded device can be really hard, where, on the other hand, parsing
  the same structure in binary form can be quite easy to code AND reduces
  data sizes when transmitting or storing.

I wondered if this topic is going to fire up discussions in the audience, if
it makes sense, if it makes a difference, wether it is needed or not, because
it has already been solved a.s.o., and actually it did! Quite lively discussion covering
a lot of aspects, so we're confident to have raised a relevant topic.

Please find [the slides on my speakerdeck](https://speakerdeck.com/aschmidt75/iot-barcamp-data-formats-for-the-internet-of-things).
This and other material will be the basis of our upcoming projects, so stay
tuned :)

The next session i visited was "IoT Service Kit" from Michael Hufelschulte of Cassini.
The core idea is to have an offline "playground" to prototype mobile and IoT solutions.
The IoT is not only about software behind a web interface, not only about hardware gadgets either:
Its also about technology becoming ubiquitous, so we should model and design the usage
of it as well. The IoT service kit does that, and Michael invited the audience around
a table to design and run through a use case.

I was happy to find a session about radio technology as well: Crowd sourced LPWAN, LoraWAN and The Things Network,
by [@kgbvax](https://twitter.com/kgbvax). I already knew a bit about LORA, read about the underlying
radio technology (from [these fantastic slides, "Reversing LORA"](http://static1.squarespace.com/static/54cecce7e4b054df1848b5f9/t/57489e6e07eaa0105215dc6c/1464376943218/Reversing-Lora-Knight.pdf))
but still had some questions, such as what nodes to attach to a gateway, how to code, costs, regulations etc. All answered, nice session!

Last session i visited: "Selecting the right radio technology for IoT". Lyn Matten of mm1 Technology introduced
to various pitfalls that may arise when selecting a radio technology for your own project, may it be
security, energy consumption, regulations, internationalization etc. Bottom line: Know your requirements (as always :-)
Slides: to be determined.

I'm still waiting for the session notes and the full list of links to the slides. I hope they're going to
be published by the barcamp team, so i can supply you with a link to it.

Looking forward to the next Iot Barcamp!
