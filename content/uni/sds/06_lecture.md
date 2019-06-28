+++
title = "Security of Distributed Software - Lecture 06"
author = ["eo shiru"]
date = 2019-05-06
lastmod = 2019-06-28T13:20:17+02:00
tags = ["uni", "security-ds"]
draft = false
+++

## Authentication {#authentication}


### Introduction {#introduction}

Authentication is the process of verficating if someone is the one who he claims to be. There are different kinds of authenticators:

-   knowledge-based
    -   PIN, passwords
    -   Challenge-Response
-   biometrics
    -   fingerprint, iris, voice, signature, keystroke behavior
-   ownership-based
    -   something that you do not notice, but what is stored on a medium
    -   IDs, magnetic cards, certificates, smart cards
-   multi-factor authentication
    -   combination of different types of authentication
    -   2 Factors: eg deposit card + PIN, credit card + signature, password + PIN sent by SMS
    -   3 Factors: eg password + smart card + fingerprint


#### Knowledge-based Authentication {#knowledge-based-authentication}

Knowledge-based Authentication using passwords

-   Alice agrees with Bob on a secret password p for authentication of Alice to Bob
-   Bob applies a one-way or cryptographic hash function H on the password, and stores the image value H(p)
-   when authenticating the password is send and then gets hashed again and compared to the stored hash


### Kerberos {#kerberos}

Wiki: Kerberos is a computer-network authentication protocol that works on the basis of tickets to allow nodes communicating over a non-secure network to prove their identity to one another in a secure manner.

-   works according to the KDC principle
-   _User_ wants to use a certain service
-   _Client_ is the local Kerberos application
-   _Server_ provides the desired service
-   _Authentication Server (AS)_ is used for primary user authentication
-   Ticket Granting Server (TGS) issues tickets for certain services
-   KDC includes AS and TGS


#### Accreditation {#accreditation}

-   User and his password are provided to the AS
-   TGS and its secret key are also accredited by the AS
-   Server and its secret key are made known to the TGS

See pages 5 - 12 for more on Kerberos


## Identity Information in Directory Services {#identity-information-in-directory-services}

-   directory service is a special 'name service'
-   property-based requests
    -   comparison: full-name DNS request
    -   similar to 'Yellow Pages'
-   OSI X.500 is the 'classical' Directory Service
    -   however the complex 'Directory Access Protocol' (DAP) prevented it from becoming more widespread
-   LDAP = Lightweight Directory Access Protocol
    -   standardized by IETF

Important: See pages 14 - 23
