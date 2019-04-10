+++
title = "Operating Systems - Introduction"
author = ["eo shiru"]
date = 2019-04-04
lastmod = 2019-04-10T10:05:57+02:00
tags = ["uni", "os"]
draft = false
+++

Although Operating Systems depend on the underlying hardware, there general principles that are common across different operating systems and this is the topic of this course. There are two different ways to "view" an operating system (2 Sichten):

-   **Top Down View** (OS as a virtual machine)
    -   offers an abstract view on the hardware
    -   real hardware characteristics are sort of hidden
    -   take for example the harddrive and the file system:
        -   real machine (hardware): a sequence of data blocks of fixed size
        -   virtual machine: named files of variable size
-   **Bottom Up View** (OS as a resource manager)
    -   OS coordinates the access on CPU, Memory, Storage etc.
    -   time-wise resource management = applications access resources sequentially (eg printer)
    -   spatial (räumlich) resource management = applications access different locations of a resource (eg memory)

Stallings OS Definition: _An operating system is a program that controls the execution of application programs and acts as an interface between applications and the computer hardware._

However it is hard to give a precise definition of an operating system that's why we stick to a pragmatic definition: Everything that aids the execution of applications in a _generic_ way belongs to the OS / is part of the OS.

-   such wide definition would also include middleware and firmwares

_Insert a bit of OS and general Computing History here :D_

-   see the slides for this, not worth to reproduce here imho
