#### Interface vs Data Structure

Interface (API/ADT) offers 
- the specification 
- Specify **what** data can be stored. 
- What operations are supported and what they mean 
- What are the problems?

Data Structure offers 
- Representation 
- **How to** store the data 
- Algorithms to support operations.
- How to solve the problem (Solutions)

#### Case Study - Interface and Data Structure 

Sequence Interface - Problem 
- Must maintain the sequence of the items, the order must be extrinsic. 
- Based on 0-Index. 
- Supports the sequence operations below: 
	- build (x) - build the sequence operation 
	- Static features - Getter and Setter to find or replace the feature in the index. 
	- Dynamic features - add and remove the items. 

What is the proper data structure? - Solution 
**Static Array** - using the RAM model of the computation as the key. 

|                         | Sequence Interface | Data Structure                             |
| ----------------------- | ------------------ | ------------------------------------------ |
| Build(x)                |                    | O(n) per build                             |
| Getter/Setter           |                    | O(1) per each getter and setter operation. |
| Memory Allocation Model |                    | Allocate Array of the size in O(n) time.   |
|                         |                    |                                            |

#### Static Array 
- Key: word RAM model of the computation 
	- Memory : array of the w-bit words.
	- Consecutive chunk of the memory - store the information in constructive way. 
	- if you'd like to access `array[i]` => `memory[address(array)+i]`
	- array access time `O(1)`, const time. 

#### Dynamic Container - Pointer Based

##### Linked Lists 
Store the items in bunch of the nodes and each node has item and the next pointer.
Keep tracking the `head` and `length` of the list. 
Contains the data in arbitrary orders; each data posed some where in the memory (not consecutive) 
Pointer just induces the item follows next to the node. 

##### Case Study - Implants the Dynamic Sequence

|                     | Static Array                                                                       | Linked List                                                                                                                  |
| ------------------- | ---------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Insert, Delete at i | Shifting -> allocation/Copying each time you insert/delete the items.<br>O(n) time | At front : O(1) time <br>At Middle: O(i) time - Worst Case<br>At End: O(1) on the doubly linked list, points the last index. |
| Size Change         | if n = size, allocate new array of pow(2) size. <br>- O(sum of 2^i) = O(2^k+1 - 1) |                                                                                                                              |
|                     |                                                                                    |                                                                                                                              |

##### Dynamic Arrays in C++, `std::vector`



Amortization : operation takes T(n) amortized time if any K operations take =< K * T(n) time. 
Averaging over operation sequence. 


#### Sequence Interface Operation 

Operation Time in Worst case

|                   |    Static    |        Dynamic        |                    |                      |
| ----------------- | :----------: | :-------------------: | :----------------: | :------------------: |
|                   | get(), set() | Insert/delete first() | insert/delete Last | insert delete at (i) |
| **Static Array**  |    **1**     |           n           |         n          |          n           |
| **Linked List**   |      n       |         **1**         |         n          |          n           |
| **Dynamic Array** |      1       |           n           |      **1(a)**      |          n           |
