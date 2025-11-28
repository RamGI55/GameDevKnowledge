
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

Execution enviornment with restricted rights. protected address space with one or more threads. 
encapsulate one or more threads sharing process resources. 

Process provide the protection from each other, OS protected from the process. 

Trade off the protection of process 

Threads encapsulate concurrency; and active component. 
Address spaces encapsulate protection; the passive component. 
for parallelism, concurrency. 


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