#### How to use heap memory

```c 
void* malloc(size_t); 
```


```c 
int* pfoo = (int*)malloc(5* sizeof(int));
```
This allocates memory for `pfoo` integer, has 5 size of the int (4 x 5 = 20 for 32bits, 8 x 5 = 40 for 64-bit system). 

`malloc()` returns the `void*`,  which means you can explicity or implicitly designated the type of the value in C. 

```c 
int* pfoo = malloc(5* sizeof(int)); // this designated the type value as implicity as intger. 
```

**Careful!** 
C++ **DO NOT** allow to implicit change of the pointer type; must be casted when you use `malloc()`. 

```cpp 
int* pfoo = static_cast<int*>malloc(5* sizeof(int));
int* pfoo2 = (int*)malloc(5* sizeof (int)); // must explicity cast the type of the pointer in rvalue.

int* pfoo3 = malloc(5*sizeof(int)); // Error : you cannot implicity cast the void pointer 

void* pfoo4 = malloc(5*sizeof(int)); // this works, matches as void pointers. 

int* pfoo5 = new int ppfoo; // this might works.
```

Once you finished to use the pointer and have to deallocate the pointer, then free the memory with `free()` function, or designate as `NULL`, `nullptr`, or `0`. 


```c 
free (pfoo); // this frees the memory which is allocated 
pfoo = 0; // 0 also works for deallocate the memory. 
```

in C++ 

```c++ 
delete pfoo; // use delete

pfoo = nullptr; this also works. 
```

#### Memory can be free upon program termination, but.. 
The Operating System takes part in freeing the memory occupied by the program termination; However, the programmer still has the responsibility to free the memory for the program for several reasons. 

• May be more trouble than it’s worth
• Can be time-consuming and complicated for the deallocation of complex structures
• Can add to the application’s size
• Results in longer running time
• Introduces the opportunity for more programming errors

#### Dynamic Allocation Functions 

| FUNCTION        | Description                                                                                                                                                            |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| malloc (size_t) | Allocates the memory on the heap as the `size_t`.                                                                                                                      |
| realloc()       | Reallocates the memory to the larger or smaller size of the memory based on the size the previously allocated.                                                         |
| calloc()        | Allocates the zeros out memory from heap.                                                                                                                              |
| free()          | Returns a block of the memory in the heap<br>*it returns to the OS not the memory<br>Use the `NULL`, or `0` if'd like to returns the designated memory on the memory.* |

### `Realloc()` 

```c 
void* realloc(void *ptr, size_t size); 
```

Reallocate the memory block to the other place with the new size based on the previous block size. 
![[Pasted image 20251201141550.png]]
Must free the previous memory block, if you'd like to move the memory block. 

`std::vector` and 
###### Behaviour of `Realloc()`

| void* ptr | size_t size             | Result                                                 |
| --------- | ----------------------- | ------------------------------------------------------ |
| null      | NA                      | Same as `malloc()`                                     |
| not null  | 0                       | Original block freed, same as `free()`                 |
|           | < size_t (original ptr) | smaller memory block allocated in the new memory block |
|           | > size_t (original ptr) | larger memory block allocated in the new memory block  |

###### Case Study - Make dynamic array in C
Check Pointers and Array document in the link. 
[[Pointers and Array]]

### `calloc()`


#### What if you failed to allocate the memory due to the size issue? 
`ENOMEM` returns when you failed to reallocate or allocate the memory if the memory runs out. 



#### Why you should not use `free()` and `destroy ()` in native? 
However, it is unsafe to use `free()` and `destroy()` in C++  as reasons below: 
- `free()` and `destroy()` do not verify whether the pointer is valid(double-free issue), which risks the dangling pointer issue. 
- `free()` and `destroy()` are likely to return the memory to the OS rather than generally deallocate the memory. 

#### How to deal with dangling pointers
- Setting a pointer to `NULL`, `0`, or `nullptr` when initialising or deleting the pointer. 
- Writing the functions to replace the `free` function to prevent the double-free problem. 
	- Make sure to have a pointer validation interface in the function
	- Use `NULL`, `0` or `nullptr` (in C++) instead of `free()` or `delete()` (in C++)to completely deallocate the pointer. 
- Using a third-party library for memory management. 

Like `Z_Free()` on Doom source code, making the memory free functions, supports the memory verification and frees the memory the user designates. 
###### Source Code 
```c 
void Z_Free (void* ptr)

{
	memblock_t* block;
	memblock_t* other;
	block = (memblock_t *) ( (byte *)ptr - sizeof(memblock_t));
	
	if (block->id != ZONEID)
	
		I_Error ("Z_Free: freed a pointer without ZONEID");
	
	if (block->user > (void **)0x100) // why 0x100? 
	
	{
		// smaller values are not pointers
		
		// Note: OS-dependend?
		
		// clear the user's mark
		
		*block->user = 0; // set 0 of the id
	
	}

	// mark as free
	
	block->user = NULL; // nullptr
	block->tag = 0; // set 0 on ID/tag (int value) 
	block->id = 0;
	other = block->prev;

	// it seems like they use a linked list to manage the memory. 
	if (!other->user)
	{
	
		// merge with previous free block
		
		other->size += block->size;
		other->next = block->next;
		other->next->prev = other;
		
		if (block == mainzone->rover)
			mainzone->rover = other;
			
		block = other;
		
	}
	other = block->next;
	
	if (!other->user)
	
	{
		// merge the next free block onto the end
		block->size += other->size;
		block->next = other->next;
		block->next->prev = block; 
		if (other == mainzone->rover)
			mainzone->rover = block;
		
	}

}

```

*Check z_zone.c in the DOOM source code to find the more specific interface of the memory allocation in DOOM*


