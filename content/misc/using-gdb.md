+++
title = "Debugging with gdb"
author = ["eo shiru"]
date = 2018-10-27
lastmod = 2019-04-06T00:23:00+02:00
draft = false
+++

## How to debug a c program with gdb {#how-to-debug-a-c-program-with-gdb}


#### Step 1. Compile the C program with debugging option {#step-1-dot-compile-the-c-program-with-debugging-option}

Compile the C program with the debugging option which is usually `-g` for most compilers. This allows the compiler to collect the debugging information.

```sh
gcc -g example.c
```

The above command creates a `a.out` file which will be used for debugging. Of course other compiler options e.g specifying file name or turning on warnings is possible!


#### Step 2. Launch gdb {#step-2-dot-launch-gdb}

Launch the C debugger gdb as shown below:

```sh
gdb a.out
```


#### Step 3. Set breakpoints {#step-3-dot-set-breakpoints}

Places break point in the C program, where you suspect errors. While executing the program, the debugger will stop at the break point, and gives you the prompt to debug.
To set a breakpoint enter the following:

```sh
break {linenumber}
```

Additionally it is also possible to do so in the following formats/ways (the filename is optional):

```sh
break {filename:}{lineNumber}
break {filename:}{functionName}
```


#### Step 4. Execute the C program {#step-4-dot-execute-the-c-program}

Now the program can be executed using the `run` command in the gdb debugger. It is now also possible to specify command line arguments via `run {args}`.
The program then executes until it encounters the first breakpoint or finishes execution and stops.


#### Step 5. It stopped - now what?! {#step-5-dot-it-stopped-now-what}

Now since the execution is halted there multiple things you can do. `print {varName}` prints the current value of an variable (can be shortened to `p`). `c` or `continue` resumes execution until the next breakpoint. `n` or `next` executes the next line as _a single instruction_. `s` or `step` same as `next` but doesn't treat functions as a single instruction, instead goes into the function to execute it line by line.

By the way `<ENTER>` repeats the previous command. `l` or `list` can be used to print the source code, `l {lineNumber}` can be used to view a specific line and `l {functionName}` to show a function. `bt` or `backtrack` prints a backtrace of all stack frames. Finally `quit` exits the gdb debugger.

Source: <https://www.thegeekstuff.com/2010/03/debug-c-program-using-gdb/>
