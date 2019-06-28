+++
title = "Operating Systems - Coordination"
author = ["eo shiru"]
date = 2019-05-29T10:00:00+02:00
lastmod = 2019-06-28T10:40:07+02:00
tags = ["uni", "os"]
draft = false
+++

## Introduction {#introduction}

In the last chapter we looked at _communication_ and _cooperation_ to exchange information/data between processes. In this chapter we take a look at the timely aspect &rarr; _coordination_ which is also often called _synchronization_. We focus on coordination between _processes_ as we've already kind of covered coordination in the kernel via suspension etc.


## Elementary Coordination {#elementary-coordination}

Many coordination problems can be solved with a uniform concept &rarr; _signaling_:

-   signaling uses _signals_ to create a sequence relationship (Reihenfolgebeziehung)
-   ATTENTION: this has nothing to do with UNIX-signals!

Example:

-   section _A_ in process _P\_1_ shall run before section _B_ in process _P\_2_
-   Kernel offers the operations `signal` and `swait` (signal wait) for shared/common variable _s_ (`signal` and `swait` are generic names; OS's have different names for this)

{{< figure src="/knowledge-database/images/signaling.png" >}}

-   busy wait for the signal variable `s` wastes processor time by doing nothing
    -   wiki: busy waiting, which occurs when a process frequently polls to determine if it has access to a critical section. This frequent polling robs processing time from other processes
-   in this most simple case (busy wait) a single bit is sufficient as a signaling variable `s`
    -   atomic read and write is guaranteed because the data structure is secured by kernel exclusion
    -   slides: wenn jedoch mit Prozessorfreigabe gearbeitet wird, muss zusätzlich die Identität des wartenden Prozesses gespeichert werden
-   it is possible that multiple processes call `swait` for the same signal
    -   then an already waiting process shall not get "lost"
    -   &rarr; save multiple waiting processes in a queue
-   in the case of multiple waiting processes we can distinguish between two semantics:
    -   group-signaling & group-waiting = _all_ waiting processes recieve the signal
    -   or-waiting = only _one_ process recieves the signal while all others continue waiting


### Symmetric Synchronization {#symmetric-synchronization}

-   symmetric usage of the operations causes _A\_1_ as well as _A\_2_ to be executed before _B\_1_ and _B\_2_

{{< figure src="/knowledge-database/images/symmetric-sync.png" >}}


### Locks {#locks}

If _A_ and _B_ cannot run simultaneously in the following scenario<br />
 ![](/knowledge-database/images/lock_1.png)

-   either A is executed before B or B is executed before A (no overlaping between the two)
-   signaling operations can then be used to secure critical sections
    -   the operations needed for that have own names `lock` and `unlock`
    -   structurally `lock` equals `swait` and `unlock` equals `signal`

{{< figure src="/knowledge-database/images/lock_2.png" >}}


### Discussion {#discussion}

We're now able to realize different temporal orders:

-   before-after
-   simultaneously (synchronization)
-   non-simultaneously


## Capacity {#capacity}

Until now the signal variable was binary. Depending on the capacity this can be extended (scaled) and extensions partly have their own names:

-   signaling &rarr; and/or-signaling
-   synchronization &rarr; barrier
-   lock &rarr; semaphore (Art von Zaehlersperre)


### Or-Signaling {#or-signaling}

-   should the situation arise a signaling object can store multiple signals (usually from different processes)
-   this is called or-signaling (Oder-Signalisieren)
-   as long as there are signals, all `swait` calls return immediately without blocking
-   a binary variable is not sufficient for this anymore &rarr; use counter or similar
-   Note: Or-Waiting and Or-Signaling are different things - the former allows multiple `swait` calls in sequence (without setting a signal in between), the latter allows multiple sequential `signal` calls (without loosing a signal)


### And-Signaling {#and-signaling}

-   there can be more than one process involved to generate a signal
-   in the case of and-signaling the signal(-variable) is only set when enough processes set off a signal (or approved the signaling)
-   count of approving processes has to be known


### Barrier {#barrier}

-   synchronization with multiple participants &rarr; barrier synchronization
-   all processes synchronize at one point
-   processes can only continue executing when all other other processes arrived at the point of synchronization as well

{{< figure src="/knowledge-database/images/barrier.png" >}}

-   other term: group-rendezvous
-   wiki: in parallel computing, a barrier is a type of synchronization method. A barrier for a group of threads or processes in the source code means any thread/process must stop at this point and cannot proceed until all other threads/processes reach this barrier

Example Barrier Implementation:

```C
class BarrierSync {
  private int number = p ; // number of processes
  private int count = 0; // number of waiting processes

  public synchronized void sync () {
    count = count +1;
    if ( count < number ) { // not everybody here , so ...
      wait (); // wait , leave the method
    } else {
      notifyAll (); // deblock all waiting processes
      count = 0;
    }
  }
}
```


### Semaphore (Variante der Zaehlersperre) {#semaphore--variante-der-zaehlersperre}

-   are used not to guarantee an exclusive access but a limited access (eg 5 processes can access the ressource at the same time)
-   data type S functions as a counter and has two operations:
    -   locksem(s), down(s) &rarr; reduce counter
    -   unlocksem(s), up(s) &rarr; increment counter
-   initial value for the counter determines the capacity (by how many the resource is accessible simultaneously)
-   locksem(s) is called when a process accesses the resource and the counter is then decremented by 1 (capacity -= 1) if the counter is > 0; if the counter = 0 then the process that called locksem(s) is blocked until the operation can be executed
-   unlocksem(s) is called when a process releases the resource and the counter is then incremented by 1 (capacity += 1), if another process is blocked from locksem(s) it is then able to continue


## Solutions to Cooperation Problems via Coordination {#solutions-to-cooperation-problems-via-coordination}

Repetition: in case of cooperation there is no explicit (initiated) receiving which can lead to problems

Many problems can be solved by creating mutual exclusion

-   locks/mutex
-   count locks (semaphore) with capacity/limit 1

Other problems require considering capacities

-   count locks

Problems: The programmer has to ensure that every lock gets unlocked and only once and that there's no recursion in a locked section


### Monitor {#monitor}

Using locks/semaphores is relatively error prone. An automatic locking and unlocking mechanism would be favorable. Monitors introduce mutual exclusion without requiring the programmer to explicitly use synchronization primitives like semaphores.


### Conditional Synchronization {#conditional-synchronization}

-   Bedingungssynchronisation
-   slides: evtl. muss ein Prozess innerhalb der Ausführung einer der Operationen blockieren
-   requires temporaly release of the implicit lock
-   for example ring buffer: waiting in `fetch()` if the buffer is empty
-   solution: conditional synchronization
    -   operation `cwait(c)`
        -   process releases monitor and waits for the `csignal(c)` and then continues
    -   operation `csignal(c)`
        -   a waiting process is released
        -   when there is no waiting process this procedure has no effect
    -   blocked processes are put in a queue

Notice the semantic differences to signaling (signal/swait):

-   `cwait` = release lock, block caller
-   `swait` = no influence on the lock, block caller

-   `csignal` = no effect when nobody's waiting, later `cwait` will still block
-   `signal` = always changes the state, later `swait` then won't block


### Different kinds of capacity limited synchronization {#different-kinds-of-capacity-limited-synchronization}

The capacity limited synchronization eg semaphore can be extended to account for different types of access. Eg Reader-Writer-Coopeartion where either one writer or multiple readers are allowed.

See page 26/27 for ringbuffer with capacity and different types


## Examples of existing coordination mechanisms {#examples-of-existing-coordination-mechanisms}

This section doesn't seem important and has not a lot of information, see page 28-31
