### 3 Types of Memory 

|                   | Scope                                                 | Lifetime                         | Location |
| ----------------- | ----------------------------------------------------- | -------------------------------- | -------- |
| Global            | The entire time                                       | The life time of the application | Stack    |
| Static            | The function it is declared within                    | The lifetime of the application  | Stack    |
| Automatic (Local) | The function it is declared within                    | While the function is executing  | Stack    |
| Dynamic           | Determined by the pointers that reference this memory | Until the memory is freed        | Heap     |

### Uses of the Pointers 
- Providing a convenient means for addressing many types of problems 
- Supporting the dynamic memory allocation 
- Providing the ability to pass data structures by pointer without incurring a large overhead (e.g. heap, tree based and linked list )

**Linked List** is the most common example to using pointers; easier to use and map directly to the next or previous link than array, which is the fixed size and continueos data structure to contain the serial of the data. 

But, it may has risks of 
	• Accessing arrays and other data structures beyond their bounds
	• Referencing automatic variables after they have gone out of existence
	• Referencing heap allocated memory after it has been released
	• Dereferencing a pointer before memory has been allocated to it 


### Using Pointers 
##### Declaring Pointers 
```c
int num; // uninitalised num; 
int* pi; // declared pointer
```

• The content of `pi` should eventually be assigned the **address of an integer variable.**
• These variables have not been initialized and thus contain garbage.
• There is nothing inherent to a pointer’s implementation that suggests what type of
data it is referencing or whether its contents are valid.
• However, the pointer type has been specified and the compiler will frequently com‐
plain when the pointer is not used correctly.

###### Reading Declaration 

```c 
const int *pci; 
```

Read the declaration as backward! 
`pci` is variable -> Pointer variable -> it is intger pointer variable -> it is const integer pointer variable. 


