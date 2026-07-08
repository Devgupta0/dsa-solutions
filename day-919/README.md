## Problem
Given an array of integers, find the length of the longest subsequence that is strictly increasing. This problem is a classic example of dynamic programming, where we break down the problem into smaller subproblems, solve each subproblem only once, and store their results to subproblems to avoid redundant computation. The goal is to find the longest subsequence where every element is larger than the previous one, and we can use dynamic programming to achieve this efficiently.

## Examples
- Input: `nums = [10, 9, 2, 5, 3, 7, 101, 18]`, Output: `4` (The longest increasing subsequence is `[2, 3, 7, 101]`)
- Input: `nums = [0, 1, 0, 3, 2, 3]`, Output: `4` (The longest increasing subsequence is `[0, 1, 2, 3]`)
- Input: `nums = [7, 6, 5, 4, 3, 2]`, Output: `1` (The longest increasing subsequence is `[7]`)

## Approach
The algorithm to solve this problem involves using dynamic programming to keep track of the length of the longest increasing subsequence ending at each position in the array. In plain English, we start by initializing an array `dp` of the same length as the input array, where `dp[i]` represents the length of the longest increasing subsequence ending at index `i`. We then iterate over the array, for each element, we compare it with all previous elements. If the current element is greater than the previous element, we update `dp[i]` to be the maximum of its current value and `dp[j] + 1`, where `j` is the index of the previous element. This is because we can potentially extend the increasing subsequence ending at `j` by appending the current element.

Step by step:
1. Initialize the `dp` array with all elements set to 1, since a single element is an increasing subsequence of length 1.
2. Iterate over the input array from the second element to the last.
3. For each element, compare it with all previous elements.
4. If the current element is greater than a previous element, update `dp[i]` to be the maximum of its current value and `dp[j] + 1`, where `j` is the index of the previous element.
5. After iterating over the entire array, the maximum value in the `dp` array represents the length of the longest increasing subsequence.

## Solution
```python
def lengthOfLIS(nums):
    # Handle the case when the input array is empty
    if not nums:
        return 0
    
    # Initialize the dp array with all elements set to 1
    dp = [1] * len(nums)
    
    # Iterate over the input array
    for i in range(1, len(nums)):
        # For each element, compare it with all previous elements
        for j in range(i):
            # If the current element is greater than the previous element, update dp[i]
            if nums[i] > nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)
    
    # Return the maximum value in the dp array
    return max(dp)
```

## Complexity
- Time: O(n^2) — This is because we have a nested loop structure where for each element in the array, we are comparing it with all previous elements, resulting in quadratic time complexity.
- Space: O(n) — We need to store the `dp` array of the same length as the input array, hence the linear space complexity.

## Key Insight
The core trick to solving this problem lies in recognizing that the length of the longest increasing subsequence ending at each position can be computed by comparing the current element with all previous elements and updating the `dp` array accordingly, which allows us to avoid redundant computation and solve the problem efficiently using dynamic programming.