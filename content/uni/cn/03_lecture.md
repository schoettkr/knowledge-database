+++
title = "Computer Networks - Lecture 03"
author = ["eo shiru"]
date = 2019-04-16
lastmod = 2019-04-27T13:36:26+02:00
tags = ["uni", "computer-networks"]
draft = false
+++

## Basic model of (tele)communication {#basic-model-of--tele--communication}

{{< figure src="/knowledge-database/images/telecommunication.png" >}}

-   participants act either as a _sender_ or _receiver_
-   usage of the service occurs at a special interface (API) and by using a service address point
-   the medium bridges the spatial (raeumlich) distance

The following image shows a layer model which shows the reduction of deficits from bottom to top:<br />
![](/knowledge-database/images/layer-model.png)

There are certain _principles_ to _structure_ communication. These principles are also called **communication protocols** or short **protocols** and define a _set of rules_ which communication between two or more parties has to follow/obey. There are for example human communication protocols like shaking each others hand as a greeting and then there are computer communication protocols like for example TCP/IP protocols/suite.


## Open Systems Interconnection model (OSI model) {#open-systems-interconnection-model--osi-model}

The Open Systems Interconnection model (OSI model) is a **conceptual model** that characterizes and standardizes the communication functions of a telecommunication or computing system without regard to its underlying internal structure and technology. Its goal is the interoperability of diverse communication systems with standard protocols. The model partitions a communication system into abstraction layers. The original version (International Organization for Standardization, 1984) of the model defined seven layers.

A layer serves the layer above it and is served by the layer below it. For example, a layer that provides error-free communications across a network provides the path needed by applications above it, while it calls the next lower layer to send and receive packets that comprise the contents of that path.

The OSI model focuses on recommendations in regards to procedures, semantics and technologies.

There's also the internet reference model also known as Internet protocol suite which combines the layers 5 & 6 of the OSI model into the application layer and unites the OSI layers 1 & 2 into a layer which describes the network access (Network Interface / Data Link).<br />
The Internet protocol suite is a **conceptual model** and set of communications protocols used in the Internet and similar computer networks. It is commonly known as TCP/IP because the foundational protocols in the suite are the Transmission Control Protocol (TCP) and the Internet Protocol (IP). It is occasionally known as the Department of Defense (DoD) model because the development of the networking method was funded by the United States Department of Defense through DARPA.<br />
The Internet protocol suite provides end-to-end data communication specifying how data should be packetized, addressed, transmitted, routed, and received. This functionality is organized into four abstraction layers, which classify all related protocols according to the scope of networking involved. The Internet protocol suite predates the OSI model, a more comprehensive reference framework for general networking systems:<br />
![](/knowledge-database/images/tcpip-vs-osi.png)

Mnemonics for OSI layers:

-   [Application] All
-   [Presentation] People
-   [Session] Should
-   [Transport] Try
-   [Network] New
-   [Data Link] Dr
-   [Physical] Pepper

Services in the OSI Basic Reference Model:<br />
![](/knowledge-database/images/osi-services.png)

I omit the following slides, which are a bunch of diagrams of services and protocols. Go look in the slides for those and maybe print them out for the exam.
