# Operating Systems Chapter 3

## Process Concept

An OS executes a variety of programs:
  * Batch system - Jobs
  * Time-shared systems - user programs or tasks

Job and process are used almost interchangeably

**Process** - A program in execution; process execution must progress in sequential fashion

Multiple parts of a process:
1. The text section which is the program code
2. The current activity including program counter and processor registers
3. The stack - Containing temporary data such as function parameters, return addresses and local variables
4. The data section containing global variables
5. The Heap containing memory dynamically allocated at run time

**Program** - A passive entity stored on disk (executable file). Process is active while program is passive.
 \* A program becomes a process when its executable file is loaded into memory.

One program can be several processes, ie multiple users executing the same program.

![process](/home/minad/Pictures/process.png)

## Process State

As a process executes it changes its state.

Process states:
**new**: The process is being created
**running**: Instructions are being executed
**waiting**: The process is waiting for some event to occur
**ready**: The process is waiting to be assigned to a processor
**terminated**: The process has finished execution

The following is a representation of a process life cycle.

The process starts in the *new* state.
The process is then admitted to the *ready* state.
When the scheduler dispatches it it moves to the running state.
Then, if it has to wait for I/O or some other event, it moves to the waiting state until the I/O or event completes, after which it moves back to the ready state. If it gets interrupted while running it moves back to the running state. Step 3 is then repeated until the process finishes in which case it exits to the terminated state.

![process_state](/home/minad/Pictures/process_state.png)

## Process Control Block (PCB)

The PCB (also known as task control block) is the information associated with each process

This information contains:
The process state
Program counter - location of next instruction to execute
CPU registers - Contents of all process-centric registers
CPU scheduling information - priorities, scheduling queue pointers
Memory-management information - Memory allocated to the process
Accounting information - CPU used, clock time elapsed since start, time limits
I/O status information - I/O devices allocated to process, list of open files

![pcb](/home/minad/Pictures/pcb.png)

## CPU Switch from process to process

![process_switch](/home/minad/Pictures/process_switch.png)

## Process Scheduling

Maximize CPU use, quickly switch processes onto CPU for time sharing

**Process scheduler** selects among available processes for next execution on CPU

Maintains scheduling queues of processes:
* Job queue - set of all processes in the system
* Ready queue - set of all processes residing in main memory, ready and waiting to execute
* Device queues - set of processes waiting for an I/O device

Processes migrate among the various queues

![queues](/home/minad/Pictures/queues.png)
![queues_diagram](/home/minad/Pictures/queues_diagram.png)
