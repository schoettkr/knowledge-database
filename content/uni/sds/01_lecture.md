+++
title = "Security of Distributed Software - Lecture 01"
author = ["eo shiru"]
date = 2019-04-02
lastmod = 2019-04-26T20:59:31+02:00
tags = ["uni", "security-ds"]
draft = false
+++

This was the first lecture in the new summer semester and therefore it was mostly about organizational stuff. Here are the most important points:

-   the exam is gonna be a written one and might be open-book
-   the start of the tutorials will be announced on the course website sometime in April (registration via opal)
-   there are gonna be exercises to hand in
    -   those can be found [here](https://bildungsportal.sachsen.de/opal/auth/RepositoryEntry/19946340368/CourseNode/86516925533323)

Distributed Solution Design<br />
![](/knowledge-database/images/distributed-solution-design.png)

The increasing decentralization of public networks by deregulation of telecommunications markets leads to increasingly extensive use of the Web and increasing usage of the open & decentralized Internet. Back then networks were mostly closed and managed centrally, while the Internet was used as a pure research and didn't have worthwile targets. Because of the recent evolution of the internet and the world wide web security mechanisms are becoming an indispensable part of modern communication systems. Security must be considered in a comprehensive & integrated way, taking new aspect into account: identity and privacy.<br />
But what is **security**? Security refers to the ability to avoid being harmed by any risk, danger or threat (Cambridge Dictionary of English). In practice and regarding IT infrastructure this is an _unreachable goal_. Therefore your software is never a 100% secure.


## Security Goals {#security-goals}

Our focus is on actions to achieve security goals.<br />
Mnemonic for security goals: "**CIA**":

-   Confidentiality
    -   data secrecy
-   Integrity
    -   data intactness
-   Authenticity
    -   secure data origin
-   additional (soon-to-be) major goals:
    -   Liability (Non-Repudiability)
        -   non-repudiation (repudiation = Zurückweisung, Nichtanerkennung) of data origin
        -   important for contracts or in the fight against SPAM
    -   Identity
        -   verification of an individual entity
        -   nowadays identity is of increasing significance

In this lecture the generic term **Assets** denotes things worth protecting eg data or services (business applications for example).

A strong physical security is the foundation to protecting assets and achieving the security goals. Physical vs Digital Security:<br />
![](/knowledge-database/images/physical-digital-security.png)
