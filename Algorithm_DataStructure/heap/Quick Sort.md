

### Partitioning The Array 

**Divide** 

**Conquer** 

```
QUICKSORT (A, p, r)
if q < r
	q = PARTITION (A, p,r)
	QUICKSORT (A, p, q-1)
	QUICKSORT (A, q+1, r)
```


```
PARTITION (A,p,r)
x = A[r]
i = p - 1
for j = p to r -1 
	if A[j] <= x 
		i =i+1 
		exchange A[i] with A[j]
exchange A[i+1] with A[r]
return i + 1 
```

**Initalisation** 

**Maintenance** 

**Termination** 

### Performance of Quicksort 
##### Worst-case Partitioning 



### Randomised Version of Quicksort 

