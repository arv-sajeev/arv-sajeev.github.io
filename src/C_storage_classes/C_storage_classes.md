# C storage classes, use them right !!

It's been almost two months into my new job at Altran, the work here is substantially different to what I used to do before. 
While enjoyable it's got quite a steep learning curve, but one obstacle that I didn't expect was the language being used,*C*. 
C was the first (programming) language that I'd ever seriously learnt. Being a biology student in high school, I only got the chance to get started with programming
around the second year of my college education for Data structures lab. I think C should be definitely be the first choice  unlike languages like python or Java since it helps the student 
understand about how a computer works and grasp concepts like the [Von-Neumann](https://en.wikipedia.org/wiki/Von_Neumann_architecture) architecture of computers. 

I did quite well in DSA lab and was able to most of my work without copying it from my friends which I then saw as a huge achievement given that most of my classmates had taken Computer Science for high school. With the confidence from college and my experience doing a significantly large [project](https://github.com/arv-sajeev/pfm) in C, I was disappointed in finding myself struggling to go through the code in my project and also bombing couple of interviews due to me lacking in certain fundamental concepts. Here is a post that I made as a reference for future me and maybe some of you who underestimated the power of C, hope it helps.

---

## Layout of a program in memory

I hope that you are familiar with the fact that programs written in C, C++ and other similar languages are compiled or converted into a format readable by the CPU
before running them. While compiling them different parts of the program get mapped to different sections or segments depending on the intended behaviour. Segmentation is a very 
important virtual memory management technique used to manage the memory of a program by mapping the different sections of a program to different address spaces, making it easier to manage
their different behaviours and provide protection from the different sections from interfering with each other. Access to the different sections are typically done employing separate base address 
registers to access the address space. Now let's go ahead to the different types of segments

### Code segment

This segment is the part of the object file that will contain the actual "code" or instructions that you have written in your program with references to the memory and other variables as names. It is generally
read only and protected so that no changes to the code are made during execution. With this part being read only it is even possible to store in ROM 

### Data segment

This section contains any data that is global and is initialized with a value. A global data location as we shall see later on is one that can be accessed from within any point in the program and whose value
generally last throughout the lifetime of the program. This values are non constant and can be modified later on

* data locations that are global and are initialized
* their values can be modified later on
* lasts throughout the life of the program
* during compilation, their values are first stored as part of the text segment but memory is allocated in data segment, it only during loading that the values of these data location are copied from code segment to data segments

### Block Starting symbol segment

The BSS segment is very similar to the data segment and differs only in the fact that it holds global data that isn't explicitly initialized. It's owes it's weird name to the first
specialised assembly instruction that allowed to declare a separate location for uninitialized data and hence saving space is the object file.

* data locations are global and are uninitialized
* most languages provide a default implicit initialization of all 0's
* lasts throughout the life of the program
* during compilation only the names to the locations are stored in text and the length of the total bss segment is provided, it is during loading that space is allocated using the length provided and initialized to 0's


### Stack segment 

This is probably the segment you've used the most. Any data that is local to a function is stored in the stack segment within a stack frame for maintaining the scope of that function. 
The LIFO behaviour of the stack makes it the appropriate segment to store local data that who's lifetime are bound to how long the function that they are in exists. 

* the data locations are local and uninitialized, no implicit initialization so locations maybe filled with garbage values
* C and similar languages do not prevent you from accessing uninitialized data locations, may results in bugs due to garbage values
* The stack segment is maintained using separate pointers to point to the top of the stack and the base of the stack frame of the function call currently in scope
* In the x86 architecture, the stack grows downward towards lower addresses
* the heap and stack segments grow towards each other

### Heap segment

This segment is used for dynamic allocation of memory, the program can use libc functions like malloc etc. to allocate custom chunks of memory. The
OS uses efficient algorithms to assign memory minimizing fragmentation. Extra memory can be allocated to the heap segments using system calls 

* the data locations come into scope when explicitly allocated and out of scope when explicitly deallocated or to the end of the program
* the heap grows from the end of the data segment towards the stack segment

---













