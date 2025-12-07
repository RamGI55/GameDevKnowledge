https://leetcode.com/problems/top-k-frequent-elements/description/?envType=problem-list-v2&envId=heap-priority-queue

Given an integer array `nums` and an integer `k`, return _the_ `k` _most frequent elements_. You may return the answer in **any order**.

### Naive Approach - Heap 

In the simple heap approach, with an `O(N log k)` time complexity. 

``` 
HEAP APPROACH 
topKFrequent(A , i)

// build a HashMap element to its frequency.
Hashmap with <int,int> 
	FOR from 1 UP TO A.size()
	ADD HASHMAP

// build the heap of size k using N elements 
HASH For FREQ as A.size()
Min-HeapSort from 1 up to freq.size() 

// convert heap into an output array. 
Output Array
	Push_back freq[i].first on the Output Array

RETURN Output Array. 

```

**Answer (C++)**

```cpp 
class Solution {
public:

// heap approach O(n log k)
static bool compare (pair<int, int>&p1, pair<int, int>&p2){
	
	return p1.second > p2.second;
}

vector<int> topKFrequent(vector<int>& nums, int k) {
	int n = (int)nums.size();
	unordered_map<int,int> mp;
		for (int i = 0; i < n; i++)
		{
			mp[nums[i]] ++;
		}

	vector<pair<int,int>> freq (mp.begin(), mp.end());
	sort(freq.begin(), freq.end(), compare); // using compare sort to make it as min-heap. 
	vector<int>answer;
		for (int i = 0; i < k ; i++)
		{
			answer.push_back(freq[i].first);
		}
		return answer; 
	} 
}; 

```


### Expected Approach - Quick Sort 
Quick sort is typically used to solve the problems of "find `k`th something". 