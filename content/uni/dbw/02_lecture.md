+++
title = "Databases & Webtechnology - Lecture 02"
author = ["eo shiru"]
date = 2019-04-10
lastmod = 2019-04-11T16:47:25+02:00
tags = ["uni", "db-web"]
draft = false
+++

## Client Webtechnology {#client-webtechnology}

In the last lecture we looked at different kinds of server-side webtechnology. In this lecture we turn our attention to client-side webtechnology.


## 1. Persistent Code {#1-dot-persistent-code}


### 1.1 External applications / Tools (Hilfsprogramme) {#1-dot-1-external-applications-tools--hilfsprogramme}

External tools enable the display of non-HTML data / files and is (analogous to CGI on server-side) independant of the client process. External tools/application are full standalone programs that are called by the browser (client) to display data that's not HTML. These applications run seperately from the browser and have their own functionality. Therefore they can also be executed without a browser. Examples of external applications would be Acrobat Reader, a Mail Client or a Torrent Downloader.<br />
Basically the browser recieves some content and then has two options if the data cannot by displayed by the browser itself:

-   start the application that is registered for the type which is noted in the MIME header of the file and let it process the data (eg display of PDFs back when browsers couldnt do that)
-   the browser offers to download the data in form of a file

**Pro:**

-   universal
    -   almost all data is processable in some way because external applications offer this functionality
-   security
    -   no interference with the browser or server, because external programs/processes are active

**Contra:**

-   platform dependence
    -   applications need to be installed and executable from/on the client side


### 1.2 Plugins {#1-dot-2-plugins}

Plugins facilitate clientside support to process/display data which is otherwise not supported by the client itself. These plugins run (similar to BGI on serverside) in the same process space as the client (-> the browser).<br />
Plugins are separate programs that are loaded by the browser (client) and run in the same process, to process data that is not supported natively. These plugins run in the browser and are therefore not executable without the browser.

-   client recieves data for which MIME type a plugin is registered
-   client loads plugin that is registered for this MIME type
-   plugin processes the data

**Pro:**

-   functionality
    -   it is possible to process varying kinds of data
-   convenience
    -   plugins are installed once and require no care from then on (usually)

**Contra:**

-   platform dependence
    -   plugins depend on the computer, OS and client (browser)
-   security
    -   plugins run in the same process as the client/browser (analogous to BGI)


## 2. Temporary Code {#2-dot-temporary-code}


### 2.1 Scripts (interpreted code) {#2-dot-1-scripts--interpreted-code}

Scripts allow the execution of application logic from the internet client (browser) if the browser has an interpreter for that scripting language. The code gets for example embedded in the HTML document and the browser interprets and executes the instructions. Thus it is possible to integrate complex presentation logic into an HTML document.

-   eg `<script>` tag for JavaScript

    ```html
    <script language="JavaScript">
      function makeCookie() {
        document.cookie = Date()
      }
    </script>
    ...
    <body onLoad="makeCookie()"
    ```
-   or `<applet>` for Java classes

    ```html
    <applet code = "TicTacToe.class">
    </applet>
    ```

**Pro:**

-   functionality
    -   the browser functionalilty and capabilities can be extended a lot (eg setting Cookies, verify input etc)
-   platform independence
    -   no machine code (-> ASCII)
-   simplicity
    -   easy and fast to write and (re)use with different apps

**Contra:**

-   version and language dependence
    -   browsers may require a certain version of a language interpreter and respectively have an interpreter included at all (eg JavaVM for execution of applets)


### 2.2 Machine code {#2-dot-2-machine-code}

It is possible to execute machine code on the client side when there's an appropiate environment (temporarly but also permanently possible). With a special `<object>` tag the machine code object can be integrated with the appropiate environment into the clientside process. In the world of Microsoft this is usually realized via ActiveX by packaging machine code objects into activeX containers which are then loaded into Internet Explorer. As a diference to client scripts, the executable objects kind of bring their execution environment (Ausführungsumgebung) with them.

-   client scans recieved data for `<object>` tags
-   the code is loaded into the clients process and executed

**Pro:**

-   functionality
    -   the browser functionality can be largely extended
-   complexity
    -   a comprehensive system concept together with the MS component model

**Contra:**

-   vendor dependence
    -   browsers have to be ActiveX compatible wich is Microsoft technology


#### Verdict on temporary code {#verdict-on-temporary-code}

Scripts:

-   suitable for eg reoccuring validations
-   platform independant but clients need appropiate interpreter
-   security is not always guaranteed despite the sandbox principle

Machine code:

-   can extend functionality just like scripts
-   more complex because of the execution environment
-   but more flexible because of the container principle (component model) where the clients only need a "docking mechanism" for the containers (COM) or alternatively plugin capabilities to include execution environments and dont require a language interpreter


### 3. Summary of clientside webtechnologies {#3-dot-summary-of-clientside-webtechnologies}


#### 3.1 Programming Effort {#3-dot-1-programming-effort}

-   no/low effort for plug-ins because these usually get bought (very high tho when written)
-   medium for scripts (scope of application is usually not that large)
-   very high for applets and machine code (complex technique and usually used for more complex application)


#### 3.2 OS Dependence {#3-dot-2-os-dependence}

-   no for scripts because everything is done by the browser/client (of course browser depends on OS)
-   low to mediuem for applets because they're mostly

Okay...sorry I have to interrupt here. I stayed quiet until now but the information on the slides is such Bullshit at some points... I'll skip the rest of this great "summary" because I cba anymore to write this shit down :D. Here's another image from the slides<br />

{{< figure src="/knowledge-database/images/webtechnik-eignung.png" >}}

So since there is no written exam in this course but a practical project, I won't be posting about this course anymore since it is not worth it and the slides are just..no comment :D..
