# Chapter 2

## Operating System Services

Operating systems provide an environment for execution of programs and services to programs and users
### Application programs
One set of operating-system services provide functions that are helpful to the user, such as:
  * User Interface - Almost all operating systems have a UI. Either GUI or CLI or Batch(Batch interfaces are non-interactive user interfaces, where the user specifies all the details of the batch job in advance to batch processing, and receives the output when all the processing is done. The computer does not prompt for further input after the processing has started.)
  * Program execution -The system must be able to load a program into memory and run that program and end execution normally or abnormally (indicating an error)
  * I/O operations - A running program may require I/O which may involve a file or an I/O device
  * File-system manipulation - The file system is of particular interest as programs need to read and write files and directories, as well as create and delete, search, list file information and manage permissions
  * Communications - Processes may exchange information on the same computer or between computers over a network. Communications may be via shared memory or through message passing (packets moved by the OS)
  * Error detection - OS needs to be constantly aware of possible errors
    - May occur in the CPU and memory hardware, in I/O devices or in the users program
    - For each type of error, the OS should take the appropriate action to ensure correct and consistent computing
    - Debugging facilities can greatly enhance the users and programmers abilities to efficiently use the system


### System programs
A different set of  OS functions exist for ensuring the efficient operation of the systems itself via resource sharing:
  * Resource allocation - When multiple users or multiple jobs running concurrently, resources must be allocated to each of them. Resources such as CPU cycles, memory access, file storage, I/O devices
  * Accounting - To keep track of which users use how much and what kind of computing resource.
  * Protection and security - The owners of information stored in a multiuser or networked computer system might want to control that information, concurrent processes should not interfere with each other.
    - Protection involves ensuring that all access to system resources is controlled
    - Security of the system from outsiders requires user authentication, extends to defending external I/O devices from invalid access requests.

#### User Operating System Interface - CLI
  CLI or command interpreter allows direct command entry
  * Sometimes implemented in kernel, sometimes by systems program
  * Sometimes multiple flavors are implemented - shells
  * Primarily fetches a command from the user and executes it.
  * Sometimes commands are built in but sometimes its just the name of programs

#### User Operating System Interface - GUI
  User friendly metaphor Interface
  * Usually have a mouse keyboard and monitor.
  * Icons represent files, programs, shortcuts, actions etc
  Various mouse buttons over objects in the Interface cause various actions. (Ie provide information, opetions, execute function, or open directory)
  * Invented at Xerox PARC

  Many systems now include both CLI and GUI interfaces.

### System calls
Programming interface to the services provided by the OS.
Typically written in a high level language (C or C++).
Mostly accessed by programs via a high-level API rather than direct system calls.

The most commmon APIs are Win32 API for Windows, POSIX API for POSIX-based systems (inc. virtually all UNIX, Linux and MacOS) and Java API for JVM.

#### System Call Implementation

Typically, a number is associated with each system call.
**The system call interface** maintains a table indexed according to these numbers.

The system call interface invokes the intended system call in OS kernel and returns the status of the system call and any return values

The caller needs to know nothing about how the system call is implemented. It just needs to obey the API and understand what the OS will do as a result of the call.

More details of OS interface is hidden from the programmer by the API; and is managed by run-time support library (Set of functions built into libraries included with compiler)

#### API - System Call - OS Relationship

  ![syscall_API](/home/minad/Pictures/syscall_api.png)

  ### System Call Parameter Passing

  Often more information is required than simply the identity of the desired system call. The exact type and amount of information vary according to OS and call.

  Three general methods are used to pass parameters to the OS:
  * Simplest way - Pass the parameters in registers. In some cases however there may be more parameters than registers.
  * Parameters stored in a block or table in memory and the address is passed as a parameter in a register. This is the approach taken by Linux and Solaris
  * Parameters are placed or **pushed onto** the stack by the **program** and **popped off** the stack by the **operating system**.
  * Block and stack methods do not limit the number or length of parameters being passed.

  #### Parameter passing via Table
  ![parameter_table](/home/minad/Pictures/parameter_table.png)

  ### Types of System Calls
    * Process control
        - Create/terminate process
        - End/abort
        - Load/execute
        - Get/set process attributes
        - Wait for time
        - Wait for event or signal event
        - Allocate and free memory
        - Dump memory if error
        - Debugger for determining bugs, step-by-step execution
        - Locks for managing access to shared data between processes
    * File management
        - Create/delete file
        - Open/close files
        - Read, write, reposition
        - Get and set file attributes
    * Device management
        - Request device, release device
        - read, write, reposition
        - Get device attributes, send device attributes
        - Logically attach or detach devices.
    * Information maintenance
        - Get/set time or date
        - Get/set system data
        - Get/set process file or device attributes
    * Communications
        - Create/delete communication connection
        - Send/receive messages if message passing model to host name or process name: From client to server
        - Shared memory model create and gain access to memory regions
        - Transfer status information
        - Attach and detach remote devices
    * Protection
        - Control access to resources
        - Get/set permissions
        - Allow and deny user access

## Examples

### MS-DOS

  * Single-tasking
  * Shell invoked when system boots
  * Simple method to run program - No process created
  * Single memory space
  * Loads program into memory, overwriting all but kernel
  * When the program exits it reloads the shell

  ![ms-dos](/home/minad/Pictures/ms-dos.png)

### FreeBSD

  * UNIX variant
  * Multitasking
  * When the user logs in, the users choice of shell is invoked
  * Shell executes fork() syscall to create process
    - Executes exec() to load program into process
    - Shell waits for process to terminate or continues with user commands
  * Process exits with code 0 if there was no error
  * But exits with a code larger than 0 is an error code

## System programs
  System programs provide a convenient environment for program development and execution. Some of them are simply user interfaces to system calls; others are considerably more complex

  They can be divided into:
  * File manipulation
  * Status information sometimes stored in a file modification
  * Programming language support
  * Program loading and execution
  * Communications
  * Background services
  * Application programs

      Most users' view of the operating system is defined by the system programs not the actual system calls.

  * File manipulation - Create, delete, copy, rename, print, dump, list and generally manipulate files and directories.
  * Status information
    - Some ask the system for info like date, time, available memory, disk space, no. users
    - Others provide detailed performance, logging and debugging information
    - Typically these programs format and print the output to the terminal
    - Some systems implement a registry - used to store and retrieve configuration information
  * File modification
      - Text editors to create and modify files
      - Special commands to search contents of files or perform transformations of the text
  * Programming language support - Compilers, assemblers, debuggers and interpreters are sometimes provided
  * Program loading and execution - Absolute loaders, relocatable loaders, linkage editors and overlay-loaders, debugging systems for higher level and machine language
  * Communications - Provide the mechanism for creating virtual connections among processes, users and computer systems
    - Allow users to send messages to one another's screens, browse web pages, send electronic mail messages, log in remotely, transfer files between machines
  * Background services
    - Launch at boot time
      - Some launch for system startup and terminate
      - Some only terminate at shutdown
    - Provide facilities like disk checking, process scheduling, error logging, printing
    - Run in user context not kernel context
    - Known as services, subsystems or daemons
  * Application programs
    - Don't pertain to system
    - Run by users
    - Not typically considered part of OS
    - Launched by cmd line, mouse click or touching of screen

### Operating System Design and Implementation

  Design and Implementation of OS is not "solvable" but some approaches have proven successful. The internal structure of Operating Systems can vary wildly.

  The design is started by defining goals and specifications. The design is affected by the choice of hardware and the type of system.

  ###### User goals vs System goals

  User goals - Operating System should be convenient to use, easy to learn, reliable, safe and fast

  System goals - Operating System should be easy to design, implement and maintain. It should also be flexible, reliable, error free and efficient.

  It is important to separate the **policy**, that is "*What* will be done?" and the **Mechanism**, that is "*How* to do it".

  The principle of separating policy from mechanism is an important principle as it allows maximum flexibility if decisions are to be changed later.

  Specifying and designing an OS is a highly creative task of software engineering

  ### Implementation

  There is much variation in the implementation of OS's as the early OS's were written in assembly, then in system programming languages like Algol, PL/1, and now in C, C++.

  Its actually now a mix of languages where the lowest levels of OS are written in assembly, the main body in C, and system programs in C, C++, scripting languages.

  More high level languages are easier to port but are slower.

  **Emulation** can allow an OS to run on non-native hardware

  ### Operating System Structure

  General-purpose OS is a very large program with various ways to structure it.

  Simple structure - MS-DOS
  More complex - UNIX
  Layered - An abstraction
  Microkernel - Mach(?)

  #### Simple Structure - MS-DOS

  MS-DOS was written to provide the most functionality in the least space.
  * Not divided into modules
  * Although it has some structure, its interfaces and levels of functionality are not well separated

  ![ms-dos_structure](/home/minad/Pictures/ms-dos_structure.png)

  #### Non Simple Structure - UNIX

  UNIX - Limited by hardware functionality. The original UNIX OS had limited structuring. The UNIX OS consists of two separate parts:
  - Systems programs
  - The kernel, which consists of everything below the sys call interface, and above the physical hardware
    - Provides the file system, CPU scheduling, memory management, and other OS functions.It provides a large number of functions for one level.

![unix_structure](/home/minad/Pictures/unix_structure.png)

  #### Layered Approach

  The OS is divided into a number of layers(levels), each built on top of lower layers.
  The bottom layer (layer 0) is the hardware; the highest layer (layer N) is the UI.

  With modularity, **layers are selected such that each uses functions or operations and services of only lower-level layers**.

  #### Microkernel System Structure

  Mach is an example of microkernel. MacOS (Darwin) is partly based on Mach.

  The microkernel moves as much from the kernel into user space as possible.

  Communication takes place between user modules using message passing.

  Benefits:
  * Easier to extend a microkernel
  * Easier to port the OS to new architectures
  * More reliable as less code is running in kernel mode
  * More secure

Detriments: Performance overhead of user space to kernel space communication

  ![microkernel_structure](/home/minad/Pictures/microkernel_structure.png)

  #### Modules

   Many modern OSs implement **loadable kernel modules**
  * Uses object orientated approach
  * Each core component is a separate module
  * Each talks to others over known interfaces
  * Each is loadable as needed by the kernel

    Similar to layers but with more flexibility, used in Linux and Solaris
  #### Solaris Modular Approach

  ![Solaris_Modular](/home/minad/Pictures/Solaris_Modular.png)

  #### Hybrid Systems

  Most modern operating systems are actually not one pure model.

  Hybrid combines multiple approaches to address performance, security and usability needs.

  Linux and Solaris kernels are monolithic, because having the operating system in  a  single  address  space  provides  very  efficient  performance. However, they are also modular, so that new functionality can  be dynamically added to  the  kernel.

  Windows is mostly monolithic, but retains behaviours of microkernels including providing support for separate subsystems (known as *personalities*) that run as user-mode processes

  MacOS uses a  hybrid structure. As shown below it uses a layered system with Aqua UI , and a set of application environments and services such as the Cocoa programming environment. Below these layers is the *kernel environment*. This is a kernel consisting of Mach microkernel and BSD UNIX parts, plus I/O kit and dynamically loadable modules - called kernel extensions. See below image.

  ![macos_kernel](/home/minad/Pictures/macos_kernel.png)

  #### iOS

  iOS is structured on MacOS, and added functionality. Doesn't run MacOS applications natively and also runs on a different CPU architecture (ARM vs Intel)

  Cocoa Touch is an Objective-C API for developing apps.

  Media services layer is for graphics audio and video.

  Core services provides a variety of features such as cloud computing and databases.

  The Core OS is based on MacOS kernel

  #### Android

  Android OS is developed by Open Handset Alliance (mostly Google) and is Open Source. Its similar to iOS in  that  it  is  a  layered  stack  of  software  that provides a rich set of frameworks for developing mobile applications and based on the Linux Kernel but is modified. Linux provides process, memory, device-driver management and the android kernel has been expanded to add power management.

  Runtime environment includes set of core libraries and Dalvik virtual machine. Apps are developed in Java using the Android API. Java class files are compiled to Java bytecode then translated to an executable that runs in the Dalvik VM

  Libraries include frameworks for web browser (webkit), database (SQLite), multimedia, and libc. The libc library is similar to the standard C library but is much smaller and has been designed for the slower CPUs that characterize mobile devices.

  ![android_architecture](/home/minad/Pictures/android_architecture.png)

### Operating System Debugging

  Debugging: Finding and fixing errors or bugs. OS generates log files containing error information.

  Failiure of an application can generate core dump file capturing memory of the process. Operating system failiure can generate crash dump file containing kernel memory.

  Beyond crashes, performance tuning can optimise system performance. Sometimes using *trace listings* of activities, recorded for analysis.

  Profiling: periodic sampling of instruction pointer to look for statistical trends

  **Kernighan's Law**: "Debugging is twice as hard as writing the code in the first place. Therefore if you write the code as cleverly as possible, you are, by definition, not smart enough to debug it"

### Performance Tuning

  Improve performance by removing bottlenecks.

  OS must provide means of computing and displaying measures of system behaviour. Eg, 'top' in linux or task manager in windows.

### Operating System Generation

  Operating systems are designed to run on any class of machines; the system must be configured for each specific computer site.

  **SYSGEN** program obtains information concerning the specific configuration of the hardware system.

  Its used to build system-specific compiled kernel or system-tuned kernel and therefore generates more efficient code than one general kernel.

  #### System Boot

  Once an operating system has been generated, it is made available for use by the hardware.

  Booting: The process of starting a computer by loading the kernel.

  When a CPU receives a reset event — for instance, when it is powered up or rebooted — the instruction register is  loaded with a predefined memory location and the execution starts there.

  A small piece of code - the bootstrap loader - stored in ROM or EEPROM locates the kernel, loads it into memory, and starts it.

  Sometimes this is a two step process where the boot block at a fixed location is loaded by the ROM code which loads the bootstrap loader from disk.

  Common bootstrap loader (boot loader), GRUB, allows selection of kernel from multiple disks, versions and kernel options.

  The kernel then loads and the system is running.
