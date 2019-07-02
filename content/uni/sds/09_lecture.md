+++
title = "Security of Distributed Software - Lecture 09"
author = ["eo shiru"]
date = 2019-05-27
lastmod = 2019-07-02T08:37:28+02:00
tags = ["uni", "security-ds"]
draft = false
+++

**Part III: Trustworthy Software Engineering**<br />

Trustworthy Software

-   in Cordis.Europa.Eu security document defined as: Trustworthiness can be seen as software and infrastructure that is secure, reliable and resilient to attacks and operational failures; guaranteeing quality of service; protecting user data; ensuring privacy and providing usable and trusted tools to support the user in his/her security management.
-   trustworthiness needs to be considered from the outset rather than being addressed as add-on feature

So we focus on: Identity & Security By Design (SBD)

-   who is it for?
-   why does it matter?
-   what is it all about?
-   where does it apply?
-   when to apply?
-   how to apply?


## Identity (Chapter 1) {#identity--chapter-1}

Internet as a danger zone in terms of identity

-   what exactly needs to be protected?
-   what should one orient towards?
-   which data is exceptionally worthy of protection?

Security vs Identity

-   for starters: keynote by Dick Hardt at WWW 2007 on "Identity 2.0"
-   speech on identity by Kim Cameron


### Identity - Problem {#identity-problem}

-   Kim Cameron: "The internet was built without a way to know who and what you are connecting to."
-   initial situation:
    -   internet services are left on their own
        -   must provide security &rarr; isolated identity solutions
    -   criminalization of internet
        -   leads to loss of internet's credibility, for example drawback for e-businesses
    -   identity layers are complex
        -   successful attemps, such as SSL and Kerberos - however overall too many different scenarios are required, so agreement is difficult

Possible solution: Identity Metasystem

-   such a system provides confidential support to ensure who is connecting to whom/what on the internet
-   many questions:
    -   who holds the data?
    -   who trusts whom?
    -   what scales?
    -   how does one realize openness to new developments that do not yet exist?


### Identity - Identity in Detail {#identity-identity-in-detail}

-   there are numerous definitions of identity, the lecture is based on Kim Cameron's definition: "_Digital identity is a set of **claims**, which are made by a **digital subject** about self or other subjects._"
    -   digital subject = person or thing (referred or real) in a digital realm that is described or with which one is dealing
        -   "with which one is dealing" = often in the context with request/response model
        -   example digital subject: real persons, devices, resources, rules/policies and relationships between digitial subjects
        -   discussions of the 'subject' term extend into philosophy
    -   claim = claim suggests that something is true, typically something that seems to be controversial or questionable
        -   remark: claim is a relationship between a certain instance, a digital subject and an identity attribute

We must be able to **structure our understanding** of digital identity:

-   we need a way to avoid returning to the **Empty Page** every time we talk about digital identity
-   we need to inform peoples' thinking by teasing apart the factors and dynamics explaining the successes and failures of identity systems since the 1970s
-   we need to develop hypotheses - resulting from observation - that are testable and can be disproved
-   our goals must be pragmatic, bounding our inquiry, with the aim of defining the characteristics of an unifying identity metasystem
-   the "**Laws of Identity**" offer a good way to express this thought
-   beyond mere conversation, the Blogosphere offers us **a crucible**
    -   the concept has been to employ this crucible to _harden and deepen the laws_

These definitions embrace Kerberos, X.509, SAML. They take this problem of the evaluation of the usefulness of a digital identity up to a higher level in the systems sense of multiple layers. These definitions separate the layer of where stuff is communicated from the layer where evaluations are done – a very important step forward.


### Identity - Laws of Identity {#identity-laws-of-identity}

1.  **User Control and Consent**
    -   _digital identity systems must only reveal information identifying a user with the user's consent_
        -   systems need to appeal in their convenience & simplicity
        -   constantly care about users' confidence
            -   requires holistic commitment
            -   user must be cetral to control with respect to which identities are used and which data is made public
            -   system must protect from deception (eg website location and missue)
            -   system must inform the user of possible consequences upon certain action (data sharing, login etc)
            -   the holistic approach must be used as a paradigm in all contexts (eg when logging into a company or a private blog it should always be clear that the user consents to the release of certain)
2.  **Minimal Disclosure for a Constrained Use**
    -   _the solution that discloses the least identifying information and best limits its use is the most stable long term solution_
        -   one should assume that data/information violations are unavoidable
        -   to reduce risks, information use should be checked with respect to 2 strategies: "must be obtained" or "must be saved"
        -   less information implies less value implies less risk
        -   "as little as possible identification information" means:
            -   reduction of linkable information
            -   use of claim transformations
        -   avoid unnecessary information storage for "possible future" use (why should a credit card be stored by the shop?)
        -   the law is closely related to information disasters
3.  **Justifiable Parties**
    -   _Digital identity systems must limit disclosure of identifying information to parties having a necessary and justifiable place in a given identity relationship_
        -   user has to have a clear understanding of whom the information is/will be exchanged with
        -   system itself may not draw conclusions about relationships between subject and parties (eg Microsoft Passport is useful for logging into MSN but why should it know if I login to Google or eBay?)
        -   in which situations are regulatory identities required?
        -   same holds for intermediaries (what should they know to achieve their goal)
        -   all participants must submit statements of how the information will be used
4.  **Directed Identity**
    -   /a unifying identity metasystem must support both "omni-directional" identifiers for public entities and "unidirectional" identifiers for private entities
        -   digital identity should always be viewed in the context of another identity or a set of identities
        -   omni-directional = public entities require "beacons" (publicly known identifier or URI) &rarr; eg websites (URLs) or public devices
        -   uni-directional = private entities (people) require an ability not to be turned into a beacon
            -   they require a unidirectional identifier, which can be used in combination with a trusted beacon (no correlation, eg user-bank interaction)
            -   negative examples: bluetooth and RFID, partially WLAN
5.  **Pluralism of Operators and Technologies**
6.  **Human Integration**
7.  **Consistent Experience Across Contexts**
