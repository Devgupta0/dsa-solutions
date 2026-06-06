## Problem
Given an array of integers, find the length of the longest increasing subsequence. This problem involves finding a subsequence where every element is larger than the previous one, and we need to optimize the solution using dynamic programming and binary search.

## Examples
* Input: `nums = [10, 9, 2, 5, 3, 7, 101, 18]`, Output: `4` (The longest increasing subsequence is `[2, 3, 7, 101]`)
* Input: `nums = [0, 1, 0, 3, 2, 3]`, Output: `4` (The longest increasing subsequence is `[0, 1, 2, 3]`)
* Input: `nums = [7, 6, 5, 4, 3, 2]`, Output: `1` (The longest increasing subsequence is `[7]`)

## Approach
The problem can be solved by using dynamic programming to store the length of the longest increasing subsequence ending at each position, and then using binary search to optimize the solution. In plain English, the algorithm works as follows: we initialize an array to store the longest increasing subsequence, and then we iterate over the input array. For each element, we find the largest previous element that is smaller than the current one, and we update the length of the longest increasing subsequence accordingly.

Here are the steps:
1. Initialize an array `dp` to store the longest increasing subsequence.
2. Initialize an array `tails` to store the smallest tail of each subsequence.
3. Iterate over the input array `nums`.
4. For each element `num`, use binary search to find the largest previous element that is smaller than `num`.
5. If `num` is larger than the last element in `tails`, append it to `tails`.
6. Otherwise, update the corresponding element in `tails` with `num`.
7. The length of the longest increasing subsequence is the length of `tails`.

## Solution
```python
from bisect import bisect_left

def lengthOfLIS(nums):
    """
    Find the length of the longest increasing subsequence in the given array.
    
    Args:
    nums (list): The input array of integers.
    
    Returns:
    int: The length of the longest increasing subsequence.
    """
    # Initialize the tails array with the first element of nums
    tails = [nums[0]]
    
    # Iterate over the rest of the elements in nums
    for num in nums[1:]:
        # If num is larger than the last element in tails, append it to tails
        if num > tails[-1]:
            tails.append(num)
        # Otherwise, update the corresponding element in tails with num
        else:
            # Use binary search to find the index where num should be inserted
            idx = bisect_left(tails, num)
            tails[idx] = num
    
    # The length of the longest increasing subsequence is the length of tails
    return len(tails)
```

## Complexity
- Time: O(n log n) — because we are iterating over the input array and using binary search to find the position where each element should be inserted.
- Space: O(n) — because in the worst case, the `tails` array can grow up to the size of the input array.

## Key Insight
The core trick to solve this problem is to use binary search to find the position where each element should be inserted in the `tails` array, which allows us to maintain the smallest tail of each subsequence and optimize the solution.