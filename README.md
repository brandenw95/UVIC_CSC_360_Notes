# (CSC 360) - Operating Systems - Complete Notes - Spring 2023

[TOC]

# Class Intro

> Prof:
>
> Email:
>
> Office Hours:

> All chapters covered:  ("//" = split in slides)
>
> - **Chapter 1 - Overview of operating systems**
>   - 1.1, 1.2, 1.3 // 1.4, 1.5, 1.6, 1.7, 1.8
> - **Chapter 2 - Operating system structures**
>   - 2.1, 2.2, 2.3, 2.4, 2.5 // 2.6, 2.7
> - **Chapter 3 - Process Management**
>   - 3.1, 3.2, 3.3, 3.4 // 3.5
> - **Chapter 4 - Threads**
>   - 4.1, 4.2, 4.3, 4.4, 4.6
> - **Chapter 5 - Process Synchronization**
>   - 5.1, 5.2, 5.3 // 5.5, 5.6, 5.7
>
> 
>
> Formatting Legend: 
>
> - Chapters = H1
> - Sub headings (ex, 1.1, 1.2, etc.) = H2
> - Sub-Sub heading titles = H4

# Midterm Review

Midterm info:

> **Date:** 
>
> **Time:** 
>
> **Location:** 

![image-20221021130714300](assets/image-20221021130714300.png)

Content - Concepts portion

> - batching processing
> - time sharing
> - Process states and the state transition diagram
> - Process tree
> - Mode switch
> - Context switch
> - IPC with message passing
> - IPC with shared memory
> - Thread
> - User thread vs. kernel thread
> - Thread model
> - The critical section problem and algorithms to solve the problem
> - Understand how to use mutex, semaphore, condition variable

Content - Programming Understanding portion

> - **Processes**
>   - fork()
>   - wait(), waitpid(), kill()
>   - exec*() family
> - **Shared Memory**
>   - shmget(), shmat(), shmdt()
> - **Threads**
>   - pthread_attr_init(), pthread_create( ), pthread_join ()
> - **Semaphores, Conditional variables, mutex, monitors and Proc. sync.**
>   - sem_init(), sem_wait(), sem_post(), pthread_mutex_lock(), pthread_mutex_unlock, pthread_cond_wait(), pthread_cond_signal()

### batching processing

Batch processing is a technique in which an Operating System collects the programs and data together in a batch before processing starts.

![image-20221022163849827](assets/image-20221022163849827.png)

The operatig systems does the following BEFORE the processing starts:

- OS defines a Job to be started and combines resources in a single unit
- OS keeps a number of jobs in memeory and just excecutes them
- Jobs are processed sequentially (First come first serve)
- job gets released from memory and a copy is saved for later

| Advantages                                                   | Disadvantages                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Batch processing takes much of the work of the operator to the computer. | Difficult to debug program.                                  |
| Increased performance as a new job get started as soon as the previous job is finished, without any manual intervention. | A job could enter an infinite loop.                          |
|                                                              | Due to lack of protection scheme, one batch job can affect pending jobs. |

#### Multiprogramming

Sharing the processor, when two or more programs reside in memory at the same time.

![image-20221022164509251](assets/image-20221022164509251.png)

**An OS does the following activities related to multiprogramming:**

- The operating system keeps several jobs in memory at a time.
- This set of jobs is a subset of the jobs kept in the job pool.
- The operating system picks and begins to execute one of the jobs in the memory.
- Multiprogramming operating systems monitor the state of all active programs and system resources using memory management programs to ensures that the CPU is never idle, unless there are no jobs to process.

| Advantages                                                   | Disadvantages                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| High and efficient CPU utilization.                          | CPU scheduling is required.                                  |
| User feels that many programs are allotted CPU almost simultaneously. | To accommodate many jobs in memory, memory management is required. |
|                                                              |                                                              |



### Time Sharing (Multitasking)

Multitasking is when multiple jobs are executed by the CPU simultaneously by switching between them. 

![image-20221022164221070](assets/image-20221022164221070.png)

**An OS does the following activities related to multitasking:**

- The user gives instructions to the operating system or to a program directly, and receives an immediate response.
- The OS handles multitasking in the way that it can handle multiple operations/executes multiple programs at a time.
- Multitasking Operating Systems are also known as Time-sharing systems.
- These Operating Systems were developed to provide interactive use of a computer system at a reasonable cost.
- A time-shared operating system uses the concept of CPU scheduling and multiprogramming to provide each user with a small portion of a time-shared CPU.
- Each user has at least one separate program in memory.
- A program that is loaded into memory and is executing is commonly referred to as a **process**.
- When a process executes, it typically executes for only a very short time before it either finishes or needs to perform I/O.
- Since interactive I/O typically runs at slower speeds, it may take a long time to complete. During this time, a CPU can be utilized by another process.
- The operating system allows the users to share the computer simultaneously. Since each action or command in a time-shared system tends to be short, only a little CPU time is needed for each user.
- As the system switches CPU rapidly from one user/program to the next, each user is given the impression that he/she has his/her own CPU, whereas actually one CPU is being shared among many users.

### Process states and the state transition diagram

> **Process State**
>
> The state may be new, ready, running,waiting,halted, and so on.

A process may be in one of the following states:

- **New** - Process has been created
- **Running** - Instructions are being executed.
- **Waiting** - The process is waiting for some event to occur (I/O or Reception)
- **Ready** - The process is waiting to be assigned by the processor
- **Terminated** - The process has finished execution.

> ***Note:*** That only **one** process can be running on any processor at any instant.

![image-20221022165137887](assets/image-20221022165137887.png)

NEW => READY => RUNNING => TERMINATED

NEW => READY => RUNNING => INTURUPTED => READY

NEW => READY => RUNNING => WAITING => READY

> **Program Counter**
>
> The counter indicates the address of the next instruction to be executed for this process.

> **CPU-scheduling information**
>
> Includes a process priority, pointers to scheduling queues, and any other scheduling parameters.

### Process Tree

The creating process is called a parent process, and the new processes are called the children of that process. Each of these new processes may in turn create other processes, forming a tree of processes.

![image-20221022170124300](assets/image-20221022170124300.png)

- A child process may be able to obtain its resources **directly from the operating system**, or it may be constrained to a subset of the **resources of the parent process**. 
- he parent process might pass down information to the child process.

**When a process creates a new process, two possibilities for execution exist:**

1. The parent continues to execute concurrently with its children.
2. The parent waits until some or all of its children have terminated.

**There are also two address-space possibilities for the new process:**

1. The child process is a duplicate of the parent process (it has the same program and data as the parent).
2. The child process has a new program loaded into it.

**A parent may terminate the execution of one of its children beacuse:**

- The child has <u>exceeded its usage of some of the resources</u> that it has been allocated. (To determine whether this has occurred, the parent must have a mechanism to inspect the state of its children.)
- The task assigned to the child is <u>no longer required.</u>
- <u>The parent is exiting</u>, and the operating system does not allow a child to continue if its parent terminates.

### Mode switch (User vs kernel mode)

### Context switch

> **Process Control Block (PCB)**
>
> A block of memory that hold information about the current process.

![image-20221022172022654](assets/image-20221022172022654.png)

**Each PCB can hold info on the following:**

- <u>Process state</u> - State status
- <u>Program counter</u> - address to the next instruction
- <u>CPU Registers</u> - Any allocated registers
- <u>CPU-Scheduling</u> - Process pointers and priority
- <u>Memory management info</u> - contains different memory table info
- <u>accounting information</u> - Realtime limits
- <u>I/O status</u> - List of all IO allocated

![image-20221022172428047](assets/image-20221022172428047.png)

Context switching is loading Info into the PCB and passing it from process to processes.

![image-20221022172552250](assets/image-20221022172552250.png)

### IPC (Inter-process Communication) with message passing

> **IPC** **(Inter-process Communication)**
>
> Cooperating processes require an inter-process communication (IPC) mechanism that will allow them to exchange data and information.

> **Message Passing**
>
> message-passing model, communication takes place by means of messages exchanged between the cooperating processes.
>
> Message passing provides a mechanism to allow processes to communicate and to synchronize their actions without sharing the same address space.

| Advantages                                    | Disadvantages                                            |
| --------------------------------------------- | -------------------------------------------------------- |
| Useful for sending small amounts of data/info | Slow - Uses system calls instead of shared memory space. |
| Easier to implement                           |                                                          |
| No need to share address space                |                                                          |

![image-20221022173321505](assets/image-20221022173321505.png)

- Messages sent by a process can be either fixed or variable in size.
  - If only fixed-sized messages can be sent, the system-level implementation is straightforward. 
    - makes the task of programming more difficult

**Methods for logically implementing a link and the send()/receive() operations:**

- Direct or indirect communication.
- Synchronous or asynchronous communication.
- Automatic or explicit buffering.

#### Naming

Processes that want to communicate must have a way to refer to each other. They can use either direct or indirect communication.

##### Direct Communication

Explicitly name the recipient or sender of the communication needed.

```c
send(P, message); // Send message to process P
recieve(Q, message); // Recieve messages from process Q
```

**Communication link Properties:**

- A link is established automatically between every pair of processes

- A link is associated with exactly two processes.
- Between each pair of processes, there exists exactly one link.

**Asymmetric vs Symmetric** 

```c
//Symmetric
send(P, message); // Send message to process P.
recieve(Q, message); // Recieve messages from process Q.
```

```c
// Asymmetric
send(P, message); // Send message to process P.
recieve(message); // Recieve messages without explicit naming of sender.
```

The disadvantages of symmetric and asymmetric is a <u>lack of modularity</u> .

##### Indirect Communication

messages are sent to and received from mailboxes, or ports.

```c
send(A, message); // Send message to mailbox A
recieve(A, message); // Recieve messages from mailbox A
```

**Communication link Properties:**

- A link is established between a pair of processes only if both members of the pair have a shared mailbox.
- A link may be associated with more than two processes.
- Between each pair of communicating processes, a number of different links may exist, with each link corresponding to one mailbox.

***Ex. Who gets the message?***

```c
P1 => send(A, message); // Send message to mailbox A
P2 => recieve(A, message); // Recieve messages from mailbox A
P3 => recieve(A, message); // Recieve messages from mailbox A
```

**The answer depends on which of the following methods we choose:**

- Allow a link to be associated with two processes at most.
- Allow at most one process at a time to execute a receive() operation.
- Allow the system to select arbitrarily which process will receive the message (that is, either P2 or P3,but not both, will receive the message).The system may define an algorithm for selecting which process will receive the message (for example, *round robin*, where processes take turns receiving messages). The system may identify the receiver to the sender.

**If the mailbox is owned by:**

- **Operating system** - Must provide a mechanism to do the following
  - Create a new mailbox.
  - Send and Receive through the mailbox.
  - Delete the mailbox.
- **Process** - We distinguish between the owner (which can only receive messages through this mailbox) and the user (which can only send messages to the mailbox).

#### Synchronization of Process Communication

Communication takes place between two system calls:

- Send()
- Receive()

**Blocking (synchronous) vs Non-blocking (Asynchronous)**

- **Blocking**
  - **Send** - The sending process is blocked until the message is received by the receiving process or by the mailbox.
  - **Receive** - The receiver blocks until a message is available.
- **Non-Blocking**
  - **Send** - The sending process sends the message and resumes operation.
  - **Receive** - The receiver retrieves either a valid message or a null.

#### Buffering

Whether communication is direct or indirect, messages exchanged by communicating processes reside in a temporary queue.

**queues can be implemented in three ways:**

- **Zero capacity** **(NON BUFFERING)** - The queue has a maximum length of zero; thus,the link cannot have any messages waiting in it. In this case, the sender must block until the recipient receives the message.
- **Bounded capacity** **(BUFFERING)** - The queue has finite length n; thus, at most n messages can reside in it.
- **Unbounded capacity** **(BUFFERING)** - The queue’s length is potentially infinite; thus, any number of messages can wait in it. The sender never blocks.

### IPC (Inter-process Communication) with shared memory

> **Shared Memory**
>
> A region of memory that is shared by cooperating processes is established. 

- The operating system tries to prevent one process from accessing another process’s memory. Shared memory requires that two or more processes agree to <u>remove this restriction</u>.
- The Process is also responsible for ensuring that they are not writing to the same location simultaneously.

#### Producer - Consumer Problem 

A <u>producer</u> process produces information that is consumed by a <u>consumer</u> process.

The producer and consumer must be synchronized, so that the consumer does not try to consume an item that has not yet been produced.

##### Bounded vs Unbounded Buffers

- **Unbounded -** Places no practical limit on the size of the buffer.
- **Bounded -** Assumes a fixed buffer size. 

### Threads

#### Overview

> **Thread**
>
> A thread is a basic unit of CPU utilization and comprises of:
>
> - Thread ID
> - Program Counter
> - A register Set
> - A stack data structure

##### Motivation

![image-20221022184149838](assets/image-20221022184149838.png)

With threading a single application or program can:

- Display images.
- display text.
- Retrieve data from the network.
- And more.

Like IPC threads help RPC servers handle concurrent connections.

![image-20221022184424212](assets/image-20221022184424212.png)

##### Benefits of threading

- **Responsiveness** - keep program running even if blocked.
- **Resource sharing** - threads share the memory and the resources of the process to which they belong by default.
- **Economy** - because threads share the resources of the process to which they belong, it is more economical to create and context-switch threads.
- **scalability** - threads may be running in parallel on different processing cores.

#### Multicore Programming

![image-20221022184810891](assets/image-20221022184810891.png)

##### Challenges in multicore Programming

five areas present challenges in programming for multicore
systems:

- **Identifying tasks -**  examining applications to find areas that can be divided into separate
- **Balance -** ensure that the tasks perform equal work of equal value.
- **Data Splitting -** data accessed and manipulated by the tasks must be divided to run on separate cores.
- **Data dependency -** the data accessed by the tasks must be examined for dependencies between two or more tasks.
- **Testing a debugging -** when a program is running in parallel on multiple cores, many different execution paths are possible.

##### Types of Parallelism (Data vs Task)

> **Data Parallelism**
>
> Focuses on distributing subsets of the same data across multiple computing cores and performing the same operation on each core.

> **Task Parallelism**
>
> Involves distributing not data but tasks (threads) across
> multiple computing cores. Each thread is performing a unique operation.

### User thread vs. kernel thread

> **User Threads**
>
> User threads are supported above the kernel and are managed without kernel support.

> **Kernel Threads**
>
> kernel threads are supported and managed directly by the operating system.
>
> ***Note:*** Windows, Linux, Mac OS X,and Solaris - support kernel threads.

### Thread model

A relationship must exist between user threads and kernel threads. 3 common ways:

- many-to-one model
- one-to-one model
- many-to-many model

#### Many-to-One Model 

![image-20221022190333089](assets/image-20221022190333089.png)

> Maps many user-level threads to one kernel thread.

| Advantages                                                   | Disadvantages                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Thread management is done by the thread library in user space, so it is efficient | The entire process will block if a thread makes a blocking system call. |
|                                                              | because only one thread can access the kernel at a time,multiple threads are unable to run in parallel on multicore systems. |



#### One-to-One Model

![image-20221022190345411](assets/image-20221022190345411.png)

>   Maps each user thread to a kernel thread.

| Advantages                                                   | Disadvantages                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| It provides more concurrency than the many-to-one model by allowing another thread to run when a thread makes a blocking system call. | creating a user thread requires creating the corresponding kernel thread. (High overhead) |
| Allows multiple threads to run in parallel on multiprocessors. |                                                              |



#### Many-to-Many Model

![image-20221022191444594](assets/image-20221022191444594.png)

> multiplexes many user-level threads to a smaller or equal number of kernel threads.

| Advantages                                                   | Disadvantages |
| ------------------------------------------------------------ | ------------- |
| Developers can create as many user threads as necessary.     | None          |
| Corresponding kernel threads can run in parallel on a multiprocessor. | None          |
| when a thread performs a blocking system call, the kernel can schedule another thread for execution. | None          |

### The critical section problem and algorithms to solve the problem

> **Critical Section Problem**
>
> used to design a protocol followed by a group of processes, so that when one process has entered its critical section, no other process is allowed to execute in its critical section.

> **critical section**
>
> The segment of code where processes access shared resources, such as common variables and files, and perform write operations on them.

```c
While(True){
    // Aquire lock
    
    // Critical section
    
    // Disable lock
    
    // Exit()
}
```



#### Solutions to the critical section problem

##### Methods

- **Mutual Exclusion** - When one process is executing in its critical section, no other process is allowed to execute in its critical section.
- **Progress** - When no process is executing in its critical section, and there exists a process that wishes to enter its critical section, it should not have to wait indefinitely to enter it.
- **Bounded waiting** - There must be a <u>bound on the number of times a process is allowed to execute</u> in its critical section, after another process has requested to enter its critical section and before that request is accepted.

##### Implementations (Locks)

- **test_and_set** - Uses boolean True/False.
- **Mutex Locks** - Software method that provides the `acquire()` and `release()` functions that execute atomically.
- **Semaphores** - More sophisticated methods that utilize the `wait()` and `signal()` operations that execute atomically on Semaphore `S`, which is an integer variable.
- **Conditional Variables** -  Utilizes a queue of processes that are waiting to enter the critical section.

### Understand how to use mutex, semaphore, condition variable (IMPORTANT)

#### Mutex

> **Mutex (Mutual Exclusion)**
>
> to protect critical regions and thus prevent race conditions. Also called a spin lock.

- A process that attempts to acquire an unavailable lock is blocked until the lock is released.

```C
//aquire a lock
Aquire(){
    while(!available){
        //Wait
        available = false;
    }
}

// release a lock
release(){
    available = true;
}
```

| Advantages                                                   | Disadvantages                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Easy to implement                                            | Requires busy waiting - other processes loop waiting for the mutex to unlock |
| no context switch is required when a process must wait on a lock |                                                              |

#### Semaphores

> Semaphores
>
> is an integer variable that, apart from initialization, is accessed only through two standard atomic operations:
>
> - wait()
> - Signal()

```c
//Definition of wait()
wait(S){
    while(S <= 0){
        //busy wait
        S--;
    }
}
```

```c
//Definition of Signal()
signal(S){
    S++;
}
```

- All modifications to the integer value of the semaphore in the wait() and signal() operations must be executed indivisibly. That is, when one process modifies the semaphore value, no other process can simultaneously modify that same semaphore value. 
- Signal and wait must be executed without interruption.

> **Counting semaphores**
>
> The value of a counting semaphore can range over an unrestricted domain.

> **Binary semaphores**
>
> The value of a binary semaphore can range only between 0 and 1.

#### Conditional variables

##### Why conditional variables instead of mutex?

When we need to conditionally check if some condition is met, we do not want to use spin look because it wastes CPU time. A condition variable is a way to achieve the same goal without continually polling.

- Always use in conjunction to a mutex lock

## // Programming Section

### Processes

#### fork()

> **Description:**
>
>  Fork system call is used for creating a new process, which is called ***child process\***, which runs concurrently with the process that makes the fork() call (parent process). After a new child process is created, both processes will execute the next instruction following the fork() system call. A child process uses the same pc(program counter), same CPU registers, same open files which use in the parent process.
>
> **Return Integer Values:**
>
> - **Negative** - creation of child process failed
> - **Zero** - Created a child process successfully
> - **Positive** - Returned to parent or caller, the positive  return number is the PID of the child process.
>
> **Note:** Will not run on a Windows environment.

##### Fork the current program

```c
#include <stdio.h> 
#include <sys/types.h> 
#include <unistd.h> 
#include <stdlib.h>
int main() 
{ 
    fork(); 
    fork(); 
    fork(); 
    printf("Hello world!\n");
    return 0; 
} 
```

Output:

```bash
Hello world!
Hello world!
Hello world!
Hello world!
Hello world!
Hello world!
Hello world!
Hello world!
```

##### Fork with Child/Parent conditional

```c
#include <sys/types.h>
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>
#include <stdlib.h>
int main(int argc, char *argv[])
{
   printf("I am: %d\n", (int) getpid());

   pid_t pid = fork();
   printf("fork returned: %d\n", (int) pid);

   if (pid < 0) { /* error occurred */
   	perror("Fork failed");
   }
   if (pid == 0) { /* child process */
   	printf("I am the child with pid %d\n", (int) getpid());
               printf("Child process is exiting\n");
               exit(0);
       }
   /* parent process */
   printf("I am the parent waiting for the child process to end\n");
       wait(NULL);
       printf("parent process is exiting\n");
       return(0);
}
```

Output:

```bash
I am: 2337
fork returned: 2338
I am the parent waiting for the child process to end
fork returned: 0
I am the child with pid 2338
Child process is exiting
parent process is exiting
```

#### wait()

> **Description:**
>
> A call to wait() blocks the calling process until one of its child processes exits or a signal is received. After child process terminates, parent ***continues*** its execution after wait system call instruction. Child process may terminate due to any of these:
>
> - It calls exit();
> - It returns (an int) from main
> - It receives a signal (from the OS or another process) whose default action is to terminate.
>
> **Parameters:**
>
> - NULL -  indicates that we will not know about the state of the child’s process’s transition.
>
> **Return Values:**
>
> - No return values
>
> **Note:** This call placed into the parent section of code makes the child go/run first.

```c
#include <unistd.h>
#include <sys/types.h>
#include <stdio.h>
#include <sys/wait.h>
int main(){
    pid_t p;
    printf("Before fork\n");
    p=fork();
    //Child
    if(p==0){
        printf("I am a child");
        printf("my parent is waiting");
    }
    //Parent
    else{
        wait(NULL);
        printf("My child ID is..");
    }
    printf("common");
}
```

Output:

```bash
Befrore fork
I am a child
my parent is waiting
common
my child ID is...
common
```



#### waitpid()

> **Description:**
>
> system call suspends execution of the current process until a child specified by *pid* argument has changed state. By default, **waitpid**() waits only for terminated children
>
> **Parameters:**
>
> - **Less than -1** - meaning wait for any child process whose process group ID is equal to the absolute value of *pid*
> - **-1** - wait for any child processes
> - **0** - wait for any child process whose process group ID is equal to that of the calling process.
> - **Greater than 0** - wait for the child whose process ID is equal to the value of pid.
>
> **Return Values:**
>
> - on success, returns the process ID of the child whose state has changed 
>
> - **-1** - On error
>
> **Note:** Each of these calls sets *errno* to an appropriate value in the case of an error.

Usage same as `wait()` so no example.

#### kill()

> **Description:**
>
>  A command for you to kill a running process using a process ID.
>
> **Parameters:**
>
> - PID - PID you wnt to kill
> - Sig - signal you want to send
>
> **Return Values:**
>
> - **-1** - on error
> - **0** - on success
>
> **Optional sig kill options:**
>
> - SIGNULL - 0 - Check access to pid
> - SIGHUP - 1 - Terminate; can be trapped
> - SIGINT - 2 -  Terminate; can be trapped
> - SIGQUIT - 3 - Terminate with core dump; can be trapped
> - SIGKILL - 9 - Forced termination; cannot be trapped
> - SIGTERM - 15 - Terminate; can be trapped
> - SIGSTOP - 24 - Pause the process; cannot be trapped. (This is default if signal not provided)
> - SIGTSTP - 25 - Stop/pause the process; can be trapped
> - SIGCONT - 26 - Run a stopped process
>
> ***Note:***  the specific mapping between numbers and signals can vary between Unix implementations.

#### exec*() family

> **Description:**
>
> The exec family of functions replaces the current running process with a new process. It can be used to run a C program by using another C program. It comes under the header file **unistd.h.**
>
> - execl() 
> - execlp()
> - execle()
> - execv()
> - execvp()
> - execve()

##### execl() 

> e*xecl()* receives the location of the executable file as its first argument. The next arguments will be available to the file when it’s executed. The last argument has to be *NULL*:

Function Header:

```c
int execl(const char *pathname, const char *arg, ..., NULL)
```

Example Usage:

```c
#include <unistd.h>

int main(void) {
    char *file = "/usr/bin/echo";
    char *arg1 = "Hello world!";
	
    execl(file, file, arg1, NULL);

    return 0;
}
```

Output:

```bash
$ gcc execl.c -o execl
$ ./execl
Hello world!
```

##### execlp()

> *execlp()* is very similar to *execl()*. However, *execlp()* uses the PATH environment variable to look for the file. Therefore, the path to the executable file is not needed.

Function Header:

```c
int execlp(const char *file, const char *arg, ..., NULL)
```

Example Usage:

```c
#include <unistd.h>

int main(void) {
    char *file = "echo";
    char *arg1 = "Hello world!";

    execlp(file, file, arg1, NULL);
    return 0;
}
```

Output:

```bash
$ gcc execlp.c -o execlp
$ ./execlp
Hello world!
```

##### execle()

> If we use *execle(),* we can pass environment variables to the function, and it’ll use them:

Function Header:

```c
int execle(const char *pathname, const char *arg, ..., NULL, char *const envp[])
```

Example Usage:

```c
#include <unistd.h>

int main(void) {
    char *file = "/usr/bin/bash";
    char *arg1 = "-c";
    char *arg2 = "echo $ENV1 $ENV2!";
    char *const env[] = {"ENV1=Hello", "ENV2=World", NULL};
	
    execle(file, file, arg1, arg2, NULL, env);

    return 0;
}
```

Output:

```bash
$ gcc execle.c -o execle
$ ./execle
Hello World!
```

##### execv()

> *execv()*, unlike *execl()*, receives a vector of arguments that will be available to the executable file. In addition, the last element of the vector has to be NULL:

Function Header:

```c
int execv(const char *pathname, char *const argv[])
```

Example Usage:

```c
#include <unistd.h>

int main(void) {
    char *file = "/usr/bin/echo";
    char *const args[] = {"/usr/bin/echo", "Hello world!", NULL};
	
    execv(file, args);

    return 0;
}
```

Output:

```bash
$ gcc execv.c -o execv
$ ./execv
Hello world!
```

##### execvp()

> Just like *execlp()*, *execvp()* looks for the program in the PATH environment variable:

Function Header:

```c
int execvp(const char *file, char *const argv[])
```

Example Usage:

```c
#include <unistd.h>

int main(void) {
    char *file = "echo";
    char *const args[] = {"/usr/bin/echo", "Hello world!", NULL};
	
    execvp(file, args);

    return 0;
}
```

Output:

```bash
$ gcc execvp.c -o execvp
$ ./execvp
Hello world!
```

##### execve()

> We can pass environment variables to *execve()*. In addition, the arguments need to be inside a NULL-terminated vector:

Function Header:

```c
int execve(const char *pathname, char *const argv[], char *const envp[])
```

Example Usage:

```c
#include <unistd.h>

int main(void) {
    char *file = "/usr/bin/bash";
    char *const args[] = {"/usr/bin/bash", "-c", "echo Hello $ENV!", NULL};
    char *const env[] = {"ENV=World", NULL};
	
    execve(file, args, env);

    return 0;
}
```

Output:

```bash
$ gcc execve.c -o execve
$ ./execve
Hello World!
```



### Shared memory (IPC system calls)

> **Description:**
>
> through shared memory is a concept where two or more process can access the common memory. And communication is done via this shared memory where changes made by one process can be viewed by another process.
>
> The problem with pipes, fifo and message queue – is that for two process to exchange information. The information has to go through the kernel.
>
> - Server reads from the input file.
> - The server writes this data in a message using either a pipe, fifo or message queue.
> - The client reads the data from the IPC channel,again requiring the data to be copied from kernel’s IPC buffer to the client’s buffer.
> - Finally the data is copied from the client’s buffer.
>
> A total of four copies of data are required (2 read and 2 write). So, shared memory provides a way by letting two or more processes share a memory segment. With Shared Memory the data is only copied twice – from input file into shared memory and from shared memory to the output file.

#### shmget()

> **Description:**
>
> upon successful completion, shmget() returns an identifier for the shared memory segment.

#### shmat()

> **Description:**
>
> Before you can use a shared memory segment, you have to attach yourself to it using shmat().

#### shmdt()

> **Description:** 
>
> When you’re done with the shared memory segment, your program should detach itself from it using shmdt().

#### Example Using shmget(), shmat() and shmdt()

```C
//SHARED MEMORY FOR WRITER PROCESS
#include <iostream>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <stdio.h>
using namespace std;

int main()
{
	// ftok to generate unique key
	key_t key = ftok("shmfile",65);

	// shmget returns an identifier in shmid
	int shmid = shmget(key,1024,0666|IPC_CREAT);

	// shmat to attach to shared memory
	char *str = (char*) shmat(shmid,(void*)0,0);

	cout<<"Write Data : ";
	gets(str);

	printf("Data written in memory: %s\n",str);
	
	//detach from shared memory
	shmdt(str);

	return 0;
}

```

```c
// SHARED MEMORY FOR READER PROCESS
#include <iostream>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <stdio.h>
using namespace std;

int main()
{
	// ftok to generate unique key
	key_t key = ftok("shmfile",65);

	// shmget returns an identifier in shmid
	int shmid = shmget(key,1024,0666|IPC_CREAT);

	// shmat to attach to shared memory
	char *str = (char*) shmat(shmid,(void*)0,0);

	printf("Data read from memory: %s\n",str);
	
	//detach from shared memory
	shmdt(str);
	
	// destroy the shared memory
	shmctl(shmid,IPC_RMID,NULL);
	
	return 0;
}

```

Output Writer:

```bash
./writer
Write Data : Geeks for Geeks
Data written in memory: Geeks for Geeks
```

Output Reader:

```bash
./reader
Data read from memory: Geeks for Geeks
```



### Threads (IMPORTANT)

> **Description:**
>
> It allows us to create multiple threads for concurrent process flow. It is most effective on multiprocessor or multi-core systems where threads can be implemented on a kernel-level for achieving the speed of execution. Gains can also be found in uni-processor systems by exploiting the latency in IO or other system functions that may halt a process.

#### pthread_attr_init()

> function initializes the thread attributes object pointed to by attr with default attribute values.

Function Header:

```C
int pthread_attr_init(pthread_attr_t *attr);
```



#### pthread_create()

> **Description:**
>
> used to create a new thread.
>
> **Parameters:**
>
> - **thread:** pointer to an unsigned integer value that returns the thread id of the thread created.
> - **attr:** pointer to a structure that is used to define thread attributes like detached state, scheduling policy, stack address, etc. Set to NULL for default thread attributes.
> - **start_routine:** pointer to a subroutine that is executed by the thread. The return type and parameter type of the subroutine must be of type void *. The function has a single attribute but if multiple values need to be passed to the function, a struct must be used.
> - **arg:** pointer to void that contains the arguments to the function defined in the earlier argument.

Function Header:

```c
int pthread_create(pthread_t * thread, const pthread_attr_t * attr, void * (*start_routine)(void *), void *arg);
```

#### pthread_join ()

> **Description:**
>
> used to wait for the termination of a thread.
>
> **Parameters:**
>
> - **th:** thread id of the thread for which the current thread waits.
> - **thread_return:** pointer to the location where the exit status of the thread mentioned in th is stored.

Function Header:

```c
int pthread_join(pthread_t th, void **thread_return);
```

#### Example Using pthread_join() and pthread_create()

```c
// C program to show thread functions

#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>

void* func(void* arg)
{
	// detach the current thread
	// from the calling thread
	pthread_detach(pthread_self());

	printf("Inside the thread\n");

	// exit the current thread
	pthread_exit(NULL);
}

void fun()
{
	pthread_t ptid;

	// Creating a new thread
	pthread_create(&ptid, NULL, &func, NULL);
	printf("This line may be printed"
		" before thread terminates\n");

	// The following line terminates
	// the thread manually
	// pthread_cancel(ptid);

	// Compare the two threads created
	if(pthread_equal(ptid, pthread_self())
		printf("Threads are equal\n");
	else
		printf("Threads are not equal\n");

	// Waiting for the created thread to terminate
	pthread_join(ptid, NULL);

	printf("This line will be printed"
		" after thread ends\n");

	pthread_exit(NULL);
}

// Driver code
int main()
{
	fun();
	return 0;
}

```

Output:

```bash
This line may be printed before thread terminates
Threads are not equal
Inside the thread
This line will be printed after thread ends
```

### Semaphores, Conditional variables, mutex, monitors and Proc. sync. (IMPORTANT)

#### sem_init()

#### sem_wait()

#### sem_post()

#### pthread_mutex_lock()

#### pthread_mutex_unlock()

#### pthread_cond_wait()

#### pthread_cond_signal()



# Chapter 1 (Introduction)

## 1.1 - What Operating Systems Do

#### User View 

##### Mainframes and Minicomputers 

a user sits at a terminal connected to a mainframe or a minicomputer. Other users are accessing the same computer through other terminals. These users share resources and may exchange information. The operating system in such cases is designed to maximize resource utilization— to assure that all available CPU time, memory, and I/O are used efficiently and that no individual user takes more than her fair share.

##### Workstations and servers

users sit at workstations connected to networks of
other workstations and servers. These users have dedicated resources at their disposal, but they also share resources such as networking and servers, including file, compute, and print servers. Therefore, their operating system is designed to compromise between individual usability and resource utilization.

##### Headless systems

Some computers have little or no user view. For example, embedded
computers in home devices and automobiles may have numeric keypads and may turn indicator lights on or off to show status, but they and their operating systems are designed primarily to run without user intervention.

##### The system View

> **Operating System**
>
> the operating system is the program most intimately involved with the hardware. In this context, we can view an operating system as a resource allocator.

> **The Kernel** 
>
> the one program running at all times on the computer.

> **Middleware** 
>
> a set of software frameworks that provide additional services to application developers. For example, each of the two most prominent mobile operating systems—Apple’s iOS and Google’s Android—features a core kernel along with middleware that supports databases, multimedia, and graphics (to name a only few).

## 1.2 - Computer-System Organization

![image-20221018155522395](assets/image-20221018155522395.png)

#### Computer system operation 

![image-20221018155611140](assets/image-20221018155611140.png)

#### Storage Structure

> **Main Memory** 
>
> General-purpose computers run most of their programs from rewritable memory, called main memory (also called random-access memory, or RAM). Main memory commonly is implemented in a semiconductor technology called dynamic random-access memory (DRAM).

> **von Neumann architecture**
>
> first fetches an instruction from memory and stores that instruction in the instruction register. The instruction is then decoded and may cause operands to be fetched from memory and stored in some internal register. After the instruction on the operands has been executed, the result may be stored back in memory.

Ideally, we want the programs and data to reside in main memory
permanently but cant because:

- Main memory is usually too small to store all needed programs and data permanently.
- Main memory is a **volatile** storage device that loses its contents when power is turned off or otherwise lost.

![image-20221018160011925](assets/image-20221018160011925.png)

## 1.3 - Computer-System Architecture

![image-20221018160050679](assets/image-20221018160050679.png)

#### Single Processor systems

> **Microprocessors**
>
> PCs contain a microprocessor in the keyboard to convert the keystrokes into codes to be sent to the CPU. In other systems or circumstances, special-purpose processors are low-level components built into the hardware. The operating system cannot communicate with these processors; they do their jobs autonomously

#### Multiprocessor Systems

> **Multiprocessor systems**
>
> multiprocessor systems (also known as parallel systems or multicore systems) have begun to dominate the landscape of computing. Such systems have two or more processors in close communication, sharing the computer bus and sometimes the clock, memory, and peripheral devices.

##### 3 main advantages

- **<u>Increased throughput</u>** - By increasing the number of processors,we expect to get more work done in less time.
- **<u>Economy of scale</u>** - Multiprocessor systems can cost less than equivalent multiple single-processor systems
- **<u>Increased reliability</u>** - If functions can be distributed properly among several processors, then the failure of one processor will not halt the system, only slow it down. 

> **graceful degradation**
>
> The ability to continue providing service proportional to the level of surviving hardware.

> **Fault Tolerant** 
>
> These systems can suffer a failure of any single component and still continue operation.

##### 2 Types of Multiprocessing

- **<u>asymmetric multiprocessing</u> -** A boss processor controls the system; the other processors either look to the boss for instruction or have predefined tasks. This scheme defines a boss–worker relationship. The boss processor schedules and allocates work to the worker processors.
- **<u>symmetric multiprocessing</u> - ** each processor performs all tasks within the operating system. SMP means that all processors are peers; no boss–worker relationship exists between processors.

The difference between symmetric and asymmetric multiprocessing may
result from either hardware or software.

![image-20221018162258406](assets/image-20221018162258406.png)

##### Multicore Processors

A recent trend in CPU design is to include multiple computing cores on a single chip. Such multiprocessor systems are termed multicore.

They can be <u>more efficient</u> than multiple chips with single cores because on-chip communication is faster than between-chip communication.

![image-20221018162654731](assets/image-20221018162654731.png)

##### Blade Servers

Are a relatively recent development in which multiple processor boards, I/O boards, and networking boards are placed in the same chassis. The difference between these and traditional multiprocessor systems is that each blade-processor board boots independently and runs its own operating system.

##### Clustered Systems 

They are composed of two or more individual systems—or nodes—joined together. Systems are considered <u>loosely coupled.</u>

> **Hot Stand-by mode** 
>
> While the other is running the applications. The hot-standby host machine does nothing but monitor the active server.

> **parallelization**
>
> divides a program into separate components that run in parallel on individual computers in the cluster.

> **distributed lock manager (DLM)**
>
> Ensure that no conflicting operations occur.

> **storage-area networks (SANs)**
>
> which allow many systems to attach to a pool of storage. If the applications and their data are stored on the SAN,then the cluster software can assign the application to run on any host that is attached to the SAN.

![image-20221018163117177](assets/image-20221018163117177.png)

## 1.4 - Operating-System Structure

An operating system provides the environment within which programs are executed.

> **Multiprogramming**
>
> Increases CPU utilization by organizing jobs (code and data) so that the CPU always has one to execute.

> **Time-shared operating systems**
>
> A time-shared operating system uses CPU scheduling and multiprogramming to provide each user with a small portion of a time-shared computer.
>
> Amore common method for ensuring reasonable response time is virtual memory.

## 1.5 - Operating-System Operations

Modern operating systems are interrupt driven. If there are no processes to execute, no I/O devices to service, and no users to whom to respond, an operating system will sit quietly, waiting for something to happen.

#### Dual-Mode and Multimode Operation

In order to ensure the proper execution of the operating system, we must be able to distinguish between the execution of operating-system code and user defined code. 

![image-20221018163706027](assets/image-20221018163706027.png)

##### User mode vs Kernel Mode

> **Mode bit**
>
> Added to the hardware of the computer to indicate the current mode: kernel (0) or user (1). With the mode bit, we can distinguish between a task that is executed on behalf of the operating system and one that is executed on behalf of the user.

At system boot time, the hardware starts in kernel mode. The operating
system is then loaded and starts user applications in user mode. 

The dual mode of operation provides us with the means for protecting the
operating system from errant users.

> **Virtual machine manager (VMM)**
>
> In this mode, the VMM has more privileges than user processes but fewer than the kernel. It needs that level of privilege so it can create and manage virtual machines, changing the CPU state to do so.

#### Timer

We must ensure that the operating system maintains control over the CPU. We cannot allow a user program to get stuck in an infinite loop or to fail to call system services and never return control to the operating system. To accomplish this goal, we can use a timer.

A timer can be set to interrupt the computer after a specified period. The period may be fixed (for example, 1/60 second) or variable (for example, from 1 millisecond to 1 second). A variable timer is generally implemented by a fixed-rate clock and a counter.

## 1.6 - Process Management

We emphasize that a program by itself is not a process. A program is a passive entity, like the contents of a file stored on disk, whereas a process is an active entity. A single-threaded process has one program counter specifying the next instruction to execute. 

#### The operating system is responsible for the following activities in connection with process management:

- Scheduling processes and threads on the CPUs.
- Creating and deleting both user and system processes.
- Suspending and resuming processes.
- Providing mechanisms for process synchronization.
- Providing mechanisms for process communication.



## 1.7 - Memory Management

For a program to be executed, it must be mapped to absolute addresses and
loaded into memory. As the program executes, it accesses program instructions and data from memory by generating these absolute addresses. Eventually, the program terminates, its memory space is declared available, and the next program can be loaded and executed.

#### The operating system is responsible for the following activities in connection with memory management:

- Keeping track of which parts of memory are currently being used and who is using them.
- Deciding which processes (or parts of processes) and data to move into and out of memory.
- Allocating and deallocating memory space as needed.



## 1.8 - Storage Management

The operating system implements the abstract concept of a file by managing mass-storage media, such as tapes and disks, and the devices that control them. In addition, files are normally organized into directories to make them easier to use.

#### The operating system is responsible for the following activities in connection with file management:

- Creating and deleting files.
- Creating and deleting directories to organize files.
- Supporting primitives for manipulating files and directories.
- Mapping files onto secondary storage.
- Backing up files on stable (nonvolatile) storage media.

# Chapter 2 (Operating System Structures)

## 2.1 - Operating-System Services

![image-20221018165321410](assets/image-20221018165321410.png)

One set of operating system services provides functions that are helpful to
the user

#### User interface types

- **Command-line interface (CLI)**
- **Batch Interface** - commands and directives to control those commands are entered into files, and those files are executed.
- **graphical user interface (GUI)** - A window system with a pointing device to direct I/O, choose from menus, and make selections and a keyboard to enter text.

#### Program Execution

The system must be able to load a program into memory and to run that program. The program must be able to end its execution, either normally or abnormally (indicating error).

#### I/O operations

A running program may require I/O,which may involve a file or an I/O device.

#### File-system Manipulation

programs need to read and write files and directories. They also need to create and delete them by name, search for a given file, and list file information. Finally, some operating systems include permissions management to allow or deny access to files or directories based on file ownership. 

#### Communications

Communication may occur between processes that are executing on the same computer or between processes that are executing on different computer systems tied together by a computer network.

> **Shared Memory**
>
> In which two or more processes read and write to a shared section of memory.

> **Message Passing**
>
> In which packets of information in predefined formats are moved between processes by the operating system.

#### Error detection

Errors may occur in:

- CPU and memory hardware (such as a memory error or a power failure)
- I/O devices (such as a parity error on disk, a connection failure on a network, or lack of paper in the printer)
- The user program (such as an arithmetic overflow, an attempt to access an illegal memory location, or a too-great use of CPU time).

#### Resource allocation

When there are multiple users or multiple jobs running at the same time, resources must be allocated to each of them. The operating system manages many different types of resources.

#### Accounting

We want to keep track of which users use how much and what kinds of computer resources. This record keeping may be used for accounting (so that users can be billed) or simply for accumulating usage statistics. Usage statistics may be a valuable tool for researchers who wish to reconfigure the system to improve computing services.

#### Protection and Security 

When several separate processes execute concurrently, it should not be possible for one process to interfere with the others or with the operating system itself. Protection involves ensuring that all access to system resources is controlled. Security of the system from outsiders is also important. 

## 2.2 - User and Operating-System Interface

#### Command Interpreters

Some operating systems include the command interpreter in the kernel.

> **Shells**
>
> Systems with multiple command interpreters to choose from.
>
> ex. *Bourne shell, C shell, Bourne-Again shell, Korn shell*

Commands given at this level manipulate files: **create, delete, list, print, copy, execute, and so on**.

Uses the command to identify a file to be loaded into memory and executed. Thus, the UNIX command to delete a file

```shell
rm file.txt
```

searches for a file called rm,load the file into memory, and execute it with the parameter file.txt.

#### Graphical User Interfaces

Graphical user interfaces first appeared due in part to research taking place
in the early 1970s at Xerox PARC research facility. The first GUI appeared on the Xerox Alto computer in 1973.

## 2.3 - System Calls

> **System calls**
>
> provide an interface to the services made available by an operating system. These calls are generally available as routines written in C and C++, although certain low-level tasks (for example, tasks where hardware must be accessed directly) may have to be written using assembly-language instructions.

APIs are used to abstract away more detailed system calls.

![image-20221018171350179](assets/image-20221018171350179.png)

> **System call interface**
>
> The run-time support system (a set of functions built into libraries included with a compiler).

![image-20221018171518445](assets/image-20221018171518445.png)

#### Three general methods are used to pass parameters to the operating system.

1. Pass the parameters in registers.
2. parameters are generally stored in a block, or table, in memory, and the address of the block is passed as a parameter in a register. (Linux)
3. Parameters also can be placed, or pushed,onto the stack by the program and popped off the stack by the operating system.

> ***<u>Note:</u>*** Some operating systems prefer the block or stack method because those approaches do not limit the number or length of parameters being passed.

![image-20221018171849157](assets/image-20221018171849157.png)

## 2.4 - Types of System Calls

System calls can be grouped roughly into six major categories: **process control, file manipulation, device manipulation, information maintenance, communications,and protection.**

#### Process control

If control returns to the existing program when the new program terminates, we must save the memory image of the existing program; thus, we have effectively created a mechanism for one program to call another program.

We may want to wait for a certain amount of time to pass (wait time()). More probably, we will want to wait for a specific event to occur (wait event()). The jobs or processes should then signal when that event has occurred (signal event()).

To ensure the integrity of the data being shared, operating systems often provide system calls allowing a process to lock shared data.



| ![image-20221018173411277](assets/image-20221018173411277.png) | ![image-20221018173335846](assets/image-20221018173335846.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |

##### To create a new proccess

The system executes a fork() system call. Then, the selected program is loaded into memory via an exec() system call, and the program is executed. Either waits or runs in the background.

When the process is done, it executes an exit() system call to terminate, returning to the invoking process a status code of 0 or a nonzero error code.

![image-20221018172513167](assets/image-20221018172513167.png)

##### Responsibilities:

- end, abort
- load, execute
- create process, terminate process
- get process attributes, set process attributes
- wait for time
- wait event, signal event
- allocate and free memory

#### File manipulation

##### To create and delete files:

We first need to be able to create() and delete() files.

<u>**File Attributes may include:**</u>

- File name
- file type
- protection codes
- accounting information

If the system programs are callable by other programs, then each can be considered an API by other system programs.

![image-20221018172523515](assets/image-20221018172523515.png)

##### Responsibilities:

- create file, delete file
- open, close
- read, write, reposition
- get file attributes, set file attributes

#### Device manipulation

A process may need several resources to execute—main memory, disk drives, access to files, and so on. If the resources are available, they can be granted, and control can be returned to the user process. Otherwise, the process will have to wait until sufficient resources are available.

![image-20221018172536287](assets/image-20221018172536287.png)

##### Responsibilities:

- request device, release device
- read, write, reposition
- get device attributes, set device attributes
- logically attach or detach devices

#### Information maintenance

Many system calls exist simply for the purpose of transferring information between the user program and the operating system. For example, most systems have a system call to return the current time() and date().Other system calls may return information about the system, such as the number of current users, the version number of the operating system, the amount of free memory or disk space, and so on.

![image-20221018172547792](assets/image-20221018172547792.png)

##### Responsibilities:

- get time or date, set time or date
- get system data, set system data
- get process, file, or device attributes
- set process, file, or device attributes

#### Communications

There are two common models of inter process communication: the message passing model and the shared-memory model. In the message-passing model, the communicating processes exchange messages with one another to transfer information. Messages can be exchanged between the processes either directly or indirectly through a common mailbox.

![image-20221018172557102](assets/image-20221018172557102.png)

##### Responsibilities:

- create, delete communication connection
- send, receive messages
- transfer status information
- attach or detach remote devices

#### Protection

Protection provides a mechanism for controlling access to the resources provided by a computer system. Historically, protection was a concern only on multi programmed computer systems with several users. However, with the advent of networking and the Internet, all computer systems, from servers to mobile handheld devices, must be concerned with protection.

![image-20221018172607326](assets/image-20221018172607326.png)

##### Responsibilities:

- File and system security

## 2.5 - System Programs

> Where system programs fall in the hierarchy:
>
> Hardware -> Operating Systems -> System Programs (system utilities)

#### System Programs

Can be divided  into these categories:

- **File Management** - Create, delete, copy,rename,print, dump, list, and generally manipulate files and directories.
- **Status Information** - Ask the system for the date, time, amount of available memory or disk space, number of users, or similar status information.
- **File Modification** - Text editors may be available to create and modify the content of files stored on disk or other storage devices. There may also be special commands to search contents of files or perform transformations of the text.
- **Programming-language support** - Compilers, assemblers,debuggers,and interpreters for common programming languages
- **Program Loading and Execution** - Once a program is assembled or compiled, it must be loaded into memory to be executed.
- **Communications** - Provide the mechanism for creating virtual connections among processes, users, and computer systems.
- **Background services** - Systems have methods for launching certain system-program processes at boot time.

#### Application programs

Systems supplied with programs that are useful in solving common problems or performing common operations.

- Web browsers
- word processors and text formatters
- spreadsheets
- database systems
- compilers
- plotting and statistical-analysis packages
- games



## 2.6 - Operating-System Design and Implementation

#### Design Goals

Possible diffrent goals: 

- batch
- time sharing
- single user
- multiuser
- distributed
- real time
- general purpose

> **User Goals**
>
> Users want certain obvious properties in a system. The system should be convenient to use, easy to learn and to use, reliable, safe, and fast. 

> **System Goals**
>
> A similar set of requirements can be defined by those people who must
> design, create, maintain, and operate the system. The system should be easy to design, implement, and maintain; and it should be flexible, reliable, error free, and efficient. 

#### Mechanisms (How) and Policies (What)

The separation of policy and mechanism is important for flexibility. Policies are likely to change across places or over time. In the worst case, each change in policy would require a change in the underlying mechanism.

Policy decisions are important for all resource allocation.

#### Implementation

Early operating systemswere written in assembly language.Now, although
some operating systems are still written in assembly language, most are written in a higher-level language such as C or an even higher-level language such as C++.

The only possible disadvantages of implementing an operating system in a
higher-level language are reduced speed and increased storage requirements.



## 2.7 - Operating-System Structure

> A common approach is to partition the task into small components, or modules, rather than have one monolithic system.

#### Simple Structure

![image-20221018180005993](assets/image-20221018180005993.png)

![image-20221018180032471](assets/image-20221018180032471.png)

#### Layered Approach

With proper hardware support, operating systems can be broken into pieces that are smaller and more appropriate than those allowed by the original MS-DOS and UNIX systems.

![image-20221018180136650](assets/image-20221018180136650.png)

The major difficulty with the layered approach involves appropriately
defining the various layers.

A final problem with layered implementations is that they tend to be less
efficient than other types. For example:

- when a user program executes an I/O operation, it executes a system call that is trapped to the I/O layer, which calls the memory-management layer, which in turn calls the CPU-scheduling layer, which is then passed to the hardware.

#### Microkernels

The main function of the microkernel is to provide communication between
the client program and the various services that are also running in user space.

![image-20221018180409330](assets/image-20221018180409330.png)

> ***Note:*** The client program and service never interact directly.

##### Benefits of the microkernel approach

Makes extending the operating system easier. All new services are added to user space and consequently do not require modification of the kernel.

##### Drawbacks of the microkernel approach 

The performance of microkernels can suffer due to increased system function overhead.

#### Modules

![image-20221018180649206](assets/image-20221018180649206.png)

Organized around a central core with 7 modules:

1. Scheduling classes 
2. File systems
3. Loadable system calls
4. Executable formats
5. STREAMS modules
6. Miscellaneous
7. Device and bus drivers

#### Hybrid Systems

>  In practice, very few operating systems adopt a single, strictly defined structure. Instead, they combine different structures, resulting in hybrid systems that address performance, security, and usability issues.

- Mac OS X
- iOS
- Android OS

# Chapter 3 (Processes)

## 3.1 - Process Concept

**Different terminology for Processes:**

- Jobs (Batch systems)
- User-programs or Tasks (Time-shared)
- Processes (Single and multiprocessor)

#### The Process

> **Process**
>
> A program in execution. Note: Program file or program is not a process.

##### Active vs Passive Entity

- Program file or executable = passive entity
- Process = active entity

![image-20221018181926507](assets/image-20221018181926507.png)

#### Process State 

As a process executes, it changes state.

- **New** - Newly created
- **Running** - Instructions are being exicuted
- **Waiting** - The process is waiting for an event to occur
- **Ready** - The proccess is ready to be assigned to a procceor
- **Terminated** - the process has finished execution

#### Process Control Block

> **Process control block**
>
> Each process is represented in the operating system by a process control block (PCB)—also called a task control block. 



![image-20221018182438997](assets/image-20221018182438997.png)

![image-20221018182556561](assets/image-20221018182556561.png)

![image-20221018182621942](assets/image-20221018182621942.png)

## 3.2 - Process Scheduling

**Objectives for various systems:**

**<u>Multiprocessing</u>** - To have some process running at all times, to maximize CPU utilization.

**<u>Time-shared Systems</u>** - To switch the CPU among processes so frequently that users can interact with each program.

#### Scheduling Queues

This queue is generally stored as a linked list. Each PCB includes a pointer field that points to the next PCB in the ready queue.

> **device queue**
>
> The list of processes waiting for a particular I/O device
>
> ***Note:*** Each device has its own device queue

![image-20221018185359744](assets/image-20221018185359744.png)

A process continues this cycle until it terminates, at which time it is removed from all queues and has its PCB and resources deallocated.

#### Schedulers

> **scheduler**
>
> A action that chooses what program goes first in the queue.

##### Long vs Short 

- <u>**Short term CPU scheduler**</u> - selects from among the processes that are ready to execute and allocates the CPU to one of them. (miliseconds)
- **<u>Long term Job scheduler</u>** - elects processes from this pool and loads them into memory for execution. (minutes)

Main difference is the frequency of execution.

> **degree of multiprogramming**
>
> If the degree of multiprogramming is stable, then the average rate of process creation must be equal to the average departure rate of processes leaving the system. 

> **I/O-bound process**
>
> spends more of its time doing I/O than it spends doing computations.

> **CPU-bound process**
>
> I/O requests infrequently, using more of its time doing computations.

###### Medium-Term Scheduler

Buffer zone between memory and sort term scheduling.

![image-20221018190335199](assets/image-20221018190335199.png)

#### Context Switch

We perform a **state save** of the current state of the CPU, be it in kernel or user mode, and then a **state restore** to resume operations.



## 3.3 - Operations on Processes

#### Process Creation

- Parent processes 
  - Child processes

>  **PID (Process Identifier)**
>
> The pid provides a unique value for each process in the system, and it can be used as an index to access various attributes of a process within the kernel.

![image-20221018190847065](assets/image-20221018190847065.png)

##### When a process creates a new process, two possibilities for execution exist:

1. The parent continues to execute concurrently with its children.
2. The parent waits until some or all of its children have terminated.

##### There are also two address-space possibilities for the new process:

1. The child process is a duplicate of the parent process (it has the same program and data as the parent).
2. The child process has a new program loaded into it.

![image-20221018191051598](assets/image-20221018191051598.png)

#### Process Termination

The process stops with the exit() command.

##### A parent may terminate the execution of one of its children for a variety of reasons, such as these:

- The child has exceeded its usage of some of the resources that it has been allocated. (To determine whether this has occurred, the parent must have amechanism toinspect the state ofits children.)
- The task assigned to the child is no longer required.
- The parent is exiting, and the operating system does not allow a child to continue if its parent terminates.

> **cascading termination**
>
> Some systems do not allow a child to exist if its parent has terminated. In such systems, if a process terminates (either normally or abnormally), then all its children must also be terminated.

## 3.4 - Interprocess Communication

**Benefits:**

- Information sharing
- Computation speedup
- Modularity
- Convenience

![image-20221018191625090](assets/image-20221018191625090.png)

## 3.5 - Examples of IPC Systems

### IPC (Inter-process Communication) with message passing

> **IPC** **(Inter-process Communication)**
>
> Cooperating processes require an inter-process communication (IPC) mechanism that will allow them to exchange data and information.

> **Message Passing**
>
> message-passing model, communication takes place by means of messages exchanged between the cooperating processes.
>
> Message passing provides a mechanism to allow processes to communicate and to synchronize their actions without sharing the same address space.

| Advantages                                    | Disadvantages                                            |
| --------------------------------------------- | -------------------------------------------------------- |
| Useful for sending small amounts of data/info | Slow - Uses system calls instead of shared memory space. |
| Easier to implement                           |                                                          |
| No need to share address space                |                                                          |

![image-20221022173321505](assets/image-20221022173321505.png)

- Messages sent by a process can be either fixed or variable in size.
  - If only fixed-sized messages can be sent, the system-level implementation is straightforward. 
    - makes the task of programming more difficult

**Methods for logically implementing a link and the send()/receive() operations:**

- Direct or indirect communication.
- Synchronous or asynchronous communication.
- Automatic or explicit buffering.

#### Naming

Processes that want to communicate must have a way to refer to each other. They can use either direct or indirect communication.

##### Direct Communication

Explicitly name the recipient or sender of the communication needed.

```c
send(P, message); // Send message to process P
recieve(Q, message); // Recieve messages from process Q
```

**Communication link Properties:**

- A link is established automatically between every pair of processes

- A link is associated with exactly two processes.
- Between each pair of processes, there exists exactly one link.

**Asymmetric vs Symmetric** 

```c
//Symmetric
send(P, message); // Send message to process P.
recieve(Q, message); // Recieve messages from process Q.
```

```c
// Asymmetric
send(P, message); // Send message to process P.
recieve(message); // Recieve messages without explicit naming of sender.
```

The disadvantages of symmetric and asymmetric is a <u>lack of modularity</u> .

##### Indirect Communication

messages are sent to and received from mailboxes, or ports.

```c
send(A, message); // Send message to mailbox A
recieve(A, message); // Recieve messages from mailbox A
```

**Communication link Properties:**

- A link is established between a pair of processes only if both members of the pair have a shared mailbox.
- A link may be associated with more than two processes.
- Between each pair of communicating processes, a number of different links may exist, with each link corresponding to one mailbox.

***Ex. Who gets the message?***

```c
P1 => send(A, message); // Send message to mailbox A
P2 => recieve(A, message); // Recieve messages from mailbox A
P3 => recieve(A, message); // Recieve messages from mailbox A
```

**The answer depends on which of the following methods we choose:**

- Allow a link to be associated with two processes at most.
- Allow at most one process at a time to execute a receive() operation.
- Allow the system to select arbitrarily which process will receive the message (that is, either P2 or P3,but not both, will receive the message).The system may define an algorithm for selecting which process will receive the message (for example, *round robin*, where processes take turns receiving messages). The system may identify the receiver to the sender.

**If the mailbox is owned by:**

- **Operating system** - Must provide a mechanism to do the following
  - Create a new mailbox.
  - Send and Receive through the mailbox.
  - Delete the mailbox.
- **Process** - We distinguish between the owner (which can only receive messages through this mailbox) and the user (which can only send messages to the mailbox).

#### Synchronization of Process Communication

Communication takes place between two system calls:

- Send()
- Receive()

**Blocking (synchronous) vs Non-blocking (Asynchronous)**

- **Blocking**
  - **Send** - The sending process is blocked until the message is received by the receiving process or by the mailbox.
  - **Receive** - The receiver blocks until a message is available.
- **Non-Blocking**
  - **Send** - The sending process sends the message and resumes operation.
  - **Receive** - The receiver retrieves either a valid message or a null.

#### Buffering

Whether communication is direct or indirect, messages exchanged by communicating processes reside in a temporary queue.

**queues can be implemented in three ways:**

- **Zero capacity** **(NON BUFFERING)** - The queue has a maximum length of zero; thus,the link cannot have any messages waiting in it. In this case, the sender must block until the recipient receives the message.
- **Bounded capacity** **(BUFFERING)** - The queue has finite length n; thus, at most n messages can reside in it.
- **Unbounded capacity** **(BUFFERING)** - The queue’s length is potentially infinite; thus, any number of messages can wait in it. The sender never blocks.

### IPC (Inter-process Communication) with shared memory

> **Shared Memory**
>
> A region of memory that is shared by cooperating processes is established. 

- The operating system tries to prevent one process from accessing another process’s memory. Shared memory requires that two or more processes agree to <u>remove this restriction</u>.
- The Process is also responsible for ensuring that they are not writing to the same location simultaneously.

#### Producer - Consumer Problem 

A <u>producer</u> process produces information that is consumed by a <u>consumer</u> process.

The producer and consumer must be synchronized, so that the consumer does not try to consume an item that has not yet been produced.

##### Bounded vs Unbounded Buffers

- **Unbounded -** Places no practical limit on the size of the buffer.
- **Bounded -** Assumes a fixed buffer size. 

# Chapter 4 (Threads)

## 4.1 - Overview

> **Thread**
>
> A thread is a basic unit of CPU utilization and comprises of:
>
> - Thread ID
> - Program Counter
> - A register Set
> - A stack data structure

#### Motivation

![image-20221022184149838](assets/image-20221022184149838.png)

With threading a single application or program can:

- Display images.
- display text.
- Retrieve data from the network.
- And more.

Like IPC threads help RPC servers handle concurrent connections.

![image-20221022184424212](assets/image-20221022184424212.png)

#### Benefits of threading

- **Responsiveness** - keep program running even if blocked.
- **Resource sharing** - threads share the memory and the resources of the process to which they belong by default.
- **Economy** - because threads share the resources of the process to which they belong, it is more economical to create and context-switch threads.
- **scalability** - threads may be running in parallel on different processing cores.

## 4.2 - Multicore Programming

![image-20221022184810891](assets/image-20221022184810891.png)

#### Challenges in multicore Programming

five areas present challenges in programming for multicore
systems:

- **Identifying tasks -**  examining applications to find areas that can be divided into separate
- **Balance -** ensure that the tasks perform equal work of equal value.
- **Data Splitting -** data accessed and manipulated by the tasks must be divided to run on separate cores.
- **Data dependency -** the data accessed by the tasks must be examined for dependencies between two or more tasks.
- **Testing a debugging -** when a program is running in parallel on multiple cores, many different execution paths are possible.

#### Types of Parallelism (Data vs Task)

> **Data Parallelism**
>
> Focuses on distributing subsets of the same data across multiple computing cores and performing the same operation on each core.

> **Task Parallelism**
>
> Involves distributing not data but tasks (threads) across
> multiple computing cores. Each thread is performing a unique operation.

## 4.3 - Multithreading Models

> **User Threads**
>
> User threads are supported above the kernel and are managed without kernel support.

> **Kernel Threads**
>
> kernel threads are supported and managed directly by the operating system.
>
> ***Note:*** Windows, Linux, Mac OS X,and Solaris - support kernel threads.

A relationship must exist between user threads and kernel threads. 3 common ways:

- many-to-one model
- one-to-one model
- many-to-many model

#### Many-to-One Model 

![image-20221022190333089](assets/image-20221022190333089.png)

> Maps many user-level threads to one kernel thread.

| Advantages                                                   | Disadvantages                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Thread management is done by the thread library in user space, so it is efficient | The entire process will block if a thread makes a blocking system call. |
|                                                              | because only one thread can access the kernel at a time,multiple threads are unable to run in parallel on multicore systems. |



#### One-to-One Model

![image-20221022190345411](assets/image-20221022190345411.png)

>   Maps each user thread to a kernel thread.

| Advantages                                                   | Disadvantages                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| It provides more concurrency than the many-to-one model by allowing another thread to run when a thread makes a blocking system call. | creating a user thread requires creating the corresponding kernel thread. (High overhead) |
| Allows multiple threads to run in parallel on multiprocessors. |                                                              |



#### Many-to-Many Model

![image-20221022191444594](assets/image-20221022191444594.png)

> multiplexes many user-level threads to a smaller or equal number of kernel threads.

| Advantages                                                   | Disadvantages |
| ------------------------------------------------------------ | ------------- |
| Developers can create as many user threads as necessary.     | None          |
| Corresponding kernel threads can run in parallel on a multiprocessor. | None          |
| when a thread performs a blocking system call, the kernel can schedule another thread for execution. | None          |

## 4.4 - Thread Libraries 

A thread library provides the programmer with an API for creating and managing threads. There are two primary ways of implementing a thread library:

- to provide a library entirely in user space with no kernel support. All code and data structures for the library exist in user space. This means that invoking a function in the library results in a local function call in user space and not a system call.
- to implement a kernel-level library supported directly by the operating system. In this case, code and data structures for the library exist in kernel space. Invoking a function in the API for the library typically results in a system call to the kernel.

#### Pthreads Library

This is a specification for thread behavior, not an implementation.

```c
// Pthread code for joining 10 threads.
#define NUM THREADS 10

/* an array of threads to be joined upon */ 
pthread_t workers[NUM THREADS];

for (int i = 0; i < NUM THREADS; i++){
	pthread join(workers[i], NULL);   
} 
```



## 4.6 - Threading Issues

#### The fork() and exec() System Calls

if a thread invokes the exec() system call, the program specified in the parameter to exec() will replace the entire process—including all threads.

#### Signal Handling

##### **Pattern of signaling**

1. A signal is generated by the occurrence of a particular event.
2. The signal is delivered to a process.
3. Once delivered, the signal must be handled.

##### **A signal may be *handled* by one of two possible handlers**

1. A default signal handler
2. A user-defined signal handler

Every signal has a default signal handler that the kernel runs when handling that signal. This default action can be overridden by a user-defined signal handler that is called to handle the signal.

##### **In multithreaded cases a signal can be sent via these options**

1. Deliver the signal to the thread to which the signal applies.
2. Deliver the signal to every thread in the process.
3. Deliver the signal to certain threads in the process.
4. Assign a specific thread to receive all signals for the process.

##### Async vs sync signals

- **Async** - Ctrl+C type interupts
- **Sync** - system calls like:
  - `kill(pid_t, int signal)`
  - `pthread_kill(pthread_t tid, int signal)`

#### Thread Cancellation

Cancellation of a target thread may occur in two different scenarios:

- **Asynchronous cancellation** - One thread immediately terminates the target thread.
- **Deferred cancellation** - The target thread periodically checks whether it should terminate, allowing it an opportunity to terminate itself in an orderly fashion.

#### Thread-Local Storage (TLS)

- Local variables are visible only during a single function invocation, whereas TLS data are visible across function invocations.
- TLS data are unique to each thread.

#### Scheduler Activations

A final issue to be considered with multithreaded programs concerns communication between the kernel and the thread library.

> **Light weight Process (LWP)**
>
> implementing either the many-to-many or the two-level
> model place an intermediate data structure between the user and kernel threads.

- Each LWP is attached to a kernel thread

![image-20221022193300525](assets/image-20221022193300525.png)

# Chapter 5 (Process Synchronization)

## 5.1 Background

## 5.2 The Critical-Section Problem

> **Critical Section Problem**
>
> used to design a protocol followed by a group of processes, so that when one process has entered its critical section, no other process is allowed to execute in its critical section.

> **critical section**
>
> The segment of code where processes access shared resources, such as common variables and files, and perform write operations on them.

```c
While(True){
    // Aquire lock
    
    // Critical section
    
    // Disable lock
    
    // Exit()
}
```

#### Solutions to the critical section problem

##### Methods

- **Mutual Exclusion** - When one process is executing in its critical section, no other process is allowed to execute in its critical section.
- **Progress** - When no process is executing in its critical section, and there exists a process that wishes to enter its critical section, it should not have to wait indefinitely to enter it.
- **Bounded waiting** - There must be a <u>bound on the number of times a process is allowed to execute</u> in its critical section, after another process has requested to enter its critical section and before that request is accepted.

##### Implementations (Locks)

- **test_and_set** - Uses boolean True/False.
- **Mutex Locks** - Software method that provides the `acquire()` and `release()` functions that execute atomically.
- **Semaphores** - More sophisticated methods that utilize the `wait()` and `signal()` operations that execute atomically on Semaphore `S`, which is an integer variable.
- **Conditional Variables** -  Utilizes a queue of processes that are waiting to enter the critical section.

## 5.3 Peterson’s Solution

## 5.5 Mutex Locks

> **Mutex (Mutual Exclusion)**
>
> to protect critical regions and thus prevent race conditions. Also called a spin lock.

- A process that attempts to acquire an unavailable lock is blocked until the lock is released.

```C
//aquire a lock
Aquire(){
    while(!available){
        //Wait
        available = false;
    }
}

// release a lock
release(){
    available = true;
}
```

| Advantages                                                   | Disadvantages                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Easy to implement                                            | Requires busy waiting - other processes loop waiting for the mutex to unlock |
| no context switch is required when a process must wait on a lock |                                                              |

#### Conditional variables

##### Why conditional variables instead of mutex?

When we need to conditionally check if some condition is met, we do not want to use spin look because it wastes CPU time. A condition variable is a way to achieve the same goal without continually polling.

- Always use in conjunction to a mutex lock

## 5.6 Semaphores

> **Semaphores**
>
> is an integer variable that, apart from initialization, is accessed only through two standard atomic operations:
>
> - wait()
> - Signal()

```c
//Definition of wait()
wait(S){
    while(S <= 0){
        //busy wait
        S--;
    }
}
```

```c
//Definition of Signal()
signal(S){
    S++;
}
```

- All modifications to the integer value of the semaphore in the wait() and signal() operations must be executed indivisibly. That is, when one process modifies the semaphore value, no other process can simultaneously modify that same semaphore value. 
- Signal and wait must be executed without interruption.

> **Counting semaphores**
>
> The value of a counting semaphore can range over an unrestricted domain.

> **Binary semaphores**
>
> The value of a binary semaphore can range only between 0 and 1.

## 5.7 Classic Problems of Synchronization
