#### Recalled the 4 Fundamental OS concepts
- Thread : Execution context 
	- Fully describes program state 
	- program counter, registers, execution flags, and stack 
- Address Space (with or w/o translation)
	- Set of  memory addresses accessible to program (for read or write)
	- Distinct from memory space of the physical machine, in which case the program operate in virtual address space OS given 
- Process : an instance of the running programme 
	- Protected address space + one or more threads

- Dual mode operation/Protection 
	- Only system has the ability to access certain resources. 
	- combined with translation, isolates programs from each other and the OS from programmes. 

Threads are not physical core - the virtual cores when it is resident in the processor's registers. 

Virtual Address Space - is the processor's view of memory; separate from the physical storage. Virtual addresses are allocated in the physical address space through the translation map. 

#### Dual mode operation 
- User mode vs Kernel mode 
	- Only kernel mode can handle the privileged actions. 
	- calls system call when user tried to access on the kernel mode. 

### Thread 
- Thread is the single unique execution context. 
- mode bit? -> integer value to represent the each dual mods. 

##### What brings the motivation for the thread
- **Operation system must handle multiple things at once.** 
	- like processes, interrupts, background system maintenance. 
- Servers and Parallel programs and interface must handle multiple things at once. 
- Threads allows handling multiple things at once; Each thread can take the one thing or one task.
- Handling I/O take subsequent time to process; you may not want to interrupted other tasks by I/O process

### Multiprocessing vs Multiprogramming 
- Multiprocessing - has multi cores or CPUS 
- Multiprogramming - Do multiple jobs and processes 
- Multithreading -do multiple threads and processes. 

##### Concurrency vs Paralleism 
![[Screenshot 2025-12-02 033555.png]]
*Source -  CS162 Lecture 3: Abstractions 1: Threads and Processes, John Kubiatowicz*

*Once the moment the scheduler handles two more thread, you have to start thinking about correctness; have to design the locking schemes or other scheduling plan.*

**Multiprogramming is for thinking how the programmer thinks the correctness with right methods.** 

#####  ***Concurrency IS NOT parallelism!*** 
- If runs the two threads on a single core system; you execute concurrently, but not in parallel - cannot run it in multiprocessing! 
- Multithreading != Multiprocessing 
- Each thread can handles or manages a separate things or tasks, but it does not guaranteed to do the task simultaneously! 

### How to do multithreading programming 

```c
main () { 
	computePI("Pi.txt"); // it would goes forever 
	PrintPoo("poo.txt"); // never runs the print poo. 
}
```

if the compute pi (of course) goes forever, print poo won't be pop in the screen. Even you have multiple cores to handle the multiple tasks, you won't expect to show the poo unless you put those two task in the different thread. 

```c
main ()
{ 	
	create_thread(computePI, "Pi.txt");
	create_thread(PrintPoo, "poo.txt"); 
}

```

`create_thread()` spawns the new thread to run the given procedure; should behave as if another CPU is running the given procedure. 

Then scheduler schedules the works like this and you can see the print poo. 
![[Pasted image 20251202035205.png]]

 Question : How can we make a multithreaded process? 
	- Once the process starts, it issues system calls to create new threads. These new threads are part of the process; they share its address space. 

### System calls? 
- System calls : the programmatic way in which a computer program requests a service from the kernel of the operating system on which it is executed.
	- System calls interface works in the software level to access portable OS kernel mode. 
	- OS library issues system call
	- language runtime use OS library and issues system call
	- System calls happens all time in under the hood, you cannot see it.
![[Pasted image 20251202114928.png]]
- System calls interface is not generalised but unique by the OS, but partially it shares. 

- `libc.h` - OS library to issue the syscalls, it inherited each computer programme to access on the OS 
- `pthreads` - OS Library APOI for threads 
```c 
int pthread_create (pthread_t* thread, const pthread_attr_t *attr, void*(*start_routine) (void*) , void *arg)

void*(*start_routine) (void*) // void function pointer of start_routine poiunter takes void pointer 
```
- thread is created executing `start_routine` with `arg`  as its sole argument. 
- return is implicit call to `pthread_exit`.

```c 
void pthread_exit(void *value_ptr); 
```
`pthread_exit`  terminates the thread and makes value_ptr available to any successful join. 

```c
int pthread_join(pthread_t thread, void **value_ptr); 
```

###### What happened when `pthread_create()` is called in the process? 
- asm code issues syscall -> access in the kernel mode -> return value from reges. 
- pthread_process is the wrapper of the system call. 
- issuing syscall is not cheap; transition user mode -> kernel mode is beyond than simple switching. 

### Folk-Join Pattern 
![[Pasted image 20251202131621.png]]
- Main thread creates the collection of the sub-treads and passing them args to work on, then joins with them when the main thread works done and collecting the results. 

### Thread State 
- State shared by all thread in process and address space. 
- State "private" to each thread
	- Kept in TCB = thread control block
	- CPU registers (including, program counter)
	- Execution stack 


### Race Condition
Each treads do the same works eventually it races against to other thread. 

##### Example - work in the R/B tree with the multiple thread. 

### Lock 
`lock.acquire()` - wait until lock is free
`lock.release() `

OS Library Locks 
`int pthread_mutex_init()`
`int pthread_mutex_lock(pthread_mutex_t *mutex)`


### Life of the Process 
![[Pasted image 20251202141635.png]]