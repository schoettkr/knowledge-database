+++
title = "Operating Systems - Operating System Design"
author = ["eo shiru"]
date = 2019-04-04T10:00:00+02:00
lastmod = 2019-05-01T11:38:06+02:00
tags = ["uni", "os"]
draft = false
+++

Complex systems consist of single component. Therefore the successfull design of a complex system requires knowledge about the variants and relations of the components.<br />
There are some fundamental, reoccuring concepts when it comes to operating systems:

-   **virtualization**
    -   refers to the act of creating a virtual (rather than actual) version of something, including virtual computer hardware platforms, storage devices, and computer network resources
    -   the things "created" by virtualizing often don't exist (or not the required amount or in the quality)
    -   for example relevant for processes, memory space
-   **concurrency**
    -   ability of different parts or units of a program, algorithm, or problem to be executed out-of-order or in partial order, without affecting the final outcome
    -   allows for parallel execution of the concurrent units, which can significantly improve overall speed of the execution in multi-processor and multi-core systems
    -   allows execution without a linear order
    -   for example relevant for threads, IPC, resource management
-   **persistence**
    -   refers to the characteristic of state that outlives the process that created it
    -   achieved in practice by storing the state as data in computer data storage
    -   for example relevant for file systems, files, nonvolatile (persistent) storage

This course will mainly deal with virtualization and concurrency since persistence is a subject of the database modules.


## 1.) Design Principles {#1-dot--design-principles}

Best practices apply to the general design of systems and discuss typical design principles. At times best practices are violated, which is acceptable if there's a justified reason.


### 1.1) KISS {#1-dot-1-kiss}

-   "keep it small and simple" or "keep it simple, stupid"
-   Occam’s Razor: „Plurality should not be assumed without necessity.“
    -   &rarr; _simpler solutions are more likely to be correct than complex ones_
-   A. Einstein: „Everything should be made as simple as possible, but no simpler"
-   Antoine de Saint-Exupery: „Perfection [in design] is achieved not when there is nothing more to add, but rather when there is nothing more to take away.“


### 1.2) Modularity {#1-dot-2-modularity}

A system is subdivides into smaller parts called modules, so that:

-   the interaction **in** a module is **high**
-   the interaction **between** a module is **low**
-   the **interfaces** between modules is **simple**
-   the modules are managable in regards to their size and complexity

Interaction refers to control flow and the exchange of data (information flow).
Modularization can be applied hierarchially, for example in a tree-like organization of similar elements to create scalability and control of complexity. For example this windows device tree:<br />
![](/knowledge-database/images/windows-device-tree.png)


### 1.3) Layering {#1-dot-3-layering}

-   modularization via seperating the functionality a system into layers, where
    -   more simple and universally functionality are layered at the bottom
    -   more complex and specific functionality is layered at the top
-   each layer abstracts lower layers
-   each layer has an interface that allows usage by higher layers

Exemplified by the IP stack:<br />
![](/knowledge-database/images/ip-stack-layers.png)


### 1.4) End-to-end Principle {#1-dot-4-end-to-end-principle}

„A function of service should be carried out within a general layer only if it is needed by all clients of that layer and if it can be completely implemented in that layer.“<br />
![](/knowledge-database/images/e2e-principle.png)

The "hour-glass architecture" visualizes this principle:<br />
![](/knowledge-database/images/hourglass.png)

In case of the internet there could be different applications at the top of the hour glass (diversity) which all adhere to the internet protocol (uniformity) to connect to different networks (diversity). Transfering this to operating systems there could also be different applications at the top, a operating system API in the middle (uniform) and different hardware at the bottom (divers).<br />
Take for example the Portable Operating System Interface (POSIX) which offers an uniform API for UNIX-like systems and allows compatibility and portability of eg source code.


### 1.5) Principle of separating policy and mechanism {#1-dot-5-principle-of-separating-policy-and-mechanism}

-   to have portable/neutral functionality sometimes compromises are needed
-   for example lower layers may offer mechanisms or functionality which then has to be parameterized in higher layers with regards to the specific application
    -   e.g `clone()` in Linux or `CreateProcess()` in Windows
-   separation of policy (_what_) and mechanism (_how_)


### 1.6) Orthogonality {#1-dot-6-orthogonality}

-   functionalities of an operating systems should be independant of each other
-   every component should incorporate orthogonal design criteria, which in this case means free combinability (freie Kombinierbarkeit)


### 1.7) SSOT / SPOT {#1-dot-7-ssot-spot}

-   SSOT = Single Source of Truth or SPOT = Single Point of Truth
-   is the practice of structuring information models and associated data schema such that every data element is stored exactly once
-   any possible linkages to this data element are by reference only
-   code: functionality is implement once
-   data: every information has only one represantion
-   applying this principle prevents inconsistency


## 2.) Architectures {#2-dot--architectures}

An operating systems manages resources. Resources can be divided/allocated in regards to time and space (eg processor cycles, storage, bandwith). This management is generally just code as well. But is is some sort of special code because

-   it has the priviliges to "control" applications and hardware
-   arranges and assigns resources

The interaction of \\(\textbf{Prozessor - Operating System - Resource - Program}\\) is realized via the concept of a **"process"**.<br />
From a conceptual perspective processes do not exist in the hardware. Something has to provide processes and support their interaction. This "something" is called the **kernel** of an Operating System and it provides the essential **infrastrucute for processes**.<br />
In a first, highlevel outline of operating systems we divide the address space (virtual memory) between the:

-   **user space** (slides: Prozessbereich?) for the running processes
    -   area of virtual memory where user processes run
-   **kernel space** (Kernbereich)
    -   area of virtual memory where kernel processes run
    -   offers infrastructure for processes
    -   OS, background processes, kernel extensions and most device driver run here

Which exactly belongs in the kernel and what not depends on the architecture. For example in (some?) Windows the graphic system runs in the Kernel, in Linux usually not


### 2.1) Monolithic System {#2-dot-1-monolithic-system}

{{< figure src="/knowledge-database/images/monolithic-os.png" >}}

-   no strict separation between applications and operating system
-   operating system as a library
-   suited for small static system (zB Echtzeitbereich)


### 2.2) Monolithic Kernel {#2-dot-2-monolithic-kernel}

{{< figure src="/knowledge-database/images/monolithic-os-kernel.png" >}}

-   operating system architecture where the entire operating system is working in kernel space
-   clear separation (protection) between applications and OS
-   no protection between kernel components internally
-   eg Windows till Windows ME and early Linux
-   speed advantages over microkernel but also more error prone because a crash of a part crashes the whole system


### 2.3) Microkernel (μ-kernel) {#2-dot-3-microkernel--μ-kernel}

{{< figure src="/knowledge-database/images/micro-kerlen.png" >}}

-   as the near-minimum amount of software that can provide the mechanisms needed to implement an operating system
-   mechanisms include low-level address space management, thread management, and inter-process communication
-   kernel contains as litte functionality as possible
-   usually limited to the process concept and their interaction
-   eg MkLinux, Minix


### 2.4) Hybrid Kernel {#2-dot-4-hybrid-kernel}

{{< figure src="/knowledge-database/images/hybrid-kernel.png" >}}

-   operating system kernel architecture that attempts to combine aspects and benefits of microkernel and monolithic kernel architectures
-   the idea behind a hybrid kernel is to have a kernel structure similar to that of a microkernel, but to implement that structure in the manner of a monolithic kernel
-   in contrast to a microkernel, all (or nearly all) operating system services in a hybrid kernel are still in kernel space
    -   so there're none of reliability benefits of having services in user space, as with microkernel
        -   but just as with monolithic kernels there there is none of the performance overhead for message passing and context switching between kernel and user mode that normally comes with a microkernel
-   eg Windows since NT/2000, MacOS


## Reference Architecture {#reference-architecture}

A reference architecture in the field of software architecture provides a template solution for an architecture for a particular domain. It also provides a common vocabulary with which to discuss implementations, often with the aim to stress commonality. A reference architecture provides a template, often based on the generalization of a set of solutions. These solutions may have been generalized and structured for the depiction of one or more architecture structures based on the harvesting of a set of patterns that have been observed in a number of successful implementations.<br />
In this course we will (primarily) view a microkernel architecture as a reference architecture which helps from a didactical point of view.<br />
Side note: μ-kernel is a relative term, since the size of a micro kernel can vary a lot:

-   Mach ≈ 350 kByte
-   L4 ≈ 12 kByte

Advantages of Microkernels

-   clear API/interface facilitates a modular structure
-   more security and stability
-   improved flexibility and extensability
-   improved verifiability of critical parts

Disadvantages of Microkernels

-   usually worse performance because of the required interaction between many processes


## 3.) Prozessbereich (User Space?) {#3-dot--prozessbereich--user-space}

Die erste grobe Gliederung zwischen Prozessbereich und Kernel wird nun beginnend mit dem Prozessbereich verfeinert:
![](/knowledge-database/images/userspace.png)


### 3.1) Flow / job Control (Steuerung des Ablaufs) {#3-dot-1-flow-job-control--steuerung-des-ablaufs}

In regards to control management there's typically a distinction between:

-   operations (Bedienung) = interaction between human and system
    -   OS commands
    -   User interfaces (GUI, shell)
    -   window systems (Fenstersysteme)
-   handling (Abwicklung) = define complex jobs for the OS
    -   batch jobs
    -   cron services
    -   shell via pipes, scripts etc

PS this section in the slides really annoyed me because the used terms are so random and nonstandard..omg :D.. my favorite is probably "Betriebsmittel" DFQ :DDD bruh


### 3.2) Services (Dienste) {#3-dot-2-services--dienste}

Further refining the User Space section we come across so called Services. Since many application have similar "wishes" or demands for the infrastructure, there are standardized services of the OS. This is realized via the hourglass principle (see above). Diverse applications make use of uniform services to access diverse things. Sidenote: Service is a term most common in the windows world - a service and a daemon (unix) are generally the same thing.<br />
Each service/daemon provides an interface or connects or at least has a context of a certain system ressource (BETRIEBSMITTEL :D), for example files, windows, network etc. We can distinguish between two kinds of system resources:

-   real, physical system resources
    -   serial interface, graphics card
-   logical system resource
    -   simplify and ease access
    -   eg files or windows
    -   are realized via physical resources

There are also two aspects when it comes to system resources:

1.  resource management (who can use what and when?)
2.  actual resource usage (eg data transfer)

{{< figure src="/knowledge-database/images/resource-management.png" >}}

Here's an outline of the service layers:<br />
![](/knowledge-database/images/service-layer.png)<br />
Every layer (eg operating / Betrieb) can be partitioned eg into Operation Device A, B (Betrieb Geraet A, B, ..) and so on. "Aufwaertsaufrufe" between layers are allowed as long as no cycles occur. Eg layer n-1 calls layer n which calls layer n+1. Layer n+1 then calls layer n but another "part" (partition, therefore no cycle) which calls another part in layer n which then again calls another part in layer n+1.<br />
Inlining the just acquired knowledge about services and resources into the user space overview this is what we get:
![](/knowledge-database/images/userspace-resources.png)<br />


### Kernel Interface {#kernel-interface}

Services are available via _system calls_, here are some UNIX examples of service interacting:

-   resource: process
    -   `fork` start new process
    -   `exit` end process
    -   `kill` send signal to process
-   resource: file
    -   `open` open file
    -   `read/write` read from or write to file
    -   `chmod` change access rights for file
-   resource: directory

    -   `mkdir` create directory
    -   `unlink` remove file name

    A system call is the transition between the user space and the kernel space (slides: Infrastrukturbereich). Wiki: A system call is the programmatic way in which a computer program requests a service from the kernel of the operating system it is executed on. This may include hardware-related services (for example, accessing a hard disk drive), creation and execution of new processes, and communication with integral kernel services such as process scheduling. System calls provide an essential interface between a process and the operating system.<br />

In most systems, system calls can only be made from userspace processes, while in some systems, OS/360 and successors for example, privileged system code also issues system calls.<br />
So system calls offer a controlled way to interact with kernel services. However it is thinkable that there might be bogus or malicious usage of this (for example abuse of resources of other processes). Therefore the kernel has to be protected of ill usage - but the kernel itself is software. The answer to this problem comes from the layer system &rarr; delegation of most safety mesaures to the hardware.


#### Processor modes {#processor-modes}

In any modern operating system, the CPU is actually spending time in two very distinct modes:

-   **kernel mode**
    -   the executing code has complete and unrestricted access to the underlying hardware
    -   code can execute any CPU instruction and reference any memory address
    -   generally reserved for the lowest-level, most trusted functions of the operating system
    -   crashes in kernel mode are catastrophic; they will halt the entire PC
-   **user mode**
    -   the executing code has no ability to _directly_ access hardware or reference memory
    -   code running in user mode must delegate to system APIs to access hardware or memory
    -   due to the protection afforded by this sort of isolation, crashes in user mode are always recoverable
    -   most of the code running on the computer will execute in user mode

These two modes aren't mere labels - they're **enforced** by the **CPU**. Issuing a system call as well as returning from one triggers a mode change / transition between user and kernel mode (is a bit expensive). So a system call should implicate a mode transition in the hardware. A normal function call is not sufficient for this and the concrete implementation differs between hardware and operating systems but typically it goes like this:

1.  user program stores a parameter at a designated location (memory adress, register)
2.  program signals (via interrupt or special instruction) to the system that a system call should be performed
3.  signal leads to mode change
4.  OS evaluates the parameter and executes the appropriate service
5.  results are stored for the user program
6.  another mode change and the application / user program resumes its work

x86 CPU hardware (intel) actually provides four protection rings: 0, 1, 2, and 3. Only rings 0 (Kernel) and 3 (User) are typically used because that's most compatible across different processor models:<br />
![](/knowledge-database/images/protection-rings.png)<br />
The outer rings can only use something from inner rings in a controlled manner. Switching rings is performed via special instructions / interrupts / traps.

Example of `read` in Unix:<br />
![](/knowledge-database/images/read-unix.png)<br />


## 4.) Kernel (Kernel Space) {#4-dot--kernel--kernel-space}

The kernel provides services via interfaces. A typical microkernel consists of the following layers:<br />
![](/knowledge-database/images/microkernel-layers.png)<br />

In the following lectures we will inspects (parts of) these layers more deeply:

-   kernel operations
    -   process management
    -   process interactions (IPC)
-   process switching operation
    -   process abstraction
    -   scheduling
-   data structure operations
-   lists, trees, hashes
-   kernel memory management
    -   linear memory

So inlining all of the mentioned layers into our two side/part OS microarchitecture this is what we get and will assume (if not stated otherwise) for the rest of the course / the following lectures:<br />
![](/knowledge-database/images/user-kernel-space.png)<br />

Nice post from Reddit, explaining and summarising some things about kernels etc:
Most advanced processors have a memory management unit (an MMU). It's a hardware mechanism for limiting the access a program has to memory (be it RAM, or memory-mapped hardware like graphics cards)

It has at least two modes: a "kernel mode" accessed only by the OS, in which the entirety of memory is available; and a "user mode", accessed by regular programs, in which only data that belongs to this program is accessible. While running in user mode, the program won't be able to read data of another program, or anything like that. This is called virtual memory and is essential for providing security (otherwise, programs might be able to read data from other programs).

Periodically, the processor will interrupt the program that is running and pass the control back to the operating system. This is called [preemptive multitasking](<http://en.wikipedia.org/wiki/Preemption%5F(computing)>. The OS can then pass control to some other program. The goal here is to give each program a fair share of processor time. This is accomplished by the process scheduler.

[ Scheduling was a somewhat heated area on Linux some years ago, where Con Kolivas, an amateur kernel hacker, proposed schedulers (the rotating staircase deadline scheduler and later the brain fuck scheduler) that would improve interactivity, but his code was rejected and he left (but eventually a code with similar goals was incorporated in Linux) ]

External events, like pressing the keyboard, moving the mouse, or receiving data from internet will interrupt the running program too, and pass control to the OS. Ever wondered why you can do ctrl+alt+del (or something like Alt+SysRQ+B in Linux) even if the computer seems to have hanged? While the graphical interface might be frozen, keyboard input is delivered by interrupts, and so if the processor is running it's always delivered. (sometimes the kernel is running out of memory or is having some other problem and has a hard time fulfilling user input - but it always gets delivered)

Also, the OS shouldn't be doing a lot of work inside the interrupt. What if another interrupt fires while he is at it? To prevent that, it disables interrupts. Non-urgent bookkeeping code competes for processor time just like regular programs, and have to go through the scheduler.

Anyway, what if a program wants to read a file, or send data to Internet? It must call a system call. It passes control to the operating system, which can do a lot of things. Sometimes, the program asked something that will take a while - for example, asked to read a file, which can take some milliseconds on a hard disk - and the kernel puts it to sleep. In the mean time it can then schedule other programs, and will wake the program when its data arrives.

The other major thing that is missing are drivers. Processors can communicate with the external world by means of pins that carry electrical signals. Those pins are typically connected to a bus like PCI express, which in turn ultimately connects everything to the processor - keyboard, mouse, graphics card, sound card, etc. While the processor handles the low-level aspects of the bus (and the BIOS does some device initialization), it's the kernel that is in charge of actually receiving and sending data to each of those devices. It's sometimes done by using memory-mapping (so the kernel writes and reads from some pre-determined part of memory to communicate to a device), but there are other methods. Each device has its own way to talk with the processor, and the kernel has to learn it all. Because, what use would have a kernel if you couldn't display anything on a monitor and couldn't type with a keyboard?

I didn't touch everything about operating systems, but anyway, interrupts and syscalls bring operating systems to live, drivers makes operating systems useful, and MMUs makes it much easier to write secure operating systems. While you can run Linux without a MMU, for such processors it's more common to write your code without an operating system (those processors are typically called microcontrollers and are used in embedded environments, like a children's toy or controlling an industrial plant - often you don't need a full blown operating system!)

Anyway, notice that I used "operating system" and "kernel" interchangeable here. That's because in computer science terms, they are the same (and when you study operating systems at university, you normally are studying kernels. For example Modern Operating Systems, a classic book on the field, is about kernels). Also, where I said "processor" you could say "CPU".

Another thing: there is also the idea that current kernels are too bloated, and we should move code from the kernel to user programs (what's left is a minimal kernel called microkernel). For example, you could write device drivers and filesystems as regular programs, instead of having them in the kernel. That way, if they crash, they won't bring the whole system down (the blue screen of death of Windows fame happened mostly due to crappy device drivers). The opposite of microkernel is monolithic kernel. The trouble with microkernels is that they are slower, sometimes considerably so.
