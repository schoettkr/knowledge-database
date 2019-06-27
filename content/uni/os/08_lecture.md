+++
title = "Operating Systems - Interprocess Communication"
author = ["eo shiru"]
date = 2019-05-22T10:00:00+02:00
lastmod = 2019-06-27T11:39:13+02:00
tags = ["uni", "os"]
draft = false
+++

## Introduction {#introduction}

Processes are usually a part of more complex systems. Therefore they sometimes have to exchange data between each other (inter process communication, IPC). Operations to achieve inter process communication are besides process management one of the major tasks of an OS kernel.<br />
There's a functional and a timely aspect when it comes to inter process communication. In this chapter we will cover the functional aspect and the timely aspect will be covered in the next chapter.<br />
Inter process communication can happen in two basic forms:

-   communication
-   cooperation

{{< figure src="/knowledge-database/images/ipc-forms.png" >}}


## Commmunication {#commmunication}

-   communication requires another abstraction &rarr; channel/canal (Kanal)
-   a channel is a data object that provides the operations `send` and `recieve` which each take the address of a container (Behaelter) as their argument
-   there are different variants of passing the message:
    -   value passing: the message ist directly put into the container (two copy operations, only synchronously)
    -   reference passing: address of the message is put into the container (one copy operations)
-   container passing: parts of the sender address space, which contain the message are inserted into the recievers' address space (does not require copying)

Process communication does not involve _time semantics_ at first - it is asynchronously &rarr; sender and reciever can execute their operations at arbitrary times. This results in the following two possible scenarios:

1.  First send, then recieve
    -   message or message address is stored in the channel/container and can then be retrieved when recieving
2.  First recieve, then send
    -   address of target/destination buffer is stored, so that the message then can be copied to that place

In either case there's a intermediate storage (Zwischenspeicherung) in the channel. Also in reality sending and recieving operations are usually coupled with synchronization (see Chapter 9 / next post).<br />
![](/knowledge-database/images/ipc-time-aspect.png)

When a channel is used for multiple communication procedures (Kommunikationsvorgaenge) there has to be the _capacity_ for multiple messages (this is the default case). Variants:

-   dynamic size &rarr; requires memory management
-   fixed size &rarr; semantic has to be clear in the case of an overflow
    -   possible semantics:
        -   oldest message decays
        -   recent message decays
        -   requires synchronization (chapter 09) &rarr; blocking


#### Communication Variants {#communication-variants}

-   Versuchendes Empfangen (polling/probing recieving) = occasionally checking _if_ a message is present &rarr; if that is the case, the message gets copied, if not nothing happens
-   Umlenkendes Senden/Empfangen = after successfully transmitting a message, the commmunication partner gets interrupted and redirected to a special program piece
    -   first arriving process stores a redirect address besides the container/channel address
    -   when the partner arrives the message is copied and the process gets redirected to the specified address

A channel is an sovereing communication object and can exist independently of any senders and recievers. Sometimes it makes sense though to assign a channel to a process. This can happen on the sender or the reciever side:

-   Kanal eines Prozesses, in dem er alle seine ausgehenden Nachrichten ablegt: Ausgangstor
-   Kanal eines Prozesses, in dem alle eingehenden Nachrichten ablegt werden: Eingangstor (port)

Eingangstore sind n:1-Kanäle, Ausgangstore 1:n-Kanäle

Ports are the most commonly used communication objects in modern operating systems.<br />
![](/knowledge-database/images/ipc-binding.png)


## Cooperation {#cooperation}

Cooperation happens when multiple proccesses access the same data which can be for example shared memory or shared files. In contrast to _communication_ the abstraction (eg memory) already exists "naturally" and doesn't need to be created (like a channel for communication). Only the common ground (Gemeinsamkeit) might need to be established.


### Cooperation - Shared Memory {#cooperation-shared-memory}

Shared memory between multiple threads of a main process is nothing special and doesn't require any special measures. Shared memory between multiple distinct processes has to be explicitly requested (system call) (slides: Realisierung über streuende Adressübersetzung).


### Cooperation - Shared Files {#cooperation-shared-files}

Many operating systems support _memory mapped files_. A memory-mapped file is a segment of virtual memory that has been assigned a direct byte-for-byte correlation with some portion of a file or file-like resource. This resource is typically a file that is physically present on disk. Once present, this correlation between the file and the memory space permits applications to treat the mapped portion as if it were primary memory.

-   operations with pointers and variables directly on the file data/content
-   reading/writing of the file is handled by the Kernels memory management
-   usually used for code
-   the resource doesnt need to be a file but can also be a device, shared memory object, or other resource that the operating system can reference through a file descriptor
-   can be used to realize shared memory when eg two processes hook in the same shared file


### Cooperation - Problems {#cooperation-problems}

Data inconsistencies may occur when cooperating because the reception of data is not directly or indirectly initiated by the recipient.<br />
![](/knowledge-database/images/cooperation-problem.png)

-   inconsistencies on concurrent access is one of the main error sources in thread programming


## IPC Examples {#ipc-examples}


### Pipes & Named Pipes {#pipes-and-named-pipes}

-   originally from UNIX and nowadays in many operating systems
-   unidirectional communication between exactly two processes
-   limited capacity
-   synchronization:
    -   when the pipe is full, the sending (writing) process is blocked
    -   when the pipe is empty, the recieving (reading) process is blocked
    -   a pipe is a "bounded-buffer" between two processes

```C
#include <sys/wait.h>
#include <stdio.h>
#include <unistd.h>

int main (int argc , char ** argv) {
  int fd [2];
  pid_t pid ;
  char send_msg [12]= "Hello World";
  char recv_msg [12];

  pipe(fd);
  pid = fork();

  if (pid > 0) { /* parent */
    write(fd[1], send_msg, 12);
    waitpid (pid, NULL, 0);
  } else { /* child */
    read (fd[0], recv_msg, 12);
    printf("Got data : %s\n", recv_msg);
  }

  return (0);
}
```

```text
Got data : Hello World
```

Pipes `|` are also pretty commonly used in the shell. What happens is that an anonymous pipe is created and `fork()` is called for the 'left' program (STDOUT on the write descriptor) and the 'right' program (STDIN on the read descriptor) eg in `cat data.txt | grep term`.<br />
Named pipes can communicate in both directions and are named via a path in the file system.


### Unix Signals {#unix-signals}

Signals are one of the oldest inter-process communication methods used by Unix systems. They are used to signal asynchronous events to one or more processes. A signal could be generated by a keyboard interrupt or an error condition such as the process attempting to access a non-existent location in its virtual memory. Signals are also used by the shells to signal job control commands to their child processes.<br />
There are a set of defined signals that the kernel can generate or that can be generated by other processes in the system, provided that they have the correct privileges. You can list a system's set of signals using the kill command (`kill -l`).

-   terminal, kernel or process send a signal &rarr; `raise()` system call or `kill` command
-   current execution is interrupted and jump to a special function the _signal handler_
-   default implementation (default action) is part of the C library
-   all signals except SIGKILL and SIGSTOP can be programatically ignored

```C
#include <stdio.h>
#include <unistd.h> // sleep
#include <signal.h>

void handle_signal (int signal) {
  printf ("\nGot it !\n");
}

int main () {
  struct sigaction sa = {
                         .sa_handler = handle_signal ,
                         .sa_flags = SA_RESTART };

  sigaction(SIGUSR1, &sa , NULL);

  while (1) {
    printf(".");
    fflush(stdout);
    sleep(1);
  }
}
```


### Sockets {#sockets}

-   platform independent API to program communicating applications
-   _socket_ as a virtual file is requested from the application
-   original description in RFC147 (today as Berkeley Sockets)
-   local endpoint of a channel (address + port)

{{< figure src="/knowledge-database/images/icp-socket.png" >}}

-   see slides 26+27 (chapter 08) for an example socket implementation
