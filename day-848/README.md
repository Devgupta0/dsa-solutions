## Problem
The problem requires finding the length of the longest increasing subsequence in an array of integers. An increasing subsequence is a sequence of numbers where each number is greater than the previous one. The goal is to optimize the solution using dynamic programming and binary search to improve efficiency.

## Examples
* Input: `[10, 22, 9, 33, 21, 50, 41, 60, 80]`
  Output: `6` (The longest increasing subsequence is `[10, 22, 33, 50, 60, 80]`)
* Input: `[1, 2, 3, 4, 5]`
  Output: `5` (The longest increasing subsequence is `[1, 2, 3, 4, 5]`)
* Input: `[5, 4, 3, 2, 1]`
  Output: `1` (The longest increasing subsequence is `[5]`)

## Approach
The approach to solve this problem involves using dynamic programming to store the lengths of the longest increasing subsequences ending at each position in the array. However, a naive dynamic programming approach would result in a time complexity of O(n^2). To optimize this, we can use binary search to find the correct position to update in the dynamic programming table. 
Here are the steps:
1. Initialize a dynamic programming table `dp` with the first element of the array.
2. Iterate over the array from the second element to the end.
3. For each element, use binary search to find the largest element in `dp` that is smaller than the current element.
4. If such an element is found, update the next position in `dp` with the current element. If not, append the current element to `dp`.
5. The length of `dp` will be the length of the longest increasing subsequence.

## Solution
```python
from bisect import bisect_left

def lengthOfLIS(nums):
    # Initialize the dynamic programming table with the first element
    dp = [nums[0]]
    
    # Iterate over the array from the second element to the end
    for i in range(1, len(nums)):
        # If the current element is greater than the last element in dp, append it
        if nums[i] > dp[-1]:
            dp.append(nums[i])
        # Otherwise, use binary search to find the correct position to update
        else:
            # Find the index to update using binary search
            idx = bisect_left(dp, nums[i])
            # Update the element at the found index
            dp[idx] = nums[i]
    
    # The length of dp is the length of the longest increasing subsequence
    return len(dp)
```

## Complexity
- Time: O(n log n) — This is because we are iterating over the array once (O(n)) and for each element, we are performing a binary search (O(log n)) to find the correct position to update in the dynamic programming table.
- Space: O(n) — This is because in the worst case, the length of the dynamic programming table `dp` can be equal to the length of the input array `nums`.

## Key Insight
The core trick to solving this problem efficiently is to use binary search to find the correct position to update in the dynamic programming table, reducing the time complexity from O(n^2) to O(n log n).