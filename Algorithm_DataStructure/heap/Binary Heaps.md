
A binary heap is a data structure that can be viewed as a nearly complete binary tree. Each node of the three corresponded to an element of the array. 

#### Binary Heaps Interface/Attributes  

- `A.length` - how many elements in the heap are stored within array A. 
- `Heap-size` - # of valid elements of the heap ; 0 <= A.Heap-size <= A.length 

**Index interfaces (1-indexed, root of the tree is A[1])** 
- `Parents(i)` - return i/2 
- `left(i)` return 2i
- `right(i)` return 2i + 1 

- `height` - the longest downward path from the node to a leaf
### Max Heap 
`Max heap` property is that for every node i  other than the root 

```c 
A[parents(i)] >= A[i]
```

`Min Heap`, by the way, is that for every node i other than the root 

```c
A[parents(i)] =< A[i]
```


#### Basic Operation of Heap
`Max-Heapify` runs in `O(lg n)` and is the key to maintaining the max-heap property. 
`Build-Max-Heap` runs in `O(n)` and produces a max heap from an unordered input array.
`Heapsort` runs in `O(n lg n)` time, sorts an array in place. 
`Max-Heap-Insert` ,`Heap-Extract-Max` , `Heap-Increase-Key` ,`Heap-Maximum` procedures runs in `O( lg n)` time 

### `max-heapify` - Maintaining the heap property

```
Max-Heapify (A,i)
l = left(i)
r = right(i)
if l <= A.heap-size and A[l] > A[i]
	largest = l 
else largest = i 
if r <= A.heap-size and A[r] > A[largest]
	largest = r 
if largest != i 
	exchanged A[i] with A[largest]
	Max-Heapify (A, largest)
```

**C++ code implementation** 

```cpp 
#include <iostream> 
#include <vector>

void maxHeapify(std::vector<int>& inV, int i)
{ 
	int l = left(i);
	int r = right(i); 
	int largest = i; 
	
	if (l <= inV.size() && inV[l] > inV[i])
	{ 
		largest = l; 
	}
	if (r <= inV.size() && inV[r] > inV[largest])
	{ 
		largest = r; 
	}
	if (largest != i)
	{ 
		std::swap(inV[i], inV[largest]);
		maxHeapify(inV, largest); 
	}
}
```


### `Build-Max-Heap` - Building Max Heap 

```
BUILD-MAX-HEAP(A)
	A.heap-size = A.length 
	for i = A.length/2 downto 1 
		MAX-HEAPFIY(A,i)
```


##### Why does the function iteration start at length/2 and go down to 1, not up to the last? 


### `HeapSort` - Heap Sort Algorithm
Start by using `Build-Max-Heap` to build a max-heap on the input array `A[1, ..., n]`, where n = A.length. 

```
HEAPSORT(A)
	BUILD-MAX-HEAP(A)
	for i = A.length downto 2 
		exchange A[1] with A[i]
		A.heap-size = A.heap-size - 1 
		MAX-HEAPIFY (A, 1)
```

Heapsort Example

```cpp 
void heapSort (std::vector<int>& v)
{
	for (int i = (int)v.size()/2 ; i >= 0 ; --i)
	{
		maxHeapify(v, 1);
	}
	int heapSize = (int)v.size(); 
	for (int i = heapSize -1 ; i >= 1 ; --1)
	{
		std::swap(v[0], v[i]);
		heapSize--;
		maxHeapify(v, heapSize, 0); // overloaded function, takes the heapsize.  
	}

}

int main() 
{ 
	std::vector<int> heap = {5, 13, 2, 5, 7, 17, 20, 8, 4}; 
	heapSort (heap); .. // 2, 4, 5, 5, 7 ,8, 13, 17, 20.
}

```

In C++, the STL library offers `sort()` to sort, which guarantees the `O(N log N)` time complexity in the worst case. 

```cpp 
std::sort(v.begin(), v.end());       // 2 arguments for setting the range of the sort. 

std::sort(v.begin(), v.end(), comp)  // 3 arguments for comparison function object which returns true if the first argument is less than the second. 
```

`stable_sort()` sorts a range of elements while preserving order between equal elements 
`partial_sort()` sorts the first N elements of the range. 



### Relative Problems 
LeetCodes 
[[347-Top K Frequent Elements]]