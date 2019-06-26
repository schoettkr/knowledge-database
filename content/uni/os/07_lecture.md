+++
title = "Operating Systems - Efficient Memory Management"
author = ["eo shiru"]
date = 2019-05-15T10:00:00+02:00
lastmod = 2019-06-26T10:02:11+02:00
tags = ["uni", "os"]
draft = false
+++

## Virtualization and Caching {#virtualization-and-caching}

Hierarchy of storages:<br />
![](/knowledge-database/images/storage-hierarchy.png)

-   in this chapter we deal with the interaction of these hierarchies


### Locality of reference (Lokalitaetsprinzip) {#locality-of-reference--lokalitaetsprinzip}

The storage hierarchy is based on the principle of locality. The principle of locality states that only a small part of the address space \\(A\\) is accessed in small intervalls \\(\delta t\\) and that there is a tendency of a processor to access the same set of memory locations repetitively over a short period of time.<br />
"When address \\(a\\) is accessed it is likely that an address nearby to \\(a\\) will be accessed".

-   eg von-Neumann-principle where programs are executed sequentially (next instruction is nearby to the current instruction)

"When address \\(a\\) is accessed it is likely that \\(a\\) will be accessed in the near future again".

-   eg when a variable gets written, it probably will be read or written again soon

Commonly both of these principals apply, for example in the case of arrays.


### Caching vs Virtualization {#caching-vs-virtualization}

-   programer/user of a storage hierachy usually does not see all layers (some are transparent)
-   accessing layer \\(k\\)
    -   de facto access of layer \\(k-1\\) &rarr; caching
    -   de facto access of layer \\(k+1\\) &rarr; virtualization
-   caching makes access _faster_ and virtualization _increases_ the _capacity_

{{< figure src="/knowledge-database/images/caching-vs-virt.png" >}}


### Volatile vs Non-Volatile Memory {#volatile-vs-non-volatile-memory}

-   fluechtier (volatile) vs permament (non-volatile) Speicher
-   the mediums used at the top of the storage hierarchy are _volatile_ while the the bottom layers are _non-volatile_
    -   the mediums are used accordingly for example for program variables (volatile) and persistant data (non-volatile)
-   problem: caching & virtualization lead to a blending of these separations
    -   eg unwanted retention/storing, unexpected loss

{{< figure src="/knowledge-database/images/vola-non-vola.png" >}}


### Caching and the OS {#caching-and-the-os}

-   caching of primary storage happens directly by the hardware
-   the OS may need to configure the caching
    -   which storage locations/areas are covered by the cache
    -   what is the writing policy (write through vs write back)
    -   how does the cache behave in case of concurrent access
-   if the OS interfers in the memory mapping (common in case of strewing mapping), the caches may need to be invalidated (may apply the TLB as well)
-   caching of the lower storage hierarchy layers needs to be handled by the OS
-   transportation of data & instructions between memory, cache and CPU is directly handled by the hardware
-   accessing the harddisk is a task of the OS
-   reading/writing from/to archive storage is either directly initiated by the use or automatically by the OS (filesystem)

{{< figure src="/knowledge-database/images/storage-hierarchy-2.png" >}}


## Virtualization {#virtualization}

-   primary memory is often not sufficient for all processes
-   therefore there are multiple approaches to use more storage space from the storage hierarchy (harddrive)
    -   **swapping** = a process only runs when it is completely in primary memory; proceses as a whole get swapped in and out of memory repeatedly (eg early UNIX)
    -   **memory overlay** = processes do their own memory management and swap parts of their storage in and out of files
    -   **demand paging** = processes run even if they're only partly in primary storage (almost all modern OS)


### Demand Paging {#demand-paging}

-   demand paging is state of the art
-   idea: automated conjunction of _strewing mapping_ (streuender Adressierung) und _storage hierarchy_
-   not needed parts of storage are outsourced to the harddrive and insourced on demand
-   for users/programers
    -   transparent
    -   capacity limitations of memory gets replaced by the capacity limitations of the swap out device (practically unlimited)
    -   this unlimited storage is however just _virtually_ existant

**Datastrucutes for Demand Paging**<br />

-   absence of a memory page needs to be **automatically** recognized by the hardware
-   access to non-existant (or unavailable) page triggers a trap &rarr; page fault
-   management requires additional information
    -   page in primary storage?
        -   &rarr; presence bit (not for inverteded page table)
    -   frame occupied/available?
        -   &rarr; occupation bit (only for inverted page table)
    -   documentation of page access?
        -   &rarr; reference information (maybe just a bit; reference bit)
    -   page modified?
        -   &rarr; dirty bit
    -   possibly (access) rights
    -   possibly TLB configuration information

Page table for virtual memory where each process typicaly has an own page table (&rarr; uniform memory layout from the CPU pov)<br />
![](/knowledge-database/images/page-table-virt.png)<br />
![](/knowledge-database/images/pte-x86.png)<br />
![](/knowledge-database/images/page-fault.png)

**Concurrency**<br />

-   page swapping requires I/O (&rarr; **time intensive**)
    -   should be avoided; only modified pages should be written
-   avoid unneccessary blocking by increasing the concurrency
    -   ISR (interrupt service routine; interrupt handler) of the page fault does not handle the (synchronous) harddrive access
    -   ISR blocks the triggering process and wakes the _page swapper_
    -   the page swapper (own process itself) organizes the page swapping &rarr; asynchronous interaction with the hardware
        -   depending on the strategy not only the triggering page is replaced but maybe also more (principle of locality)
    -   because page faults usually occur fitfully (stossweise) the concurrency may be increased by a _frame emptier_ (Frameleerer) which organizes a reserve of empty page frames


### Selection Strategies {#selection-strategies}

-   page fault rate depends i.a. on which pages are kept in primary memory and which are outsourced
-   global vs local view: are only pages of the page fault triggering process viewed or all pages?
    -   local view
        -   concept of working sets: max amount of pages which can be accessed by a process without a page fault
        -   determing the size of working-sets may require its own strategy
-   policy (Ersetzungstrategie): which selection minimizes the amount of page faults?
-   global/local view and policy are orthogonal


#### Random Selection (Zufaellige Auswahl) {#random-selection--zufaellige-auswahl}

-   hereinafter we use the following example to evaluate different selection strategies
    -   storage size: 3 page frame
    -   reference sequence: 0 &rarr; 2  &rarr; 4 &rarr; 3 &rarr; 2 &rarr; 3 &rarr; 0  &rarr; 1 &rarr; 0 &rarr; 3 &rarr; 0  &rarr; 2

{{< figure src="/knowledge-database/images/random-selection.png" >}}

-   7 page swaps/replacements, 10 page faults
-   result could be worse or better, but how much better could it maximally be?


#### Optimal Selection Strategy {#optimal-selection-strategy}

-   goal of an optimal strategy: the amount of page fault should be minimized for the respective reference sequence
-   perfect oracle \rigtarrow remove the page which will not be needed for the longest time

{{< figure src="/knowledge-database/images/optimal-strategy.png" >}}

-   3 page replacements/swaps
-   6 page faults
-   not feasible because knowledge of the future is required
-   but can be used as a a-posteriori-comparison for other strategies
-   feasible strategies use principle of locality and use past values to estimate the future


#### Fifo {#fifo}

-   pages are swapped-in and -out in the same order

{{< figure src="/knowledge-database/images/fifo-strategy.png" >}}

-   4 page swaps
-   7 page faults
-   problem: anomaly
    -   amount of page swaps should monotonously fall for increasing amount of page frames &rarr; this does not apply to the FIFO strategy:

{{< figure src="/knowledge-database/images/fifo-anomaly.png" >}}


#### LFU (Least Frequently Used) {#lfu--least-frequently-used}

-   LFU algorithm has a counter for all pages, which is incremented for every access
-   pages with least count are removed/swapped out

{{< figure src="/knowledge-database/images/lfu-strategy.png" >}}

-   4 page swaps
-   7 page faults


#### LRU (Least Recently Used) {#lru--least-recently-used}

-   the page which has not been used for the longest time gets removed
-   possible implementation via queue
    -   page at the beginning of the queue gets replaced
    -   referenced pages are moved back to the end of the queue

{{< figure src="/knowledge-database/images/lru-strategy.png" >}}

-   4 page swaps
-   7 page faults


#### RNU (Recently Not Used) {#rnu--recently-not-used}

-   similar to LRU but with a fixed time window of length \\(k\\) which is set via the reference sequence
-   every page that was not referenced during the time window can be removed
-   \\(k\\) has to be choosen in a way where the amount of RNU pages is small but still \\(>0\\)

Example for \\(k=2\\):<br />
![](/knowledge-database/images/rnu-strategy.png)

-   4 page swaps
-   7 page faults


### Thrashing {#thrashing}

Thrashing occurs when a computer's virtual memory resources are overused, leading to a constant state of paging and page faults, inhibiting most application-level processing. This causes the performance of the computer to degrade or collapse. The situation can continue indefinitely until either the user closes some running applications or the active processes free up additional virtual memory resources. The system is busy swapping pages and doesn't get to do productive work.


#### Measures to overcome the Thrashing Effect {#measures-to-overcome-the-thrashing-effect}

-   **better program profiles** &rarr; increase locality
    -   eg alot of small loops instead of a few large loops
    -   eg work on large matrices row-wise instead of column-wise (also depends on memory layout)
-   **measures on the OS level**
    -   faster storage (eg SSD)
    -   efficient organization of swapping files (centre of the drive)
    -   reduction of processes with memory requirements/demands (swapping instead of paging, "atmende" working sets)


## Examples in concrete Operating Systems {#examples-in-concrete-operating-systems}


### Memory Managment in UNIX {#memory-managment-in-unix}

-   early UNIX systems _without virtual memory_ but with _swapping_ &rarr; memory as a resource was managed via suspending/suspensions (Verdraengung)
-   processes were completely swapped out to the drive when..
    -   .. there was no more space to create processes
    -   .. a dynamic memory allocation request couldn't be fulfilled
-   criteria for swapping:
    -   state &rarr; favor to swap out blocking processes
    -   priority and time spent in memory
        -   priority and time since the process was swapped in are added
        -   process with highest value is swapped out
-   management via First-Fit


#### Demand Paging in UNIX {#demand-paging-in-unix}

-   todays UNIX systems use demand paging with an additional page frame emptieer (page daemon)
-   global view on the memory
-   swapping strategy: **Second Chance Algorithm**
    -   if a page is accessed a reference bit is set in the page table
    -   clock hand ("Uhrzeiger") cyclically runs through all page frames
    -   the reference bit gets reseted for pages where it is set
    -   a page without a set reference bit to begin with is swapped out
    -   approximation of LRU/RNU
-   fallback to swapping when thrashing occurs

In 4.3BSD a modified second-chance algorithm was introduced where 2 hands (Zeiger) were used, one for marking and one for emptying/swapping out. The additional parameter _handspread_ (Abstand der Zeiger) can be adapted to the situation.<br />
![](/knowledge-database/images/second-chance-algorithm.png)


#### Fork via Copy-On-Write {#fork-via-copy-on-write}

-   **copy-on-write** = the child process created via the `fork()` system call inherits the state of the parent process
-   user pov: copying of a memory area / address space

Implementation:

-   only the page table is copied
-   page table entries of parents and childs are marked as read only
-   MMU yields access error on writing access
-   OS treats/handles error, creates a real copy and then repeats the action


### Memory Managment in Windows {#memory-managment-in-windows}

-   local page swap strategy &rarr; **Working Set**
-   working sets of the processes are adapted in accordance to page fault rate and total amount of page frames
    -   little (wenig) page faults &rarr; _working set trimming_
    -   lot page faults &rarr; _working set expansion_
    -   system wide maximum is calculated on startup (MmMaximumWorkingSetSize)
-   swap in and swap out auf Kosten des Seitenfehler verursachenden Prozesses
-   treatment/handling of page faults in the context of the violating process (modus switch)
-   a process always starts with an empty working set
    -   page fault on access
    -   gets filled over time when the process is running
    -   only data that is really needed lays in primary memory
-   _read-ahead_ mechanisms = read in more pages than required
-   use LRU-schema when trimming the working set
-   kernel has its own system working set
    -   includes global cache of the file system
    -   kernel code and data (incl drivers) can be swapped out


### Frame Managment in Windows {#frame-managment-in-windows}

-   unused frames are managed via global FIFO lists
    -   **free page list** = available frames, not filled with 0
    -   **modified page list** = modified pages/frames, removed from working set
    -   **standby page list** = unmodified pages/frames, removed from working set
    -   **zero page list** = available frames, filled with 0
    -   **bad page list** = frame didnt withstand hardware test

-   system-wide optimization by adjusting the working sets of single processes
-   new tiles (Kacheln) for processes from the Free Page List or the Zero Page List (depending on the use case)
-   _Soft Page Fault_ = frame goes from the standby or modified list back into the working set (no I/O operations needed)
