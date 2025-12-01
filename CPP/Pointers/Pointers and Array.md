
### Common Misconception of The Array 

	An array and a Pointer are not completely interchangeable.
	The array name is not a pointer, and a pointer is not an array name either. 
	For example, although the name of an array used by itself returns the array's address, we cannot use the name by itself as the target of an assignment. 

The size of the pointer and array is also different.`sizeof` operator for the vector is 20.  But `sizeof` operator against pv returns 4 (in 32-bit), 8 (in 64-bit).
#### Array 

An array is a contiguous collection of homogeneous elements that can be accessed using an index. 
`realloc` uses for array size changes. 

	The realloc function and variable-length arrays provide techniques for handling arrays whose sizes can change. With a little work, we can resize an array and use just the right amount of memory.

#### Get the Size of the Array 

```c
int sizeofArray = sizeof(vector)/sizeof(int); 
```

#### Point Notation and Arrays 


```c 
int vector[5] = {1,2,3,4,5}; 
int *pv = vector; 
```


#### Pointer Arithmetic 

```c 
*(pv+i) // why I should use ()? 
```


#### lvalue vs Rvalue 



#### Making a Dynamic Array in Heap Memory (with `malloc` and `realloc`) 

C99 and C++ support variable-length arrays, which is a better solution than using `realloc`
However, you can also make your own dynamic array, using realloc and malloc as your preference. 
##### Making a Dynamic Array Interface 

```c 
char* getLine(void){
	
	// size of the initial buffer and increment by the request.
	
	const size_t sizeIncrement = 10;
	// the pointer to the character read-in.
	char* buffer = (char*)malloc(sizeIncrement);
	// the pointer to the next free position in the buffer.
	char* currentPos = buffer;
	size_t maxLength = sizeIncrement;
	size_t length = 0;
	int character;
	
	if (currentPos == NULL)
	
	{
	
		return NULL;
	
	}
	
	while(1)
	{
		character = fgetc(stdin);
		if (character == '\n')
		{
			break;
		}

		if (++length >= maxLength) // if the size of the length exceeded the allocated size.
		{
			// reallocate the new size of buffer by the size increment.
			char* newBuffer = realloc(buffer, maxLength += sizeIncrement);
			
				if (newBuffer == NULL)
				{
				
					free(buffer);
					
					return NULL;
				
				}
			currentPos = newBuffer + (currentPos-buffer);
			buffer = newBuffer;
			}
			*currentPos++ = character;
	}
	*currentPos = '\0';
	return buffer;

}

```

##### Trimming the String - Dynamically adjust the size of the array 

```c
char* trim(char* phrase)
{
	char* old = phrase;
	char* new = phrase;
	
	while (*old == ' '){
	
	old++;
	
	}
	
	while (*old)
	{
		*(new++) = *(old++);
	}
	
	*new = 0;
	return (char*) realloc(phrase, strlen(phrase)+1);
}

int main (){

	char* buffer = (char*)malloc(strlen(" cat")+1);
	strcpy(buffer, " cat");
	printf("%s\n", trim(buffer));

}
```

### Example - Vector Array in C by Mashphoe

Small 
https://github.com/Mashpoe/c-vector
