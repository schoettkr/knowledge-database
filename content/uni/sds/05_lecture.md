+++
title = "Security of Distributed Software - Lecture 05"
author = ["eo shiru"]
date = 2019-04-30
lastmod = 2019-06-28T11:25:53+02:00
tags = ["uni", "security-ds"]
draft = false
+++

## SSL/TLS {#ssl-tls}

Wiki: Transport Layer Security (TLS), and its now-deprecated predecessor, Secure Sockets Layer (SSL), are cryptographic protocols designed to provide communications security over a computer network. Several versions of the protocols find widespread use in applications such as web browsing, email, instant messaging, and voice over IP (VoIP). Websites can use TLS to secure all communications between their servers and web browsers.

The TLS protocol aims primarily to provide privacy and data integrity between two or more communicating computer applications.[2]:3 When secured by TLS, connections between a client (e.g., a web browser) and a server (e.g., wikipedia.org) should have one or more of the following properties:

-   The connection is private (or secure) because symmetric cryptography is used to encrypt the data transmitted. The keys for this symmetric encryption are generated uniquely for each connection and are based on a shared secret that was negotiated at the start of the session (see § TLS handshake). The server and client negotiate the details of which encryption algorithm and cryptographic keys to use before the first byte of data is transmitted (see § Algorithm below). The negotiation of a shared secret is both secure (the negotiated secret is unavailable to eavesdroppers and cannot be obtained, even by an attacker who places themselves in the middle of the connection) and reliable (no attacker can modify the communications during the negotiation without being detected).
-   The identity of the communicating parties can be authenticated using public-key cryptography. This authentication can be made optional, but is generally required for at least one of the parties (typically the server).
-   The connection is reliable because each message transmitted includes a message integrity check using a message authentication code to prevent undetected loss or alteration of the data during transmission.

In the OSI-model SSL/TLS belongs in layer 6, in the TCP/IP model it belongs above the transport layer (ie TCP,..) and below the application layer (ie HTTP,..).

The SSL/TLS Architecture basically consists of of 2 protocol layers:<br />
![](/knowledge-database/images/ssl-layer.png)


### Record Protocol (Layer 1) {#record-protocol--layer-1}

-   represents the lower level of the TLS protocol
-   encapsulation of exchanged messages
-   decomposition into blocks for transmission
-   end-to-end encryption
    -   symmetric algorithms
    -   see the following handshake protocol
-   integrity and authenticity are ensured by cryptographic checksums

{{< figure src="/knowledge-database/images/record-protocol.png" >}}


### Handshake Protocol (Layer 2) {#handshake-protocol--layer-2}

-   server and client decide on:
    -   mode of encryption
    -   type of message authentication
    -   secret key
-   authentication via certificates is possible

![](/knowledge-database/images/handshake-protocol-1.png)
![](/knowledge-database/images/handshake-protocol-2.png)


### Change Cipher Spec Protocol (Layer 2) {#change-cipher-spec-protocol--layer-2}

-   change to the negotiated cipher suite
-   cipher suite identifies a combination of four algorithms
    -   key exchange
    -   authentication
    -   hash function
    -   encryption


### Alert Protocol (Layer 2) {#alert-protocol--layer-2}

-   signaling on error states
-   protocol defines two fields:
    -   level of error alert
        -   warning
        -   fatal &rarr; connection is immediately interrupted
    -   type of error alert
        -   detailed error description


### Application Data Protocol (Layer 2) {#application-data-protocol--layer-2}

-   pass application data transparently
    -   without consideration of its content
-   based on security parameters data is...
    -   fragmented
    -   compressed
    -   protected
    -   encrypted
