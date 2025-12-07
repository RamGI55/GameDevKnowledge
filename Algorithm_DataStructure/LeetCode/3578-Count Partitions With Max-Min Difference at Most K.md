https://leetcode.com/problems/count-partitions-with-max-min-difference-at-most-k/description/?source=submission-ac

Using the dp and sliding window (two-pointer) to get the count partition under the condition. 

### DP + Sliding Window Approach - O(n log n)

make the dp to define the number of partitions for each nums container's index. Th

> `std::multiset` is the set container that stores the elements following a specific order. 
> The value of the elements also identifies ad iterators. The values cannot be modified once the elements are emplaced in the container. 

```
COUNTINGPARTITIONS (A, k)
	Make dp[] size of A.size()+1 // define number of the partition for nums[0...n]
	SET dp[0] = 1
	Make prefixsum[] size of A.size()+1

	FOR 0 UPTO A.size()-1, emplace nums[0...n] to ordered_set[]
		IF j <= i && current max - min > k 
		Erase (nums[j])
		j++
	compute dp[i+1] = prefix[i]- prefix[j-1]
	updated prefix[i + 1]= prefix[i] + dp[i+1]

RETURN dp[n];
		
```

C++ Code implementation 

```cpp
class Solution {
	public:
		int countPartitions(vector<int>& nums, int k) {
			int n = nums.size();
			int modulo = 1e9+7;
			
			// intialised the dp table.
			
			std::vector<int> dp(n+1, 0);
			dp[0] = 1;
			std::vector<int>prefixSum(n+1, 0);
			
			prefixSum[0] = 1;
			multiset<int> cnt;
		
			for (int i = 0, j = 0 ; i < n ; i++)
			{
			
				cnt.emplace(nums[i]);
	
				while( j <= i && *cnt.rbegin() - *cnt.begin() > k)
				{
				
					cnt.erase(cnt.find(nums[j]));
					j++;
				
				}
				
					dp[i+ 1] = (prefixSum[i]- (j> 0 ? prefixSum[j-1] : 0 ) + modulo) % modulo;
					
					prefixSum [i+1] = (prefixSum[i] + dp[i+1]) % modulo;
			
			}
		
		return dp[n];
		
	}
};
```


### Optimised Solution - Backtracking min-max dp table

