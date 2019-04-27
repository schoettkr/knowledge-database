+++
title = "Security of Distributed Software - Lecture 03"
author = ["eo shiru"]
date = 2019-04-16
lastmod = 2019-04-27T11:12:31+02:00
tags = ["uni", "security-ds"]
draft = false
+++

## Attacks on End Systems {#attacks-on-end-systems}

Attacks on end systems via

-   computer viruses
-   computer worms
-   trojan horses
-   exploits
-   cracking systems

might focus on

-   unsecured computer systems
-   exploiting programming errors
-   bad security measures
-   weak passwords

Computer Virus

-   based on biological model
-   infects resources of the host system to replicate itself
-   malicious functions
    -   load generation
    -   data corruption
    -   spying
-   various types
    -   boot sector viruses
    -   file viruses
    -   macro viruses
    -   script viruses
    -   composites
-   self-defense mechanisms of viruses:
    -   stealth
    -   modification
    -   cryptographic methods
    -   polymorphism
    -   retroviruses (against anti-virus programs)
-   passive distribution: by embedding into other programs and execution by the host system

Computer Worm

-   based on biological model
-   uses resources of the host system and of the network to spread over to other systems _automatically_ in order to execute its malicious function there
-   malicious functions
    -   load generation
    -   data corruption
    -   spying
    -   spamming
    -   DDoS
-   various types
    -   email worms (social worms, file attachment, active content)
    -   interactive worms (ask the user "please press OK" to use exploits)
    -   instant messaging worm (sending of malicious software / links to all chat partners)
    -   IRC worms (usage of scripting in IRC programs)
    -   P2P worms (at file-sharing sites: tempting names &rarr; download)
    -   cell phone worms (distribution via Bluetooth, MMS, etc)
-   often in combination with other forms of malware, eg viruses droppers, backdoors, trojans

Dropper (virus dropper, DDoS dropper)

-   executable program that acts as a carrier program for malware
-   is usually terminated after the virurs has been installed

Injector

-   similar to dropper, but the malware will only be "installed" (injected) in memory

Backdoor

-   part of a program that allows users to gain acces to the machine / system bypassing the normal access security
-   variants: default passwords (BIOS); specially equipped passwords / routines / servers that allow access (sometimes subsequently installed programs)
-   closely linked to trojans and droppers

Trojan (trojan horse)

-   similar to the well-known story..
-   program that executes a potentially harmful function without user's knowledge
-   attention: often mixed up in the context of rootkits and backdoors

Rootkit (administrator toolbox)

-   collection of software tools for concealment and stealth intrusions of malicious software
-   examples: hiding backdoors by hiding processes, logs, log-ins

Exploit

-   a program (including scripts & macros) that exploits the weaknesses or failures of a system or another application to obtain privileges or to use it for DoS attacks

**Malware** as generic term refers to malicious or unwanted software or programs.

Buffer Overflow

-   application reserves a buffer to store some input values
-   the length of the input is larger than the buffer but the whole input still gets processed
-   memory space outside the buffer gets overwritten/accessed


## Attacks on Infrastructures {#attacks-on-infrastructures}

Attacks on infrastructures via

-   attacks on signaling mechanisms
-   distributed denial of service (DDoS)
-   attacks on WLAN hotspots and routers
-   break-ins (password theft, bugs, exploits)

might focus on

-   unsecured intermediate system
-   overload situations
-   unsecure data storage
-   weak passwords

Attacks on signaling mechanisms

-   ICMP: fake control messages
-   RSVP: fake resource allocation

Attacks on router

-   attacks on routing protocols
-   distribution of false routes
-   WLAN, Bluetooth etc

Attacks on Hardware, eg virtual server

-   usb-attacks

Denial of Service Attack

-   the targeted weak spot is the overload of the network component
    -   may result in loss of service or entire computer systems
-   attack possibilities
    -   basic principle: large amount of requests sent to the target service or target system
    -   requests must be designed in a way that they lead to an overload situation (more efficient use of exploits)
-   examples:
    -   ping of death = fake "echo request" information leads to a crash
    -   smurf = broadcasting of an ICMP "echo request" with false return address (address of the victim)
-   special forms
    -   **Distributed** DoS = coordinated attack with a large number of computers
        -   closely linked with trojan / droppers infected systems that can be used as a remote-controlled attack network (BotNets)
        -   BotNets - Malware starts its DDoS attacks after being distributed via a dropper

WLAN Attacks

-   the targeted weakspot is the transmission medium as well as utilizing encryption techniques
-   attack scenario
    -   capture data packets of a protected WIFI network
    -   "attack" on encryption &rarr; search for a key
    -   use the found key for further attacks in the protected network
-   examples: wepcrack, weplab etc.

Break-in

-   the targeted weakspots are routers, proxys, computers and services in a network as well as weak passwords, poor and faulty security mechanisms
-   attack scenarios
    1.  _host scanning_ &rarr; which computer / router / proxy exist in close proximity of the target (broadcasts, routing list, traffic, sniffing, DNSpredict/Google)? &rarr; list of target systems
    2.  _scanning the target system_ &rarr; type of systems (by means of fingerprints, traffic analysis, Google, whois, etc.)  which services (IP/TCP/UDP) are available or vulnerable (portscanning & ICMP etc)
    3.  _attack_ &rarr; exploiting bugs, backdoors, exploits, password scanners/lists, dropper, GoogleHackingDB
    4.  _successfull breach_ &rarr; read password lists, install droppers, backdoors, keyloggers, proxy monitor, rootkit, etc
        -   start attacks from the compromised system
        -   remove traces
-   examples:
    -   GHDB &rarr; default SSID and passwords of WIFI routers
    -   NBTEnum &rarr; search for other Windows systems
    -   Network Monitors &rarr; traffic analysis (eg TTL field observations) with respect to transparent bridges or dangers arising from IDS (not to attract attention)
