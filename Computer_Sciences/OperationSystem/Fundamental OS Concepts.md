
#### Thread 
- Fully describes the program state 
- Program Counter, registers, execution flags, stack 
- Thread is executing on a processor core when it is resident in the processor registers. 

Resident and thread working 

How the multiple processors working? 

#### Address Space 

heap and stack 

Simple multiprogramming - risky, for previous generation of the OS has no protection. 

What OS should do in security wise? 

what is protection? 
- Base and bound 

x86 Segments and Stacks 


Address Space Translation 

Paged Virtual Address Space - all pages has same size, easy to place, 
#### Process 
- Execution environment with restricted rights. protected address space with one or more threads. 
- encapsulate one or more threads sharing process resources. 

#### How does the process change the states? 
Processes protect each other; the OS protects the processes. 

Trade off the protection of the process 



### Process Creation 
The parent -> issues the pid (int) -> `pid = fork()` 

-> if `pid = 0 (child) ` -> exec() - > exit () 
	-> if `pid > 0 (parent)` -> wait(), until its child exits -> parent resumes 

- `CreateProcess()` takes two parameters: `STARTUPINFO` specifies properties of the new process, such as window size and appearance and handles standard input and output files. 
  `PROCESS_INFORMATION` takes a structure that contains a handle and the identifiers to the newly created process and its thread. 
- Those `STARTUPINFO` and `PROCESS_INFORMATION` structures are made when the programme starts. 

### Process Termination 
- The process terminates when it executes its final statement or when it asks the OS to delete itself with the `exit()` system call. 
- Memory and all resources for the process will be deallocated once process termination has been called. 
- May be called by another programme or process via a system call (e.g. `TerminateProcess()`)
- Parents may terminate the child's process for the following reasons: 
	- The child has exceeded the usage of the resource which was allocated. 
	- The tasks assigned are no longer needed. 
	- When its parent is exiting. 

`Cascading termination` - Do not allow to terminate the child to be terminated while all of the children have been terminated. 

- Zombie process - The parent process did not `wait()` while the child processes are running. 
- Orphans - The children process lost a parent but are still remaining in the process list; UNIX systems find the new parents for those orphans, LINUX - `init()`, `systemd`

### Interprocess Communications 
https://learn.microsoft.com/en-us/windows/win32/ipc/interprocess-communications
Interprocess Communications a.k.a IPC, is the mechanism to share information between processes or applications. Like an intercommunication system between processes. 

Client - the process or application takes the information
Server - the process or application gives the information. 

Fundamental models of interprocess communication - shared memory and message passing. 

Reason for the interprocess communications: 
- Information Sharing 
- Computation Speedup
- Modularity

#### Shared Memory 
Resided in the shared memory pool and communicated through the shared memory. 
Each memory to communicate each other must be attached to between of the shared memory.

#### Message Passing 
Message passing, on the other hand, it provided the mechanism 


##### Case Study - Windows 
Using advance procedual call (ALPC) for communicating the two processes, which maintains two types of the ports to communicate; connection ports and communication ports. 

Once the ALPC is estabilished, one of third information passing techniques chosen; 
1. Using port's message queue if the message is small enough (256 bytes)
2. If the message is large enough must be passed thorugh a section object; the region of the shared memory. 
3. If the data is too large to store it in section object, then directly read and write into the client address space of the client. 

### Communication Client-Server
has two strategies on communication between client and server - socket and RPC (remote procedure calls).

##### Sockets 
- A socket means the endpoint of the communication. 

##### RPC (Remote Procedure Calls)


#### Dual Mode operation/protection


Kernal mode vs User mode. 

3 types of User -> kernal mode transfer 
- Syscall 
- Interrupt
- Trap and Exception. 

what if the programme should do only can do in the kernel mode 
- Can't, only do in the api in the kernel 
- it could, download the interface to run in the kernel but not compromised the security. 

Interrupt Vector. 

How to swapping between the processes? 
Simple B&B - interrupt  -> Switch user process -> resume 

What if we running many programmes? 

Process control block (PCB)

Scheduler. 



![[Pasted image 20251002190653.png]]
Source : CS162 Lecture 2 : Four fundimental OS Concepts - John Kubiatowicz 