# Chapter 2

## Operating System Services

Operating systems provide an environment for execution of programs and services to programs and users
### Application programs
One set of operating-system services provide functions that are helpful to the user:
  * User Interface - Almost all operating systems have a UI. Either GUI or CLI or Batch(?)
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

  ### System Call Implementation

  Typically, a number is associated with each system call.
  **The system call interface** maintains a table indexed according to these numbers.

  The system call interface invokes the intended system call in OS kernel and returns the status of the system call and any return values

  The caller needs to know nothing about how the system call is implemented. It just needs to obey the API and understand what the OS will do as a result call.

  More details of OS interface is hidden from the programmer by the API; and is managed by run-time support library (Set of functions built into libraries included with compiler)

  #### API - System Call - OS Relationship

  ![syscall_API](/home/minad/Pictures/syscall_api.png)

  ### System Call Parameter Passing

  Often more information is required than simply the identity of the desired system call. The exact type and amount of information vary according to OS and call.

  Three general methods are used to pass parameters to the OS:
    * Simplest way - Pass the parameters in registers. In some cases however there may be more parameters than registers.
    * Parameters stored in a block or table in memory and the address is passed as a parameter in a register. This is the approach taken by Linux and Solaris
    * Parameters are placed or **pushed onto** the stack by the **program** and **popped off** the stack by the **operating system**.
    * Block and stack methods don not limit the number or length o parametes being passed.

    #### Parameter passing via Table
    ![parameter_table](parameter_table.png)

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
            - get/set time or date
            - get/set system data
            - get/set process file or device attributes
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

    ### System programs
      System programs provide a convenient environment for program development and execution. They can be divided into:
        * File manipulation
        * Status information sometimes stored in a file modification
        * Programming language support
        * Program loading and execution
        * Communications
        * Background services
        * Application programs

      Most users' view of the operating system is defined by the system programs not the actual system calls.
