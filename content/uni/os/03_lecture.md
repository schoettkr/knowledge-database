+++
title = "Operating Systems - Processes"
author = ["eo shiru"]
date = 2019-04-11T10:00:00+02:00
lastmod = 2019-04-24T16:43:43+02:00
tags = ["uni", "os"]
draft = false
+++

The term **process** serves as an abstraction for the cooporation between CPU, OS, resources and programs. There are different definitions in the literature tho:

-   unit of an activity from the OS / programmers view
-   executing instance of a program
-   unit of protection
-   (virtual) adress space
-   collection of resources

Wiki: A process is the instance of a computer program that is being executed. It contains the program code and its activity. Depending on the operating system (OS), a process may be made up of multiple threads of execution that execute instructions concurrently. While a computer program is a passive collection of instructions, a process is the actual execution of those instructions. Several processes may be associated with the same program.


### Program to Process (C, Repetition) {#program-to-process--c-repetition}

{{< figure src="/knowledge-database/images/program-to-process-1.png" >}}

-   specify the '-v' flag to see the single steps when compiling with the GNU compiler

**Compiler:**

-   eg `/usr/bin/gcc`
-   translates source code to assembly code

**Assembler:**

-   eg `/usr/bin/as`
-   translates assembly code to machine code
-   results in the object file / object code / object module
-   assembles symbols and marks

**Linker:**

-   eg `/usr/bin/ld` or `/usr/local/bin/collect2`
-   resolves/completes missing (unaufgeloest) symbols
-   **static** linking
    -   all symbols must be known at link time
    -   implementation becomes a part of the program file
-   **dynamic** linking
    -   symbols are resolved at runtime
    -   libraries are loaded when they're called (only possible if OS supports this)
    -   multiple processes can share code
-   object code + start up code &rarr; program file
-   there differernt formats for program files, eg `a.out`, `ELF` or `PE`


### Processes and Memory {#processes-and-memory}

-   machine code works with addresses in memory (Hauptspeicher)
-   the (logical) address space of a process is the entirety of addresses which the process can acess
-   address spaces and processes are independant concepts
-   a process requires and adress space at all times, but there may be different kinds of relations:
    -   a process has exactly one address space (classic UNIX process)
    -   multiple processes share an adress space (threads)
    -   process changes from one adress space to another
-   Operating Systems can offer protection against invalid memory accesses


### Process and virtual CPUs {#process-and-virtual-cpus}

-   usually there are more processes running than there are physical CPUs (&rarr; competition between processes)
-   context switches (Prozessumschaltungen) switch between processes to achieve "fairness"
    -   the switching is hidden from the user
-   the CPU is the active resource and "pulls" the process
-   every process therefore gets a _virtual CPU_
    -   Quora: When a core has _hyperthreading_, it’s considered to have virtual CPUs. The virtual CPUs are not fully independent because they have to contend with each other for unshared resources like memory. The operating system, however, will treat them as if they were entirely separate CPUs, so your 4-core i7 will appear to have 8 CPUs.

In this lecture a process is understoof as an unit of activity/execution where a program or part of an program is executed on a virtual processor

-   the virtual CPU in this case is implicitly created from the CPU


#### Process Control Block / Prozessbeschreibung (PCB) {#process-control-block-prozessbeschreibung--pcb}

-   process = program in execution &rarr; Code + **State**
-   **state:**
    -   what is the current position of execution?
        -   &rarr; program counter(PC, Befehlszaehler/zeiger) and (call-)stack
    -   which immutable data?
        -   variables (global, local, named, anonomyous) &rarr; register, stack, heap
-   the OS needs additional information to manage processes
    -   process id (Prozesskennung)
    -   name of program
    -   location of the code and the state
    -   possibly: priority, rights, statistics

The management information and parts of the state are stored in the **process control block** (PCB). The process informations are accessed directly or indirectly via the PCB. In bigger systems there are hundreds of processes that's why the PCB datastructures have to be efficient (&rarr; datastructure problem). Depending on the usage and the OS there're different options: eg array, tree, table. PCBs can also be distributedly stored as attribute lists.

**Static Operating Systems:**

-   all processes are known beforehand and statically defined
-   processes are defined in the OS programming process
-   PCB are defined as variables
-   processes are realized for a specific application
-   PCBs are created _once_ when booting

**Dynamic Operating Systems:**

-   the processes are created and deleted via kernel operations
-   usage of system calls (eg `create_process`, `delete_process`)
-   processes can also end/dissolve (verschwinden) by ending an application


### Dispatching {#dispatching}

Dispatching (process switching)

-   CPU interrupts the execution of the current process and continues another process
-   process switching means transitioning from one instruction sequence to another
    -   but: the CPU has only one instruction sequence
    -   therefore the register/program counter (slides: Befehlszaehler, register) is changed


#### Process switching via Jumps {#process-switching-via-jumps}

In the simplest case the dispatching is "programmed into" the processes:

-   the CPU immediately jumps to another process
-   in a high level programming language this would be an coroutine

```python
var q := new queue

coroutine produce
    loop
        while q is not full
            create some new items
            add the items to q
        yield to consume

coroutine consume
    loop
        while q is not empty
            remove some items from q
            use the items
        yield to produce
```

When there's no high level language support there has to be intervened at another place in the toolchain.<br />
So if a process is running and then there's a switch to another process, the original process would just continue where it left off, like in the picture below:
![](/knowledge-database/images/process-jump.png)

For some systems (eg real time systems) the required time to "jump" is an important quality measure. Therefore the process switch has to be really efficient and a jump is (just) a minimal solution.


#### General requirements for process switching {#general-requirements-for-process-switching}

Process switching via direct jumps is not so flexible and therefore only usable in special and simple cases. We could achieve better flexibility by:

-   variability in regards to the source of the jump
    -   &rarr; storing the continuation location as a variable in the PCB
-   choose the next process dynamically
    -   the next process to which we're switching does not always have to be the same
-   storing the process state
    -   essential parts of the process description mustn't be lost

All three of these aspects influence the process switching time

-    1. Storing the continuation location

    Before switching the process a pointer to the address of the next instruction to be executed for the current process is stored in a variable of the PCB (eg `ni`):<br />
    ![](/knowledge-database/images/pcb-ni.png)

-    2. Choose next process

    The next process gets chosen when the process switch happens. Here some possible criteria for choosing the next process:

    -   number/id of the process (cyclic switch)
    -   order of arrival (Ankunftsreihenfolge, choose the proccess that is first ready)
    -   priority

    Suppose that `PSelect()` is a function that selects the next process in accordance to a given criteria.<br />
    ![](/knowledge-database/images/pselect.png)<br />
    The next process in accordance to an order is then the next one to be get executed (`p_next`). New processes that arrive are inserted in the right order as well. This establishment of a process execution order is called `scheduling`. There are special scheduling algorithms (more on that later). Scheduling impacts the distribution of CPU time and other resources in regards to the processes and therefore also influences the system performance.<br />
    ![](/knowledge-database/images/scheduling.png)

-    3. Process state

    Processes use CPU data registers to store intermediate results (Zwischenergegnisse). When switching processes via jumps, these intermediade results are lost. That's why the "jump-solution" is only suitable if the content of the data registers isn't needed any longer and if the new/next process doesn't expect some kind of valid data register contents. The aforementioned boundaries in regards to data registers and intermediate results are usually too large in practice.<br />
    In the case of subroutine calls (Unterprogrammaufruf) / Interrups data/results get stored on the stack. However this is not appropiate for processes because the stack would implicitly determine the execution order (LIFO) which is unwanted.<br />
    In addition to the program counter and the data registers a CPU holds quite a lot of other process-specific information, for example: contents of address registers, segmentation tables, interruption masks, access-control information etc (this forms the **execution environment** for a process).<br />
    All in all, the whole process specific information stored in the CPU, is called **process context** (alternative term: micro state). This **process context** has to be saved when switching processes and needs to be restored when the process continues (context switch). When data is constant and available via PCB then the storing part can be ommitted.

    -    3.1 Context Switching

        Context switching is the most expensive/costly part of process switching. To speed things up the CPU hardware may offer support:

        -   special instructions which write the complete content of a register set from the CPU into memory and vica versa (eg ARM, X86, X64)
        -   offering multiple register sets on the CPU, so that just a register may has to be changed to specify the number of the currently valid register set ("durch Bereitstellung mehrerer Registersätze auf dem Prozessor, so dass beim Umschalten u.U. nur das Register geändert werden muss, das die Nummer des gültigen Registersatzes angibt", eg Sparc, Z80)

        In addition to the process and CPU state, there may be other states that need to be accounted for eg memory controller, interrupt controller etc.


#### Switching (Umschalten als offene Befehlsfolge) {#switching--umschalten-als-offene-befehlsfolge}

Switching has the following form:<br />
![](/knowledge-database/images/switching.png)


#### Procedure Call {#procedure-call}

The line of action when switching processes is - as already has been said - similar to a procedure call:<br />
![](/knowledge-database/images/procedure-call.png)


#### Switching as Subroutine (Umschalten als Unterprogramm) {#switching-as-subroutine--umschalten-als-unterprogramm}

The idea is that switching is a subroutine `SWITCH()`. Calling `SWITCH()` pushes the point of continuation onto the stack of `p_run`. `load context of p_run` also loads the **stackpointer** thereby the stack of `p_next` is used. `return` jumps to the point of continuation which lays on the **new** stack.
![](/knowledge-database/images/switch-subroutine.png)
Slides: Merke, dieser Prozedursprung ist ungewöhnlich - eine Prozedur (in einem Prozess) ruft ein Unterprogram auf, das später in eine andere Prozedur (in einem anderen Prozess) zurückkehrt.

It is necessary to regularily call `SWITCH()` to switch processes. Application programmer(s) shouldn't be burdened with this (&rarr; abstraction of the process as a virtual CPU &rarr; exlusive usage). As a solution necessary system services/daemons call `SWITCH()`. System calls may lead to a process switch and returning from a system call may happen after multiple process switches.


#### Automatic Switching {#automatic-switching}

In a lot of cases it is not possible or reasonable to include process switching logic explicitly into processes. It would be nice to have this done _automatically_. To realize this an intervall clock or timer, that is a hardware facility (I/O-Device), with the following functionality is needed:

-   specify a timer/ time slice (Vorgabe einer Frist / Stellen des Weckers)
-   interrupt when timer is over (Unterbrechen bei Fristablauf / Wecken)

Automatic switching is _pre-emptive_ while optional (freiwillig) switching is _cooperative_. Programs can stay unmodified when automatically switching. The switching is triggered from the 'outside' and can therefore occur at any given moment.


### Switching Prevention (Umschaltverhinderung) {#switching-prevention--umschaltverhinderung}

When automatically switching there's is no control over the point in time when the switch takes place. For example an automatic switch can be triggered while a cooperative switch takes place.
Concurrent changes to the same resource may then lead to inconsistencies (and thereby to problems). Code section that can lead to problems are called **critical sections**. If an instruction sequence enters a critical section, no additional instruction sequence should a enter critical section that may be in conflict with the current section (_mutual exlusion_).


#### Kernel as critical section {#kernel-as-critical-section}

In the OS kernel all potential conflict sections have to be identified and protected accordingly.<br />
Simple Solution: The whole kernel is put under mutual exclusion

-   kern operations then never run interlocked (verzahnt), instead the run sequentially (an einem Stueck)
-   interlocked execution only for processes which run in user mode
-   entering the kernel may happen from the top via system calls / process switching or from the bottom via interrupts of I/O hardware

Better solution: Not the whole kernel but only some sections are put under mutual exclusion

-   Kern becomes interruptable (preemptive)


#### Mutual Exclusion (gegenseitiger Ausschluss) {#mutual-exclusion--gegenseitiger-ausschluss}

How can the OS realize mutual exclusion?

Case 1: One CPU, cooperative switching, no interrupts

-   unproblematic, because nothing interrupts execution and no interlocks
-   communication with I/O devices only via polling
-   realistic for _small_ embedded systems
-   does not scale

Case 2: One CPU, preemptive switching via interrupts

-   solving via interrupt locks (Unterbrechungssperre)

{{< figure src="/knowledge-database/images/interrupt-lock.png" >}}

-   interrupts are disabled / not allowed in critical sections
-   the lock wait (Sperrzeit) has to be limited, otherwise problems with I/O-devices

Case 3: Multiple CPUs, cooperative switching, no interrupts

-   interlocks possible via parallel/concurrent execution
-   Idea: make use of a condition variable ( Bedinungsvariable, 1 Bit) &rarr; section lock
    -   if variable is not set &rarr; section or kernel is "free"
    -   if variable is set &rarr; section or kernel is already entered / in use
-   Problem: reading + setting the variable are 2 steps (protecting the critical section is a step aswell)
-   Solution: there's a computer instruction / opcode (Maschinenbefehl) which can retrieve and set a condition variable in a atomic operation
    -   there different names and sematics for this eg `compare-and-swap`, `fetch-and-add`, `bus-lock`,...
-   we'll use `test_and_set`:
    -   `test_and_set(reg, adr)` &rarr; `{load reg, adr; store adr, 1}`
    -   independantly of the variable's value the value of the variable `adr` is set to `1`
    -   the _previous_ value is stored in the register `reg`
    -   this get's repeated until the previous value is `0` (meaning that the lock is removed)
        -   &rarr; busy waiting
-   an actively waiting or busy waiting lock mechanism is also called **spin lock**
-   causes a certain waste of computing capacity

{{< figure src="/knowledge-database/images/spin-lock.png" >}}

Case 4: Multiple CPUs, preemtpive switching via interrupts

-   Idea: Combining both techniques
-   the image below shows different solution approaches

{{< figure src="/knowledge-database/images/locking-techniques.png" >}}

-   Solution A
    -   Beachte: Eine Unterbrechungsbehandlung ist eine Kernoperation, für deren Durchführung die Abschnittssperre des Kerns ebenfalls benötigt wird
    -   Gibt es nun direkt nach dem Setzen der Abschnittssperre einen Interrrupt, so würde in der Unterbrechungsbehandlung vergeblich versucht, die Abschnittssperre des Kerns zu setzen
    -   &rarr; Der Prozess würde an dieser Stelle „hängenbleiben“
-   Solution B
    -   Ideal wäre deshalb eine Operation, die unteilbar sowohl die Abschnittssperre als auch die Unterbrechungssperre setzt
    -   Leider findet man solche Operationen bei keinem Prozessor
-   Solution C
    -   Es muss daher zuerst die Unterbrechungssperre gesetzt werden und dann erst die Abschnittssperre
    -   &rarr; Lösung C ist also korrekt

Interrruption Locks are useful in the OS kernel but are not suitable for mutual exclusion in user processes. For kernel programming primitives such as `spin_lock` are available which are usually not available for application programmers. In application programs the problem of critial sections is solved via interprocess communication (IPC, chapter 5).


### Process State {#process-state}

When executing processes there might occur situations where the execution can not be continued or halts, for example when waiting for data that needs to be read. The next process would wait and do nothing in the meantime which would'nt be really effcient. That's why there's the concept of **conditional switching**:

-   whether or not a process switch happens depends on a condition
-   the condition can be represented by a simple binary variable which is set concurrently

When a process waits for lock, he cannot continue &rarr; switch to another process. To speed up the search for a process that is available to continue, processes are grouped by their **process state** (_macro state_):

-   **running** = processes whose code is currently executed on a CPU
-   **ready** = processes which can be continued but are not executed/running right now
-   **waiting** = processes which cannot be continued because they're waiting for a condition to be met

{{< figure src="/knowledge-database/images/process-state.png" >}}

There are respective operations in the kernel for **state changes**:

-   **relinquish** (aufgeben)
    -   voluntarily switching (cooperative) to another process, the current process stays continueable, that means he switches in the state _ready_
-   **assign** (zuordnen)
    -   take up the next process from the "ready" group to continue its execution on the CPU
-   **block** (blockieren)
    -   stopping execution and leaving the CPU because of a not met condition (conditional switching)
    -   because the condition has to be met before the process continues the process goes in the state "waiting"
-   **deblock** (deblockieren)
    -   when the condition that a blocked process waited for is met, the process switches state to be "ready"

There's a differentiation to be made when changing process states. There are the pure state change operations _and_ the operations which are required in the context of the other required actions (the "real work", eg dispatching).<br />
![](/knowledge-database/images/stch_deblock.png)

In addition to the state change as an operation on the PCB data structure, the real switching also needs to happen. For example relinquishing as a kernel operation would look like this:

RELINQUISH()<br />
&darr;<br />
stch\_relinquish(p\_run) // state change of running process from running to ready<br />
&darr;<br />
switch(p\_run, p\_next) // switch processes, i.e. save and load process context<br />
&darr;<br />
stch\_assing(p\_run) // state change of new running process from ready to running<br />

In dynamic systems the amount of processes is variable:

-   **activating** / **deactivating**
    -   while a process can be defined (PCB exists), program and data section may also already be there, but the process "sleeps" / is not active
    -   that's why we distinguish between active and inactive processes
    -   state changes between these states are possible via the operations **activate** and **deactivate**
-   **creating** / **deleting**
    -   processes do not exist on boot and need to be explicitly created and may need to be deleted
    -   the operations **create** and **delete** are used for this

State diagram://
![](/knowledge-database/images/state-diagram.png)<br />
The above state diagram is a generic schema that is realized differently in concrete Operating Systems, for example UNIX:
![](/knowledge-database/images/state-diagram-unix.png)<br />
or Windows:
![](/knowledge-database/images/state-diagram-windows.png)<br />

-    Process Preemption (Verdrängung)

    Wiki: In computing, preemption is the act of temporarily interrupting a task being carried out by a computer system, without requiring its cooperation, and with the intention of resuming the task at a later time. Such changes of the executed task are known as context switches. It is normally carried out by a privileged task or part of the system known as a preemptive scheduler, which has the power to preempt, or interrupt, and later resume, other tasks in the system.

    Up to this point we treated processes in a way where they stay in execution until...

    -   he voluntarily (cooperative) relinquishes
    -   a process switch is forced
    -   he cannot continue execution (unfulfilled condition)

    However there are usually processes which are more urgent/important and processes which are less urgent/important (&rarr; priority). The _priority rule_ states that at every given moment the process with the highest priority is executed. As a consequence of this the OS doesn't wait until one of the three above mentioned situation occurs, but instead initiates a switch as soon as a process with a higher priority emerges / becomes ready. It is said that the executing process is _preempted_ by the more orgent process.

    **Checking Preemption**<br />
    Assumptions:

    -   priority rule is followed
    -   priorities stay the same during the "running" state
    -   violations only happen when a new process enters the "ready" group

    ![](/knowledge-database/images/check-preemption.png)<br />
    As per the state diagram, there are exactly three possible state changes in the "ready" group:
    relinquish, deblock and activate. These operations have to check conditions and p.r.n. (ggf.) induce a switch.<br />
    ![](/knowledge-database/images/preemption-diagram.png)<br />

    **Idle Problem** (Leerlaufproblem)<br />
    It is possible that all processes are waiting, so that the CPU doesn't have any work to do. An elegant solution to this is a so called **idle process**. An idle process has the following characteristics:

    -   never blocking
    -   never ending (cyclic process)
    -   lowest priority
    -   can be left at any time (preemption/cooperation)

    The tasks of an idle process are:

    -   empty loop `while true do;`
    -   temporarily halt/stop the hardware &rarr; save energy
    -   check internal datastructures
    -   cleanup internal resources


### Process Creation {#process-creation}

How can a process be executed for the first time? Conceptually each "entrance" (Eintritt) of a process happens via the procedure "switching". Solution: Initialize process (PCB, stack) as if it was in the middle of the switching procedure (slides: Prozess (Prozesskontrollblock, Stapel) so initialisieren, als stünde er mitten in der Prozedur „Umschalten“).


#### Process Creation in Unix {#process-creation-in-unix}

**Forking** is the fundamental concept in Unix. Child-processes are created as a copy of a parent process:

-   the child process gets a new process id (pid)
-   child process is added to the managing structures (Verwaltungsstrukturen)
-   relevant sections in memory are copied
-   incrementing the resource reference counter of the parent process
-   returning the child process identity to the invoking process

Example of `fork()`:

```C
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() {
  int pid = fork();
  if (pid == 0) { // child
    for (int j = 0; j < 5; j++) {
      printf("Child process: %d (PID: %d)\n", j, getpid());
      sleep(1);
    }
    exit(0);
  } else if (pid > 0) { // parent
    for (int i = 0; i < 5; i++) {
      printf("Parent process: %d (PID: %d)\n", i, getpid());
      sleep(1);
    }
  } else { // negative results means we have a problem
    fprintf(stderr, "Error");
    exit(1);
  }
  return 0;
}
```

Determining if we're in a child or parent process after running `fork()` can only be done via the return value.


### Threads {#threads}

Process = program in execution

-   classic approach is a **heavy weight process**
-   exlusive usage of resources (address space, opened files, etc)
    -   shared resources with other processes are an exception
-   process switching has to change the adress space for each process, to guarantee memory/storage protection

![](/knowledge-database/images/hw-process.png)<br />

**Thread**

-   a thread is a **light weight process** (LWP)
-   multiple instruction pointer + stacks in the same process
-   one program, multiple resources and multiple control flows
-   switching between threads in one process is not as costly
-   no memory protection between threads in the same process

![](/knowledge-database/images/lw-process.png)<br />


#### User Threads {#user-threads}

Threads are completely realized in the application layer as a library. The OS doesn't know about those threads. Works well with virtual runtime environments such as Java.

Pro:

-   works on every OS
-   high performance, because no context switch when switching threads

Contra:

-   if one thread is blocking, the kernel has to block the whole process (all threads) because the kernel doesn't see those
-   no interrrupts within a process (special solution for switching)


#### Kernel Threads {#kernel-threads}

Kernel threads are the defacto standard in common operating systems and basis of many modern server applications. The kernel organizes/manages a systemwide list of threads.

Pro:

-   scheduling in the OS works with threads
-   process can continue running despite a blocking thread
-   no runtime system and thread table needed in every process

Contra:

-   efforts for managing is larger (system calls)
-   often compensated by **pooling**
-   time to switch differs (process switch vs thread switch)


## Summary {#summary}

-   processes are a virtualization of the CPU
-   automatic process switching brings flexibility at the cost of introducing the problem of critical sections
-   contex switch is very expensive
    -   hardware support
    -   mitigating the process abstraction via threads
-   process states introduce overhead but simplify the process switch operations

![](/knowledge-database/images/processes-summary-until-now.png)<br />

---

Additional Resources: <http://faculty.salina.k-state.edu/tim/ossg/index.html>
