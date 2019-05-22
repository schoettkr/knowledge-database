+++
title = "Operating Systems - Scheduling"
author = ["eo shiru"]
date = 2019-04-25T10:00:00+02:00
lastmod = 2019-05-22T11:52:00+02:00
tags = ["uni", "os"]
draft = false
+++

## Introduction {#introduction}

A "process" is a concept of virtualization of the hardware resource CPU. There's a _strategy_ to decide which process is allowed to use the CPU at what time and there's a _mechanism_ which refers to the way the strategy is carried out. _Dispatching_ as the mechanism is what we covered in the last lecture. In this lecture we take a look at the strategy.<br />
Generally _scheduling_ means the temporal assignment (zeitliche Zuordnung) of activities to ressources in regards. There different levels of granuality where scheduling occurs:

-   when should programms start?
-   when should a process use a CPU?
-   when should a given instruction of an instruction sequence get executed?
-   when may a process use a device?
-   when may a program run in a cluster?

In the sphere of operating systems however scheduling usually refers to the assignment of CPU resources. The scheduling strategy can have a significant impact on the performance of a system and choosing a strategy depends on certain goals (eg high efficiency, low latency, fairness, puncuality, high troughput).<br />
![](/knowledge-database/images/scheduling-schema.png)<br />
![](/knowledge-database/images/execution-response.png)


## Common Scheduling Algorithms {#common-scheduling-algorithms}

The example(s) in the following slides assumes:

-   a CPU with one core
-   ready list (Bereitliste) = queue of processes in "ready" state
-   the following five processes/tasks:

{{< figure src="/knowledge-database/images/tasks.png" >}}


### First Come First Serve {#first-come-first-serve}

-   FCFS or FIFO (First In First Out)
-   tasks are processed in the order of their arrival in the ready list
-   tasks occupy CPU until they end or until voluntary release

{{< figure src="/knowledge-database/images/fifo.png" >}}


### Last Come First Served - Preemptive Resume {#last-come-first-served-preemptive-resume}

-   LCFS-PR
-   new arrivals in the ready list suspend the current process
-   processes are processed as long as there are no new arrivals
-   the goal is to favor short processes
    -   short processes have a higher chance to finish before another process arrives
    -   long processes might be suspended multiple times

![](/knowledge-database/images/lcfs-pr.png)
![](/knowledge-database/images/lcfs-pr2.png)


### Round Robin (RR) {#round-robin--rr}

-   processes are handled in order of arrival
-   after a specified period of time (&tau; time slice, quantum) suspension happens
-   the variant covered in this lecture puts new arriving & suspended processes at the end of the queue and new processes have a higher priority
-   the goal of this strategy is an even distribution of the CPU capacity and of the waiting period for the processes
-   choosing the time slice &tau; is an optimization problem
    -   in case of a large &tau; RR becomes more similar to FCFS
    -   in case of a small &tau; the expense of frequent context switches rises

{{< figure src="/knowledge-database/images/round-robin.png" >}}


### Priorities - Nonpreemptive (PRIO-NP) {#priorities-nonpreemptive--prio-np}

-   new arrivals are sorted into the ready list in accordance to their priority and then processed until they end or voluntarily release

{{< figure src="/knowledge-database/images/prio-np.png" >}}


### Priorities - Preemptive (PRIO-P) {#priorities-preemptive--prio-p}

-   like PRIO-NP however the current process is suspended if the priority is lower than the priority of a new arrival

{{< figure src="/knowledge-database/images/prio-p.png" >}}


### Shortest Job Next {#shortest-job-next}

-   SJN or SPN (shortest process next)
-   process with the lowest execution time is executed until its end or voluntary release
-   same as PRIO-NP if PRIO-NP uses the execution time as priority criteria

{{< figure src="/knowledge-database/images/sjn.png" >}}


### Shortest Remaining Time Next {#shortest-remaining-time-next}

-   SRTN
-   process with the shortest remaining time is processed next
-   current process can be suspended
-   favors shorter processes and leads on average to shorter response times than FCFS

{{< figure src="/knowledge-database/images/srtn.png" >}}

**Note:**<br />

-   SJN & SRTN need to know the (remaining) execution time which can only come from the user in form of an estimation (disadvantage)
-   longer processes can "starve" when there are always shorter ones


### Highest Response Ratio Next {#highest-response-ratio-next}

-   HRN
-   the response ratio \\(r\\) is defined as $r\_r \frac{t\_w + t\_e}{t\_e} where t\_w is the waiting time and t\_e is the execution time
-   process with the highest r\_r value is chosen next
-   strategy is non-preemptive (nicht verdraengend)
-   as it is the case for SJN shorter processes are favored, however longer processes don't have to wait for ever but instead gain "points" by waiting (t\_w increases)
-   again execution time has to be known

{{< figure src="/knowledge-database/images/hrn.png" >}}


### Multilevel Feedback {#multilevel-feedback}

-   FB
-   scheduling algorithm with preempption per queue (Scheduling mit Verdraengung pro Warteschlange)
-   process goes into next queue after each time slice
-   queue \\(i\\) is processed when queue \\(i-1\\) is empty
-   different time slice durations &tau; are possible eg \\(\tau = 2^i\\)

![](/knowledge-database/images/fb1.png)
![](/knowledge-database/images/fb2.png)


### Overview of common Scheduling Strategies {#overview-of-common-scheduling-strategies}

{{< figure src="/knowledge-database/images/scheduling-overview.png" >}}

Operating systems usually don't use just one, pure form of the scheduling algorithms, but instead mixed forms. Also keep in mind that scheduling involves an overhead (&rarr; cost-benefit analysis).


### Classical UNIX Scheduling {#classical-unix-scheduling}

Scheduling in the UNIX-verse follows the principles of  multiple Feedback Queues in which Round Robin is used. The goal is to have a general fairness and favor interactive processes. Priorities (0..127) are assigned and put into queues (low value &rarr; high prio; high value &rarr; low prio). The priorities are computed based on the CPU usage by the process, the overall CPU load and user specifications.

{{< figure src="/knowledge-database/images/fq-unix.png" >}}

In the picture above four priorities are grouped into a queue, these are called _priority classes_. The priority gets reevalued at for example every forth timer tick. When a process gets a priority that belongs to another priority class, the process is sorted into the according queue. This is called _aging_. I/O-intensive processes are favored by this principle, because they occupy the CPU only for a short amount of time. This in turn leads to a high parallelism between the active computer components (CPU and periphery). Computing-intensive processes are penalized however. The priority computation might look similar to this:
\\[P\_J = min(P\_{base} + [\frac{L\_{CPU}}{4}] + 2 \* p\_{nice}, 127)\\]

-   P<sub>base</sub> is the base priority (eg 50)
-   p<sub>nice</sub> is an importance level that is user choosen in the range of eg -20..19

    L<sub>CPU</sub> is value in the PCB of a running process and is incremented at each tick. Over time L<sub>CPU</sub> increases and therefore the priority values of longer running process would get really high (= low prio). That's why _smoothing_ (Glaettung) exists. Smoothing cushions the impact of the last computation time and happens once per second:

\\[L'\_{CPU} = \frac{2\*l\_{RQ}}{2\*l\_{RQ} + 1} \* L\_{CPU} + p\_{nice}\\]

-   l<sub>RQ</sub> is the average length of the ready queue in the last minute

In the special case of blocking (sleeping) processes L'<sub>CPU</sub> is computed differently because these processes' L<sub>CPU</sub> values are not increased per timer ticks:
\\[L'\_{CPU} = (\frac{2\*l\_{RQ}}{2\*l\_{RQ} + 1})^{t\_{sleep}} \* L\_{CPU}\\]

-   t<sub>sleep</sub> is incremented once per second


### Linux-Scheduling {#linux-scheduling}

Note: This is a look at how scheduling was done in the old Linux 2.4.<br />

-   process management via doubly linked lists
    -   list to manage all processes
    -   list to manage all processes that are ready to run
-   the process classes
    -   conventional processes (interactive or stack processed)
        -   using prioritizing Round-Robin
    -   FIFO real time processes
    -   RR real time processes
-   the CPU time gets divided into epochs
-   in a single epoch, every process has a specified time quantum whose duration is computed when the epoch begins
    -   at the end of a epoch all processes have used their time quantum
-   each process has a _base time quantum_ which is the time-quantum value assigned by the scheduler to the process if it has exhausted its quantum in the previous epoch
    -   the users can change the base time quantum of their processes by using the `nice( )` and `setpriority( )` system calls
    -   a new process always inherits the base time quantum of its parent
-   the linux scheduler computes the goodness (Guete) of each runable process and chooses:
    -   goodness = 0 &rarr; process has used its quantum
    -   0 < goodness < 1000 &rarr; conventional process with a remaining quantum in this epoch
    -   goodness > 1000 &rarr; real time process
-   see <https://www.oreilly.com/library/view/understanding-the-linux/0596002130/ch11s02.html>
-   [here](https://www.linuxjournal.com/files/linuxjournal.com/linuxjournal/articles/039/3910/3910l3.html) is the source code of the goodness function
-   according to benchmark a core spends 37-55% in the goodness function (&rarr; bad scalability)

The problem mentioned above lead to the development of the Linux \\(\mathcal{O}(1)\\) scheduler (from Linux 2.6 and upwards).
The goals in mind were to achieve an improved scalability and SMP (symmetric multi processing). There were new priority levels (140) introduced:

-   0..99 for "real time"
-   100..139 for "normal" processes (in `top` etc displayed as 0..39)
-   user processes usually start with a priority of 119
-   priorities are assigned at start (`nice`) and vary by \\(\pm 5\\) max
    -   CPU intensive tasks are punished
    -   IO-intensive tasks are rewarded
    -   interactive processes get their quantum "refilled" each milisecond

As in the previous scheduler each process has a time quantum:

-   the quantum depends on the priority

    ```C
    #define BASE_TIMESLICE(p) (MIN_TIMESLICE + \
        ((MAX_TIMESLICE - MIN_TIMESLICE) * \
         (MAX_PRIO - 1 - (p)->static_prio / (MAX_USER_PRIO-1)))
    ```
-   per processor/core there are two priority arrays
    -   array for all processes with a positive quantum
    -   array for all processes with a used up quantum
-   each of these arrays consists of a bitmap and a queue of runable processes for every priority level
    -   bit in the bitmap specifies if a task is available in the corresponding queue
-   when there are no more processes with a positive quantum then the pointer of the two arrays are swapped
-   the next process is chosen based on the first bit that is set in the bitmap (highest prio) which respond to each prio level; inside a prio level Round Robin is then used to manage the processes

{{< figure src="/knowledge-database/images/prio-bitmap.png" >}}
