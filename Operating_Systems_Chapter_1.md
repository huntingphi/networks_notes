# Operating Systems Chapter 1
  ## What is an Operating System?
  A program that acts as an intermediary between a user of a computer and the hardware

  The OS is a resource allocator:
  * It manages all resources
  * Decides between conflicting requests to provide fair and efficient resource use

  There is no universally accepted definition.

  The **kernel** is the one program running at all times on the computer.

  Everything else is either a system program or an application program.

  Operating System Goals:
  - Execute user programs and make solving user problems easier
  - Make the computer system convenient to use
  - Use computer hardware in an efficient manner

  ### Four Components of a Computer System
    1. Hardware
    2. Operating System
    3. System and application programs
    4. Users?

  ### Computer Startup

  At power-up or reboot the bootstrap program is loaded.
  This program is typically stored in the ROM or EPROM and known as the firmware

  ### Computer System Operation

  One or more CPUs or device controllers connect through common bus providing access to shared memory

  Concurrent execution of CPUs and devices competing for memory cycles

  I/O devices and the CPU can execute concurrently.
  Each device controller is in charge of a particular device type.
  Each device controller has a local buffer.
  CPU moves data from/to main memory, to/from local buffers.
  I/O is from the device to the local buffer of controller.
  Device controller informs CPU that it has finished its operation by causing an interrupt.

  ### Common Functions of Interrupts

  Typically transfers the control to the interrupt service routine. This is generally done through the interrupt vector which contains the addresses of all the service routines.

  Interrupt architecture must save the address of the interrupted instruction

  A **trap** or **exception** is a software-generated interrupt caused either by an error or a user request

  An operating system is **interrupt driven**

  ### Interrupt Handling

  The operating system preserves the state of the CPU by storing registers and the program counter.
  It determines what type of interrupt has occurred (through):
  1. Polling
  2. Vectored interrupt system

  Separate segments of code determine what action should be taken for each type of interrupt

  ### Storage Structure *Out of Scope*

    1. Main memory - The only large storage media that the CPU can access directly. It has **random** access and is typically **volatile**
    2. Secondary storage - extension of main memory that provides large non-volatile storage capacity.
