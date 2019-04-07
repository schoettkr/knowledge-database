+++
title = "Databases & Webtechnology - Lecture 01"
author = ["eo shiru"]
date = 2019-04-03
lastmod = 2019-04-07T10:03:42+02:00
tags = ["uni", "db-web"]
draft = false
+++

### Organizational {#organizational}

Tutorial Dates: 09.04, 16.04, 23.04, 30.04, 07.05, 14.05, 20.05 (Project Start), 23.06 (Project End), 01.07-12.07 (Project Presentation ~15 minutes)


## IT-Architectures {#it-architectures}

Throughout times there where different architectures favored or dominant. In the beginning there were mostly _monolithic_ architectures. Mainframes are an example of monolithic architecture and are nowadays used primarily by large organizations for critical applications; bulk data processing, such as census, industry and consumer statistics, enterprise resource planning; and transaction processing. Advantages of such systems are the high availability and the processing power, while the cost and required operating are the main disadvantages.

The development of _intelligent terminals_ as clients and _backend machines_ as server resulted in a turn away from mainframes to a **dyadic** (zweiteilig) **architecture** (Client/Server). This is how PCs came around whith the advantage of being more affordable and still being powerful. Disadvantages at the time were beingt dependant on few manufacturers when it comes to software and hardware.

Then there is also the **multi-tier** **architecture** (often _three tier_) which allows excellent hard- and software specialization as well as connecting legacy systems with modern technologies (eg IDMS with the Internet). This comes at the price of increasing communication costs and possible over-specialization via a lot of tiers/layers.


## Server Webtechnology {#server-webtechnology}


### External Programs - Seperate Process {#external-programs-seperate-process}

**CGI** or Common Gateway Interface offers a standard protocol for web servers to execute programs (separate process) that are running on a server to generate web pages dynamically. Such programs that generate web pages are knows as _CGI scripts_ or simply as _CGIs_. The specifics of how the script is executed by the server are determined by the server. In the common case, a CGI script executes at the time a request is made and generates HTML. This allows the _dynamic_ creation of HTML in comparison to just transmitting static HTML that already lies on the server. This is how it typically goes:

1.  Client sends request to webserver, eg requesting the name of an entry in a wiki
2.  the Web Server executes the CGI (needed GET or POST parameter usually become env vars)
3.  the CGI program generates the content, eg retrieves the source of that requested entry's page and transforms it into HTML
4.  the CGI program sends the answer/output to the webserver via standard output
5.  CGI program/process stops
6.  web server receives the input from the CGI and transmits it to the user agent

| Pro                                                                 | Contra                                            |
|---------------------------------------------------------------------|---------------------------------------------------|
| independant of programing language                                  | process management (process for each request)     |
| architectural freedom (single/multithreading)                       | database usage (each process needs db connection) |
| security (webserver is not modified, communication via standard IO) | speed                                             |
|                                                                     |                                                   |

**FastCGI** is a binary protocol for interfacing interactive programs with a web server. It is a variation on the earlier Common Gateway Interface (CGI). FastCGI's main aim is to reduce the overhead related to interfacing between web server and CGI programs, allowing a server to handle more web page requests per unit of time.<br />
The "one new process per request" model of CGI programs makes them very simple to implement, but limits efficiency and scalability. At high loads, the operating system overhead for process creation and destruction becomes significant. Also, the CGI process model limits resource reuse methods, such as reusing database connections, in-memory caching, etc. Instead of creating a new process for each request, FastCGI uses persistent processes to handle a series of requests. These processes are owned by the FastCGI server, not the web server.<br />
To service an incoming request, the web server sends environment variable information and the page request to a FastCGI process over either a Unix domain socket, a named pipe, or a Transmission Control Protocol (TCP) connection. Responses are returned from the process to the web server over the same connection, and the web server then delivers that response to the end user. The connection may be closed at the end of a response, but both web server and FastCGI service processes persist.<br />
Each individual FastCGI process can handle many requests over its lifetime, thereby avoiding the overhead of per-request process creation and termination. Processing multiple requests concurrently can be done in several ways: by using one connection with internal multiplexing (i.e., multiple requests over one connection); by using multiple connections; or by a mix of these methods. Multiple FastCGI servers can be configured, increasing stability and scalability. When using FastCGI the webserver is only responsible for recieving the request, direct them to the FastCGI process and sending the results to the client.

| Pro                                      | Contra                             |
|------------------------------------------|------------------------------------|
| process management (process stays alive) | communication efforts may increase |
| database management                      |                                    |
| security                                 |                                    |


### External Programs - Same Process {#external-programs-same-process}

**BGI** or Binary Gateway Interface is a serverside, external module that runs in the same process as the webserver. A BGI script is a stand-alone (eigenstaendig) module that gets loaded into the process space of the server and is therefore _not running_ as a _separate process_. The webserver recieves the requests and forwards them to the BGI module by calling the modules functions. As an active part of the server process these functions than manage the transmission with the database and may only possibly (nur evtl) the processing of the requested data. After the data has been processed the webserver sends the data to the user agent.

| Pro                                          | Contra                                                                    |
|----------------------------------------------|---------------------------------------------------------------------------|
| speed (modules stay in server process space) | security (illegitimidate actions may be performed via harmful parameters) |
| database management                          |                                                                           |
| task separation                              |                                                                           |


### Verdict on external serverside Programs {#verdict-on-external-serverside-programs}

-   CGI
    -   secure, slow, modest maintenance
    -   appropiate when few database requests are foreseeable
-   FastCGI
    -   faster than CGI, more secure than BGI
-   BGI
    -   fast and easily maintained
    -   server dependant because of its API character
    -   problematic in regards to security


### Server Extensions (Erweiterungen) - SSI {#server-extensions--erweiterungen--ssi}

**SSI** or or Server Side Include is a technique that is used for including/inserting the content of one or more files into a webpage (HTML document) on a web server via a `#include` directive.
Typically the content that is to be inserted is stored on the server. The web server then interprets `include` directive in HTML documents and inserts the content of the specified file into the HTML document. This could commonly be a common piece of code throughout a site, such as a page header, a page footer and a navigation menu. SSI also contains control directives for conditional features and directives for calling expernal programs. In order for a web server to recognize an SSI-enabled HTML file and therefore carry out these instructions, either the filename should end with a special extension, by default .shtml, .stm, .shtm, or, if the server is configured to allow this, set the execution bit of the file.

1.  Client requests content that contains include directives from the webserver
2.  Webserver gathers the content and starts to "create" the webpage
3.  Webserver encounters include directive and reads the required files/content
4.  Webserver replaces the include directive with the just read content
5.  Webserver finishes generating the webpage
6.  Webpage is sent to the Client

**Pro:**

-   Reusability
    -   text blocks / boilerplate (like Copyrights, Signatures etc) can be inserted into HTML docs
    -   reoccuring functionality can be enabled by this technique (eg via Java Server Pages)
        -   Zur Ausführung von Code ist diese Technik nicht geeignet! Sie stellt lediglich eine Voraussetzung dafür dar
-   Simplicity
    -   für Stereotype gut einsetzbar bei Webservern ohne Sprachinterpreter
    -   the principle of inserting data/files is possible for arbitrary files and contents

**Contra:**

-   Server Load / Burden
    -   the server has to carry the burden of inserting and executing the contents
    -   this applies especially for JSP


### Server Extensions (Erweiterungen) - Scripts {#server-extensions--erweiterungen--scripts}

Server-side scripting is a technique used in web development which involves employing scripts on a web server which produce a response customized for each user's (client's) request to the website. A alternative is for the web server itself to deliver a static web page. Scripts can be written in any of a number of server-side scripting languages that are availabl, but require the appropiate interpreter for the scripting language to be installed on the server. Server-side scripting is distinguished from client-side scripting where embedded scripts, such as JavaScript, are run client-side in a web browser, but both techniques are often used together.
In the Java world this is also called Java-Servlets or serverside applets. In this case the executing machine is the JavaVM which in combination with the Servlet-Engine serves as the runtime environment. This is the basic method of operation: The instructions that the webserver should execute are integrated into the HTML document with special tags. The webserver interprets these directives or passes them onto eg the JavaVM so that the instructions are executed. This allows execution of the whole application logic:

1.  Client requests content that contains script directive
2.  Webserver gathers content and starts document creation
3.  Webserver encounters script tags (`<%...%>`) and hands on the content inside of the tags to a language interpreter
4.  After the instructions are executed the webserver replaces the script tags and their content with the result yielded by interpreting the instructions
5.  Content creation gets finished
6.  Content is sent to the client

**Pro:**

-   Flexibility/Dynamik
    -   text blocks, database requests, user requests can easily be inlined into HTML documents
-   slides: Contemporary (zeitgemäß... _no comment by me :D_)
-   Java (_rofl_ how is this a plus XD)
    -   portable, independant applications can be realized via servlets
-   Server Load / Burden
    -   the webserver has to withstand an even higher load than in the case of SSI


### Verdict on serverside Extensions {#verdict-on-serverside-extensions}

-   SSI
    -   simple basic technique
    -   really suitable for usage with files _without code_ on every webserver
        -   together with files including code only suitable for webservers that have the appropiate environment for executing the code
    -   maybe a lot of separate files in addition to the actual contents
-   Scripts
    -   simple handling without additional files
    -   including language interpreters result in bulkier webservers
    -   possibly delayed response / content delivery because of the higher server load


## Summary {#summary}


### Programming Effort {#programming-effort}

-   low to none for SSI when used for stereotypes (files w/o code), when used with JSP it gets more complicated tho
-   medium for CGI because the applications are usually ought to be kept small
-   high for scripts because the extent of the application is usually larger
-   very high for BGI (when creating APIs) because users are offered a lot of functionality


### Server Dependence {#server-dependence}

-   none for CGI because the programs are independant and communication happens solely via standard IO
-   low for SSI because almost all servers can interpret include directives
-   very high for BGI (APIs) because they have to be suited to the server type
-   very high for scripts because the server has to have the appropiate environment (interpreter etc)


### Programming Language Dependence {#programming-language-dependence}

-   low for SSI and CGI because independant
    -   Servlets and JSP are an exception for SSI because then the dependence is really high on Java
-   medium for BGI (die daraus resultiert, dass es möglich sein muss, mit der verwendeten Programmiersprache dynamische Bibliotheken zu erstellen)
-   very high for scripts because of reliance on the interpreter and language environment


### Security {#security}

-   low for BGI because there a multiple ways to cause harm especially since the execution happens direclty in the process space of the server
-   medium for scripts because they're sandboxed, but there are still ways to circumvent this security mechanism
-   high for CGI and SSI (in case of stereotypes) because of the high independency of these techniques


### Server Load {#server-load}

-   low for SSI because it does not required a lot to include files
-   medium for BGI because special task are executed but no programm logic
-   high for CGI and scripts because of the extensive programm execution
