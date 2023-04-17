# CSC 260 - Final Review 

[TOC]

# Study Guide

Based on the textbook "Operating System Concepts, 10th edition" by Abraham Silberschatz, Peter B. Galvin, and Greg Gagne, focusing on chapters 4 to 8 but also covering chapters 1 to 3.

#### Chapter 1: Introduction

- Understand what an operating system is and its various functions.
- Differentiate between kernel and user mode and the privileges associated with each.
- Describe the evolution of operating systems over time.
- Identify the various types of operating systems.

#### Chapter 2: Operating System Structures

- Understand the components of an operating system and their functions.
- Differentiate between monolithic, layered, microkernel, and hybrid operating system structures.
- Describe the system calls interface between user-level programs and the operating system.
- Explain how system calls are implemented.

#### Chapter 3: Processes

- Understand the concept of a process and its states.
- Differentiate between a process and a program.
- Describe how a process is represented in memory.
- Explain how processes are created and terminated.
- Identify the various scheduling algorithms and their pros and cons.

#### Chapter 4: Threads

- Understand the concept of a thread and how it differs from a process.
- Describe the benefits of using threads.
- Explain how threads are implemented in user-level and kernel-level threads.
- Identify the various types of thread models and their pros and cons.

#### Chapter 5: Synchronization

- Understand the problem of race conditions and how to prevent them.
- Identify the various synchronization mechanisms, such as locks, semaphores, and monitors.
- Explain how to implement mutual exclusion using locks.
- Describe how semaphores are used for synchronization.
- Understand the concept of a monitor and how it provides synchronization.

#### Chapter 6: Deadlocks

- Understand what a deadlock is and how it arises.
- Identify the necessary conditions for a deadlock to occur.
- Describe the various strategies for handling deadlocks, such as prevention, avoidance, and detection.
- Explain how the Banker's algorithm can be used to avoid deadlocks.

#### Chapter 7: Memory Management

- Understand the concept of virtual memory and how it is implemented.
- Describe the different memory allocation schemes, such as fixed partitioning, dynamic partitioning, and paging.
- Explain the concept of demand paging and its advantages.
- Understand how page replacement algorithms work, such as the optimal page replacement, FIFO, LRU, and clock algorithms.
- Describe how page faults are handled.

#### Chapter 8: Virtual Memory

- Understand how virtual memory is implemented using demand paging.
- Explain the various page replacement algorithms.
- Describe the advantages and disadvantages of using virtual memory.
- Understand how page faults are handled.
- Explain how swapping works and its relationship with virtual memory.

#### Study Tips:

- [x] Create flashcards for important concepts and definitions.
- [x] Work through practice problems and exercises at the end of each chapter.
- [x] Collaborate with classmates to review and discuss the material.
- [x] Take advantage of any online resources, such as lecture videos, tutorials, and discussion forums.
- [x] Practice coding examples to solidify understanding of concepts.
- [x] Review past exams and quizzes to familiarize yourself with the testing style of the class.

# Text book Questions and answers

These are practice questions straight out of the text book.

# 10th Edition

## Chapter 1 (Introduction)

- **Question**: What are the three main purposes of an operating system?

> **Answer:**
>
> The three main purposes are:
>
> 1. To provide an environment for a computer user to execute programs on computer hardware in a convenient and efficient manner.
> 2. To allocate the separate resources of the computer as needed to perform the required tasks. The allocation process should be as fair and efficient as possible.
> 3. As a control program, it serves two major functions: (1) supervision of the execution of user programs to prevent errors and improper use of the computer, and (2) management of the operation and control of I/O devices.

- **Question**: We have stressed the need for an operating system to make efficient use of the computing hardware. When is it appropriate for the operating system to forsake this principle and to “waste” resources? Why is such a system not really wasteful?

> **Answer:**
>
> Single-user systems should maximize use of the system for the user. A GUI might “waste” CPU cycles, but it optimizes the user’s interaction with the system.

- **Question**: What is the main difficulty that a programmer must overcome inwriting an operating system for a real-time environment?

> **Answer**:
> The main difficulty is keeping the operating system within the fixed time constraints of a real-time system. If the system does not complete a task in a certain time frame, it may cause a breakdown of the entire system. Therefore, when writing an operating system for a real-time system, the writer must be sure that his scheduling schemes don’t allow response time to exceed the time constraint.

- **Question**: Keeping in mind the various definitions of operating system, consider whether the operating system should include applications such as web browsers and mail programs. Argue both that it should and that it should not, and support your answers.

> **Answer**:
>
> An argument in favor of including popular applications in the operating system is that if the application is embedded within the operating system, it is likely to be better able to take advantage of features in the kernel and therefore have performance advantages over an application that runs outside of the kernel. 
>
> Arguments against embedding applications within the operating system typically dominate, however: 
>
> - the applications are applications—not part of an operating system, 
> - any performance benefits of running within the kernel are offset by security vulnerabilities, and
> - inclusion of applications leads to a bloated operating system.

- **Question**: How does the distinction between kernel mode and user mode function as a rudimentary form of protection (security)?

> **Answer**:
>
> The distinction between kernel mode and user mode provides a rudimentary form of protection in the following manner. Certain instructions can be executed only when the CPU is in kernel mode. Similarly, hardware devices can be accessed only when the program is in kernel mode, and interrupts can be enabled or disabled only when the CPU is in kernel mode. Consequently, the CPU has very limited capability when executing in user mode, thereby enforcing protection of critical resources.

- **Question**: Which of the following instructions should be privileged?
  - *Set value of timer.*
  - Read the clock.
  - *Clear memory.*
  - Issue a trap instruction.
  - *Turn off interrupts.*
  - *Modify entries in device-status table.*
  - Switch from user to kernel mode.
  - *Access I/O device.*

> **Answer**:
>
> The following operations need to be privileged: 
>
> - set value of timer
> - clear memory
> - turn off interrupts
> - modify entries in device-status table
> - access I/O device. 
>
> The rest can be performed in user mode.

- **Question:** Some early computers protected the operating system by placing it in a memory partition that could not be modified by either the user job or the operating system itself. Describe two difficulties that you think could arise with such a scheme.

> **Answer:** The data required by the operating system (passwords, access controls, accounting information, and so on) would have to be stored in or passed through unprotected memory and thus be accessible to unauthorized users.

- **Question:** Some CPUs provide for more than two modes of operation. What are two possible uses of these multiple modes?

> **Answer:** Although most systems only distinguish between user and kernel modes, some CPUs have supported multiple modes. Multiple modes could be used to provide a finer-grained security policy. For example, rather than distinguishing between just user and kernel mode, you could distinguish between different types of user mode. Another possibility would be to provide different distinctions within kernel code. For example, a specific mode could allow USB device drivers to run. This would mean that USB devices could be serviced without having to switch to kernel mode, thereby essentially allowing USB device drivers to run in a quasi-user/kernel mode.

- **Question:** Timers could be used to compute the current time. Provide a short description of how this could be accomplished.

> **Answer:** A program could use the following approach to compute the current time using timer interrupts. The program could set a timer for some time in the future and go to sleep. When awakened by the interrupt, it could update its local state, which it uses to keep track of the number of interrupts it has received thus far. It could then repeat this process of continually setting timer interrupts and updating its local state when the interrupts are actually raised.

- **Question:** Give two reasons why caches are useful. What problems do they solve? What problems do they cause? If a cache can be made as large as the device for which it is caching (for instance, a cache as large as a disk), why not make it that large and eliminate the device?

> **Answer:** Caches are useful when two or more components need to exchange data, and the components perform transfers at differing speeds. Caches solve the transfer problem by providing a buffer of intermediate speed between the components. If the fast device finds the data it needs in the cache, it need not wait for the slower device. The data in the cache must be kept consistent with the data in the components. If a component has a data value change, and the datum is also in the cache, the cache must also be updated. This is especially a problem on multiprocessor systems, where more than one process may be accessing a datum. A component may be eliminated by an equal-sized cache, but only if: (a) the cache and the component have equivalent state-saving capacity (that is, if the component retains its data when electricity is removed, the cache must retain data as well), and (b) the cache is affordable, because faster storage tends to be more expensive.

- **Question:** Distinguish between the client-server and peer-to-peer models of distributed systems.

> **Answer:** The client-server model firmly distinguishes the roles of the client and server. Under this model, the client requests services that are provided by the server. The peer-to-peer model doesn’t have such strict roles. In fact, all nodes in the system are considered peers and thus may act as either clients or servers—or both. A node may request a service from another peer, or the node may in fact provide such a service to other peers in the system. For example, let’s consider a system of nodes that share cooking recipes. Under the client-server model, all recipes are stored with the server. If a client wishes to access a recipe, it must request the recipe from the specified server. Using the peer-to-peer model, a peer node could ask other peer nodes for the specified recipe. The node (or perhaps nodes) with the requested recipe could provide it to the requesting node. Notice how each peer may act as both a client (it may request recipes) and as a server (it may provide recipes).

## Chapter 2 (Operating system Structures)

**Question:** What is the purpose of system calls?

> **Answer:** 
>
> System calls allow user-level processes to request services of the operating system.

**Question:** What is the purpose of the command interpreter? Why is it usually separate from the kernel?

> **Answer:** 
>
> It reads commands from the user or from a file of commands and executes them, usually by turning them into one or more system calls. It is usually not part of the kernel because the command interpreter is subject to changes.

**Question:** What system calls have to be executed by a command interpreter or shell in order to start a new process on a UNIX system?

> **Answer:** 
>
> A fork() system call and an exec() system call need to be performed to start a new process. The fork() call clones the currently executing process, while the exec() call overlays a new process based on a different executable over the calling process.

**Question:** What is the purpose of system programs?

> **Answer:**  
>
> System programs can be thought of as bundles of useful system calls. They provide basic functionality to users so that users do not need to write their own programs to solve common problems.

**Question:** What is the main advantage of the layered approach to system design? What are the disadvantages of the layered approach?

> **Answer:** 
>
> As in all cases of modular design, designing an operating system in a modular way has several advantages. The system is easier to debug and modify because changes affect only limited sections of the system rather than touching all sections. Information is kept only where it is needed and is accessible only within a defined and restricted area, so any bugs affecting that data must be limited to a specific module or layer. The primary disadvantage to the layered approach is the poor performance due to the overhead of traversing through the different layers to obtain a service provided by the operating system.

**Question:** List five services provided by an operating system, and explain how each creates convenience for users. In which cases would it be impossible for user-level programs to provide these services? Explain your answer.

> **Answer:** 
>
> The five services are:
>
> 1. Program execution. The operating system loads the contents (or sections) of a file into memory and begins its execution. A user-level program could not be trusted to properly allocate CPU time.
> 2. I/O operations. It is necessary to communicate with disks, tapes, and other devices at a very low level. The user need only specify the device and the operation to perform on it, and the system converts that request into device- or controller-specific commands. User-level programs cannot be trusted to access only devices they should have access to and to access them only when they are otherwise unused.
> 3. File-system manipulation. There are many details in file creation, deletion, allocation, and naming that users should not have to perform. Blocks of disk space are used by files and must be tracked. Deleting a file requires removing the name file information and freeing the allocated blocks. Protections must also be checked to assure proper file access. User programs could neither ensure adherence to protection methods nor be trusted to allocate only free blocks and deallocate blocks on file deletion.
> 4. Communications. Message passing between systems requires messages to be turned into packets of information, sent to the network controller, transmitted across a communications medium, and reassembled by the destination system. Packet ordering and data correction must take place. Again, user programs might not coordinate access to the network device, or they might receive packets destined for other processes.

**Question:** Why do some systems store the operating system in firmware, while others store it on disk?

> **Answer**: 
>
> For certain devices, such as embedded systems, a disk with a file system may be not be available for the device. In this situation, the operating system must be stored in firmware.

**Question**: How could a system be designed to allow a choice of operating systems from which to boot? What would the bootstrap program need to do?

> Answer: Consider a system that would like to run both Windows and three different distributions of Linux (for example, RedHat, Debian, and Ubuntu). Each operating system will be stored on disk. During system boot, a special program (which we will call the boot manager) will determine which operating system to boot into. This means that rather than initially booting to an operating system, the boot manager will first run during system startup. It is this boot manager that is responsible for determining which system to boot into. Typically, boot managers must be stored at certain locations on the hard disk to be recognized during system startup. Boot managers often provide the user with a selection of systems to boot into; boot managers are also typically designed to boot into a default operating system if no choice is selected by the user.

## Chapter 3 (Processes)

### 3.1 Process Concept

**Question**: Using the program shown in Figure 3.30, explain what the output will be at LINE A.

```c
#include <sys/types.h>
#include <stdio.h>
#include <unistd.h>

int value = 5;

int main() {
    pid_t pid;
    pid = fork();
    if (pid == 0) {
        /* child process */
        value += 15;
        return 0;
    } else if (pid > 0) {
        /* parent process */
        wait(NULL);
        printf("PARENT: value = %d",value); /* LINE A */
        return 0;
    }
}
```

> **Answer**: 
>
> The result is still 5, as the child updates its copy of value. When control returns to the parent, its value remains at 5.

### 3.2 Process Scheduling

**Question**: Including the initial parent process, how many processes are created by the program shown in Figure 3.31?

```C
#include <stdio.h>
#include <unistd.h>

int main() {
    /* fork a child process */
    fork();
    /* fork another child process */
    fork();
    /* and fork another */
    fork();
    return 0;
}
```

> **Answer**: 
>
> Eight processes are created.

### 3.3 Operations on Processes

**Question**: Original versions of Apple’s mobile iOS operating system provided no means of concurrent processing. Discuss three major complications that concurrent processing adds to an operating system.

> **Answer**: 
>
> - The CPU scheduler must be aware of the different concurrent processes and must choose an appropriate algorithm that schedules the concurrent processes.
> - Concurrent processes may need to communicate with one another, and the operating system must therefore develop one or more methods for providing interprocess communication.
> - Because mobile devices often have limited memory, a process that manages memory poorly will have an overall negative impact on other concurrent processes. The operating system must therefore manage memory to support multiple concurrent processes.

### 3.4 Interprocess Communication

**Question**: Some computer systems provide multiple register sets. Describe what happens when a context switch occurs if the new context is already loaded into one of the register sets. What happens if the new context is in memory rather than in a register set and all the register sets are in use?

> **Answer**: 
>
> The CPU current-register-set pointer is changed to point to the set containing the new context, which takes very little time. If the context is in memory, one of the contexts in a register set must be chosen and be moved to memory, and the new context must be loaded from memory into the set. This process takes a little more time than on systems with one set of registers, depending on how a replacement victim is selected.

**Question**: When a process creates a new process using the `fork()` operation, which of the following states is shared between the parent process and the child process?

1. Stack
2. Heap
3. Shared memory segments

> **Answer**:
>
>  Only the shared memory segments are shared between the parent process and the newly forked child process. Copies of the stack and the heap are made for the newly created process.

**Question**: Consider the “exactly once” semantic with respect to the RPC mechanism. Does the algorithm for implementing this semantic execute correctly even if the ACK message sent back to the client is lost due to a network problem? Describe the sequence of messages, and discuss whether “exactly once” is still preserved.

> **Answer**: 
>
> The “exactly once” semantics ensure that a remote procedure will be executed exactly once and only once. The general algorithm for ensuring this combines an acknowledgment (ACK) scheme combined with timestamps (or some other incremental counter that allows the server to distinguish between duplicate messages).
>
> The general strategy is for the client to send the RPC to the server
> along with a timestamp. The client will also start a timeout clock. The client will then wait for one of two occurrences: (1) it will receive an ACK from the server indicating that the remote procedure was performed, or (2) it will time out. If the client times out, it assumes the server was unable to perform the remote procedure, so the client invokes the RPC a second time, sending a later timestamp. The client may not receive the ACK for one of two reasons: (1) the original RPC was never received by the server, or (2) the RPC was correctly received—and performed—by the server but the ACK was lost. In situation (1), the use of ACKs allows the server ultimately to receive and perform the RPC. In situation (2), the server will receive a duplicate RPC, and it will use the timestamp to identify it as a duplicate so as not to perform the RPC a second time. It is important to note that the server must send a second ACK back to the client to inform the client the RPC has been performed.

Question: Assume that a distributed system is susceptible to server failure. What mechanismswould be required to guarantee the “exactly once” semantic for execution of RPCs?

> **Answer**:
>
> The server should keep track in stable storage (such as a disk log) of information regarding what RPC operations were received, whether they were successfully performed, and the results associated with the operations. When a server crash takes place and an RPC message is received, the server can check whether the RPC has been previously performed and therefore guarantee “exactly once” semantics for the execution of RPCs.

## Chapter 4 (Threads)

### 4.1 Overview

**Question**: Provide three programming examples in which multithreading provides better performance than a single-threaded solution.

> **Answer**:
>
>  a. A web server that services each request in a separate thread b. A parallelized application such as matrix multiplication where various parts of the matrix can be worked on in parallel c. An interactive GUI program such as a debugger where one thread is used to monitor user input, another thread represents the running application, and a third thread monitors performance

### 4.2 Multicore Programming

**Question**: Using Amdahl’s Law, calculate the speedup gain of an application that has a 60 percent parallel component for (a) two processing cores and (b) four processing cores.

> **Answer**: 
>
> a. With two processing cores, we get a speedup of 1.42 times. b. With four processing cores, we get a speedup of 1.82 times.

### 4.3 Multithreading Models

**Question**: Does the multithreaded web server described in Section 4.1 exhibit task or data parallelism?

> **Answer**: 
>
> Data parallelism. Each thread is performing the same task, but on different data.

### 4.4 Thread Libraries

**Question**: What are two differences between user-level threads and kernel-level threads? Under what circumstances is one type better than the other?

> **Answer**: 
>
> a. User-level threads are unknown by the kernel, whereas the kernel is aware of kernel threads. b. On systems using either many-to-one or many-to-many model mapping, user threads are scheduled by the thread library, and the kernel schedules kernel threads. c. Kernel threads need not be associated with a process, whereas every user thread belongs to a process. Kernel threads are generally more expensive to maintain than user threads, as they must be represented with a kernel data structure.

### 4.5 Implicit Threading

**Question**: Describe the actions taken by a kernel to context-switch between kernel-level threads.

> **Answer**: 
>
> Context switching between kernel threads typically requires saving the value of the CPU registers from the thread being switched out and restoring the CPU registers of the new thread being scheduled.

### 4.6 Threading Issues

**Question**: What resources are used when a thread is created? How do they differ from those used when a process is created?

> **Answer**: 
>
> Because a thread is smaller than a process, thread creation typically uses fewer resources than process creation. Creating a process requires allocating a process control block (PCB), a rather large data structure. The PCB includes a memory map, a list of open files, and environment variables. Allocating and managing the memory map is typically the most time-consuming activity. Creating either a user thread or a kernel thread involves allocating a small data structure to hold a register set, stack, and priority.

### 4.7 Operating-System Examples

**Question**: Assume that an operating system maps user-level threads to the kernel using the many-to-many model and that the mapping is done through LWPs. Furthermore, the system allows developers to create real-time threads for use in real-time systems. Is it necessary to bind a real-time thread to an LWP? Explain.

> **Answer**: 
>
> Yes. Timing is crucial to real-time applications. If a thread is marked as real-time but is not bound to an LWP, the thread may have to wait to be attached to an LWP before running. Consider a situation in which a real-time thread is running (is attached to an LWP) and then proceeds to block (must perform I/O, has been preempted by a higher-priority real-time thread, is waiting for a mutual exclusion lock, etc.). While the real-time thread is blocked, the LWP it was attached to is assigned to another thread. When the real-time thread has been scheduled to run again, it must first wait to be attached

## Chapter 5 (CPU Scheduling)

## Chapter 6 (Process Synchronization)

## Chapter 7 (Deadlocks)

## Chapter 8 (Memory Management)

