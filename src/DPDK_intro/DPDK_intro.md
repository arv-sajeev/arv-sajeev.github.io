# Data Plane Development Kit 

The DPDK is an open source framework that is used for ultra fast processing of packet data. It comprises of a large number of abstractions 
and libraries that can be used to program environments ranging from specialized packet processing hardware to commodity systems. It's claim 
to fame are the highly optimized drivers support along with the option for userspace packet processing. DPDK has extensive support for a wide variety 
of architectures almost everyone that is out there. The enviroment is set up while building the tool in the device.

---

## Environment Abstraction Layer

The EAL like the name suggests abstracts the hardware and software layers below to give a unified interface to write DPDK programs on, a part of the EAL is 
set up when making the software to suit the particular architecture that the application is to run on. Other application specific details can be provided via
command line during the initializing step. The application can be considered to be a monolithic one with all the DPDK libraries included along with the EAL and 
other application specific code. __Userspace drivers are setup while making and these rely on magic numbers of the kernel to ensure compatibility, updates to 
your linux system can result in a mismatch in the numbers resulting in errors like, invalid module format for you .ko files__.

The EAL in addition to hiding the architecture details provides the following services to DPDK applications

* Loading and initializing a DPDK application
* Seamlessly handling multiprocessor, multi-core by abstracting them as lcore's as assigning core affinity
* allocating memory during initialization
* atomic and lock primitives
* access to the PCI bus
* interrupt handling
* memory management ( instead of malloc )


## Core libraries

In addition to the EAL, DPDK has a set of core libraries that 

### Ring manager

DPDK uses a ring structure that provides a lockless multi-producer, multi-consumer queue. It is optimized for bulk operations and being lockless 
is much faster and has lesser bottlenecks. They are intended for communication between different lcores.

### Memory pool manager

The applications allocates the memory during initialization and is then reserved as pools of objects in memory, a pool can be used to to store 
objects and uses the ring abstraction, it also has finer details like per core cache to ensure optimum use of memory.

### Network Packet Buffer management

The packets received and created can be stored in buffers, these have extra memory allocated to them, so mutation just required simple pointer arithmetic and 
not copying of the entire packet to new buffer, optimized for bulk large sized network packets with meta-data as well

### Timer Manager

The timer service provided can be used to execute functions on timer expiry, it can be repetitive count downs or single shot actions, timers are initiated per lcore 
but use the primitives provided by the eal

### Poll mode driver

Poll mode drivers are used instead of interrupt based drivers as packets are very frequent and each arrival would set of an interrupt resulting in numerous interrupts and hence context switches
this would turn out to be a bottle neck, instead poll mode drivers are used, avoiding context switches and constant streams of interrupts.

---

## Conclusion

DPDK is a framework with quite a large number of tools and just one post can't do justice to it. For me personally it was one of the first such platform 
I ever used. The API and documentation are extremely well done and given some time and effort the seemingly cryptic example code start to make sense. One of the 
major obstacle I faced was setting it up on my PC, and errors that happened to the uio drivers each time my linux had an update, more on that some other day.


