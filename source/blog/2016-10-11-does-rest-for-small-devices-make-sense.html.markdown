---
title: Does REST for small devices make sense?
date: 2016-10-11 13:28 UTC
tags: REST, CoAP
author: Andreas
authortwitter: aschmidt75
---

When reading about maker projects, we often find case examples and remoting implementations using subsets of HTTP, i.e. requesting a status page from a RaspberryPi-based Sensor board or sending commands to small actor devices using HTTP-POST. There are quite a few blog posts and articles are about implementing a REST API on those small devices. Given that REST is well-known from the field of web development, we asked ouselves whether it would make sense to transfer REST capabilities to the field of embedded and IoT development.


## From web services to devices services

When internet services offer access methods besides a browser-/HTML-based user interface, they often implement SOAP or apply REST as an architecture pattern. Both methods are about exchanging data and structured information between clients and (web) services in a network. The first, SOAP, is a standardized protocol whereas the latter, REST, is an architectural style and not a protocol.

The REST-style approach has become very popular over the years, so developers often choose a combination of HTTP as a transport, JSON or XML as a format for exchanging data and a mix of characteristics and conventions that make up the RESTful architecture style. What are these characteristics, and can they also be applied to small IoT devices?

But first: What do we mean by "small IoT devices"? A great many blog posts on this topic include SBCs such as the Raspberry Pi and comparable devices. These include multicore-processors capable of running operating systems such as Android, Linux or Windows IoT Core. They also run more or less the same software as found on servers, only with less computing power.
For this post we'd like to concentrate on really small, constrained devices: running on a single core processor, with only Kilobytes or sometimes Megabytes of Flash memory, and definitely not a full operating system such as Linux.

## REST and RESTful

The [Wikipedias article on REST](https://en.wikipedia.org/wiki/Representational_state_transfer) states a number of architectural constraints. If a service fulfils these, it can be considered "RESTful". Maybe our small devices can also be RESTful.

The first constraint regards the **separation between client and server**, and thus the separation of a user interface, i.e. a Web UI. This can also be applied to IoT devices, i.e., when the data from a sensor is sent to a cloud service or when an App on a mobile device requests the triggering of an action on the constrained device. So maybe it's not about a "client" and "server" anymore, but still two (or more) parties exchanging data, and evolving independently of each other (as long as they adhere to the API contract).

Next up are **stateless** and **cacheable** as important constraints for being RESTful. These requirements date back to a time when server-side application used to store a lot of client context data in server sessions, normally for a large number of clients. For constrained devices, this look easy. They simply do not have enough memory and processing capabilities to implement a server-side data storage, instead directly applying changes to physical properties: turning on a light, adjusting temperatures immediately upon request. It is not a problem to design statelessness into device behaviour. Clients can request a state-change on a device, and the device can immediately fulfill this request.

The requirement **layered system** can be fulfilled as well. Quite a number of IoT devices are not directly accessible (i.e., because of non-IP network stacks, different radio technologies, etc.), but are accessible via IoT gateways. So it's the gateway that represents a layer in the system, adding functionality to the overall solution.

Next i'd like to look at the requirements for a **uniform interface**.
The **identification of resources** introduced a fundamental difference to accessing data within a web application. Developers are used to the convention that, for example, in an event store service, all events can be accessed by the URI `/events`, and a single event with id 3512 under `/events/3512`. This applies to devices as well when they are used to control different resources such as sensors and actors. Three LEDs can be accessed by, let's say `/led/1`, `/led/2` and `/led/3`, as a very simple example. Resources are **controlled by representations**, usually in some form of data format. For constrained devices, the choice of a data format is essential, as it is likely they can only supply a single data format, probably a binary one. This is in contrast to web services, which can easily produce and parse multiple representations, i.e., XML or JSON or HTML. So being on a very constrained environment, we'd have to make a choice here.

A very interesting constraint impacts the hypermedia model and brings application state changes into the representation of content. This would apply to a service answer’s requests for  resources with content where further, possible actions are supplied in URLs (i.e. see [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS)).
It would allow a client to navigate through an application without knowledge about the content structure, because it is fully supplied within the hypermedia model.

Constrained devices would also be capable of fulfilling this requirement. CoAP as an IoT protocol allows for the discovery of resources on a device, specified by the Constrained RESTful Environments (CoRE) Link Format, as specified by [RFC6690](https://tools.ietf.org/html/rfc6690).

## REST for constrained devices

Looking at the requirements of REST as an architectural style, we find that constrained devices are able to fulfil those, albeit on a much simpler level than web services do.

At one of the next blog posts, we're going to look at CoAP, the Constrained Application Protocol and see how to implement a REST API on a small device!
