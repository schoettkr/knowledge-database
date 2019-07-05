+++
title = "Operating Systems - Services, Resources and Devices"
author = ["eo shiru"]
date = 2019-06-12T10:00:00+02:00
lastmod = 2019-07-05T14:10:57+02:00
tags = ["uni", "os"]
draft = false
+++

## Services {#services}

-   services deal with resources
-   we distinguish between active and passive resources
-   services which are not offered by the kernel are realized via processes (or groups of processes) of the OS
    -   this is the majority in a micro kernel system with the exception of process management, IPC and possibly memory management
    -   UNIX concept for such processes are **daemons**
    -   clients utilize service via IPC (usually communication &rarr; via channels)
-   a process can be a client and a server at the same time


### Service - Service Relationship {#service-service-relationship}

{{< figure src="/knowledge-database/images/service-relationship.png" >}}

-   a server usually offers multiple operations that can be called by the client
-   a "job" means the execution of such operation for example
    -   file service: open, close, read, write
    -   name service: resolve, register, delete
-   the server operations can be realized in one or more processes


### Service - Time {#service-time}

-   the service relationship has an influence on the progress
    -   client has to wait (blocked) until the result comes from the server
    -   server is not available to other clients until the next round (limited throughput)
-   improvement via
    -   drifting the communication
    -   buffering
    -   parallelism in the server


#### Service - Drifting {#service-drifting}

-   goal: overlap between client and server activity
-   realization:
    -   dispatch job at the earliest possible point in time
    -   recieve/take back the result at the latest possible point in time
-   that means the job sending operation of the client should "drift forward" and the recieve operation should "drift backwards"


#### Service - Buffering {#service-buffering}

-   instead of having huge jobs it might make sense to divide jobs in smaller chunks &rarr; job processing can start earlier
-   decoupling via buffering
    -   increased parallelism between client and server
    -   avoid blocking
-   analagous for the result
    -   especially useful for services that (directly or indirectly) access I/O devices since there are may occur huge delays
-   besides a higher parallelism there are other goals that can be achieved via buffering:
    -   caching eg buffer for files
    -   changing data semantics eg character oriented I/O at blocked devices and vica versa or terminal modes (raw, cooked, halfcooked)

Different kinds of buffers:<br />
![](/knowledge-database/images/service-buffers.png)


#### Service - Parallelism {#service-parallelism}

-   parallelism between client and server improves the response time at the client (and therefore at the user)
-   we look at 3 approaches:
    -   cloning
    -   pipelining
    -   multiplexing
-   besides that there's also the option to increase server throughput

-    Service - Parallelism: Cloning

    -   server process gets offered in multiple identical clones
    -   all server processes take their jobs from a shared channel
    -   characteristics:
        -   transparency for the client
        -   overtaking (Ueberholung) is possible
        -   simple to realize

-    Service - Parallelism: Pipelining

    -   server process gets "cut" into multiple subprocesses
    -   there's a buffer between the subprocesses
    -   characteristics:
        -   no overtaking
        -   higher transport expenditure (hoeherer Transportaufwand), inner channels
        -   harder to realize

-    Service - Parallelism: Multiplexing

    -   when there are complex server which for example request sub-jobs from other servers there are multiple wait places
    -   processes gets stuck at recieving point, but could continue work at other places
    -   approach: combine all recieve channels together and react accordingly depending on the message
    -   characteristics:
        -   only one process
        -   arbitrary jobs processed at the same time
        -   difficult to realize


## Resources {#resources}

-   there are two major aspects when it comes to resources:
    -   management: who can access when?
    -   execution: how is the resource used?
-   further classification of resources (besides real/logical and possibly virtual)
    -   **reusable**
        -   resources are usually released after they've been used and can then be used from other processes
    -   **consumable** (verbrauchbar)
        -   some logical resources are consumed via their usage, that means they get created and don't exist any more after they've been used
        -   examples: signals, messages, timestamps


### Resources - Scarcenes (Knappheit) {#resources-scarcenes--knappheit}

-   some resources are unlimited and don't require specific managment
-   it gets interesting when it comes to scarce/limited resources
-   approaches:
    -   (central) **resource manager** &rarr; intermediate instance (zwischengeschaltete Instanz)
        -   eg printer driver
    -   **shared protocol** &rarr; contestants coordinate their access
        -   eg critical sections, decentral bus arbitration, global serialization in distributed systems
    -   **uncoordinated usage** &rarr; collision danger, needs to be detected and repaired
        -   eg MAC access in Ethernet, optimistic transaction

Note: in the case of a central resource manager or when following a common/shared protocol the Coffman criteria might be fulfilled (danger of a deadlock)

-   in case of uncoordinated usage "damage" can happen! &rarr; only use when collision is unlikely and the damage would be reparable


### Resources - Selection (Kandidatenauswahl) {#resources-selection--kandidatenauswahl}

-   problem: from a set/sequence of waiting resource requests one (fulfillable) should be chosen
-   there are different strategies for this, the goals are
    -   full utilization of the resource and/or
    -   fair treatment of the contestants

Be:

-   \\(n\_f(t)\\) amount of free/available resource units at moment \\(t\\)
-   \\(n\_(i)\\) amount of requested units by process \\(P\_i\\)
-   \\(W\_(t)\\) set of waiting request/processes
    -   is a queue where new arrivals/request are put at the back

If \\(n\_f > 1\\) is true for a resource it is a "Mehrexemplarbetriebsmittel" (term seems made up by professor, please let me know the correct English term)


#### Resources - Selection Strategies {#resources-selection-strategies}

**FIFO** (first in first out) / **FCFS** (first come first served)

-   only the currently first request in the queue is handled
-   when \\(n(i) \leq n\_f(t)\\) then \\(n(i)\\) get allocated/occupied for the process \\(P\_i\\), else nothing happens
-   disadvantages:
    -   resource utilization (BM-Auslastung) can be bad when the first request is really large; subsequent smaller - and fulfillable - requests don't get considered

**First Fit Request**

-   the queue is searched through from the start _until_ a request \\(j\\), which is fulfillable ($n(i) &le; n\_f(t)), is found
-   disadvantage:
    -   danger of process starvation for large request

**Best Fit Request**

-   the request wich minimizes the free rest capacity gets selected
-   the queue gets searched through completely and then the request \\(j\\) gets chosen, for which the following is minimal:<br />

\\[j\_{\text{sel}} = j, j \in W(t), n(j) \leq n\_f(t), n\_f(t) - n(j) = min\_k (n\_f(t) - n(k))\\]

-   disadvantage:
    -   danger of process starvation for large request

After a release of a large amount of resource units it might be possible that multiple requests are fulfillable at the same time. Because of that the above strategies can be applied iteratively (again and again and again,..) until no further occupations are possible

-   to reduce the effort one can limit this to a window of size \\(L\\), which means that only the first \\(L\\) positions in the queue are considered

{{< figure src="/knowledge-database/images/selection-window.png" >}}


#### Resources - Starvation {#resources-starvation}

-   starving of large request (first fit, best fit) can be avoided by using a dynamic window size
-   \\(L{\text{max}}\\) be the maximum window size (initial value)
-   then when a successfull occupation happens the window size \\(L\\) is modified in the following way:

{{< figure src="/knowledge-database/images/resource-window-mod.png" >}}

After the first request has been passed over \\(L\_{\text{max}} - 1\\) times, the window size will be \\(L = 1\\) so the first request _has to be handled_. For \\(L \rightarrow 1\\) Best-Fit and First-Fit converge to FCFS.

See pages 21 to 27 for extensive examples of selection strategies!


## I/O Devices {#i-o-devices}

-   real/physical resources are usually for input/output
    -   exceptions: CPU, memory
-   usage mainly has two tasks:
    -   **transport**: exchange/copy data between the central (CPU, memory) and the device
    -   **control/management**: set/read conditions/parameters/states for the transport eg set transfer rate, position the write/read head, ...


### I/O Devices - Software Requirements {#i-o-devices-software-requirements}

-   **abstraction** = usage should be uniform across device types (eg harddrive and CD)
-   parallelism/synchronicity = while asynchronous usage increases efficiency, synchronicity is prefered because users have a synchronous view and it simplifies the programming (see 11.2 for examples)
-   error handling = errors should be handled as close to the device as possible &rarr; transparency


### I/O Devices - Types of data transfer {#i-o-devices-types-of-data-transfer}

**Synchronous I/O**

-   user programm uses a system call
-   call is handled by the kernel and converted to the according procedure call on the (device) driver
-   the driver executes the input/output (possibly with polling and multiplexing) and returns/jumps back
-   OS kernel yields control back to the user program
-   _disadvantage_: inefficient and therefoure rarely used

**Asynchronous I/O**

-   driver instruct the device with the task and waits for an interrupt on finish
-   the interrupt is created from the device controller
-   OS does other things in the mean time
-   _disadvantage_: possibly too many interrupt (eg per printed character)

**I/O Processors** (direct memory access, DMA)

-   a special processor (DMA chip on device) controls the data flow
-   CPU initializes the processor and hands him the I/O task
-   when the chip is done he creates an interrupt
-   _disadvantage_: increased efforts on hardware side

{{< figure src="/knowledge-database/images/sync-io.png" >}}


### I/O Control {#i-o-control}

-   **trigger** (how does the device gets its tasks?)
    -   CPU needs to load control register of the control unit
        -   type of operation (eg read, write)
        -   source
        -   target
-   **reaction** (feedback/response to CPU)
    -   after finishing the I/O the CPU needs to be informed
        -   synchronous I/O: CPU occasionally checks the control register of the control unit &rarr; **polling**
        -   asynchronous I/O + DMA:  CPU is informed via an **interrupt**

Typical structure (simplified) of a simple driver process:<br />


### I/O Devices - Interrupt Problem {#i-o-devices-interrupt-problem}

-   as we've seen interrupts are widely used
    -   preemptive scheduling
    -   I/O
    -   traps (eg memory fault)
-   interrupts are **fully asynchronous**, which means that they can happen at any times also _while_ an interrupt is getting handled &rarr; problem
    -   blocking &rarr; can lead to data and flow inconsistencies
    -   nested interrupts &rarr; requires resumable code
    -   delayed (sequential) interrupts &rarr; increased control efforts (Verwaltungsaufwand)

slides: Bei der Programmierung von ISR ist besondere Sorgfalt nötig!

-   Kontamination von Zuständen vermeiden ➡ kritische Zustände

retten

-   Lange ISR vermeiden ➡ Parallelisierung
-   Typische Compileroptimierungen können schaden (z.B. sollten Variablen explizit volatile sein)


### I/O Devices - Error Handling {#i-o-devices-error-handling}

-   Bei der Verlaufsanalyse wird vor allem überprüft, ob Fehler aufgetreten sind und ggf. darauf reagiert
-   three types of errors:
    -   **delaying** (verzoegernd) &rarr; can be fixed with help of user (eg no paper in printer, no usb stick)
    -   **stochastically** &rarr; random disturbances which can be fixed via redundance (repeating the action), eg timeout, collision
    -   **definite/final** (endgueltig) &rarr; not recoverable, job/task has to be cancelled (eg no operating voltage, unknown adress)
