## Problem
Given an array of integers, find the length of the longest subsequence that is strictly increasing. This problem can be solved using dynamic programming, which is an efficient method for solving complex problems by breaking them down into simpler subproblems. The goal is to identify the longest sequence where each element is greater than the previous one.

## Examples
- Input: `nums = [10, 9, 2, 5, 3, 7, 101, 18]`, Output: `4` (because the longest increasing subsequence is `[2, 3, 7, 101]` or `[2, 5, 7, 101]`)
- Input: `nums = [0, 1, 0, 3, 2, 3]`, Output: `4` (because the longest increasing subsequence is `[0, 1, 2, 3]`)
- Input: `nums = [7, 6, 5, 4, 3, 2]`, Output: `1` (because the longest increasing subsequence is any single element, e.g., `[7]`)

## Approach
The approach to solving this problem involves using dynamic programming to track the length of the longest increasing subsequence ending at each position in the array. In plain English, the algorithm works by iterating through the array and at each step, it checks all previous elements. If the current element is greater than a previous element, it means we can potentially extend the increasing subsequence ending at the previous element by appending the current element. We keep track of the maximum length of such subsequences seen so far.

Here are the steps in detail:
1. Initialize an array `dp` where `dp[i]` will store the length of the longest increasing subsequence ending at index `i`.
2. Initialize all values in `dp` to 1, since a single element is itself a subsequence of length 1.
3. Iterate through the array from the second element to the last. For each element, compare it with all previous elements.
4. If the current element is greater than a previous element, update `dp[i]` if the length of the subsequence ending at the previous element plus one is greater than the current `dp[i]`.
5. After filling up the `dp` array, find the maximum value in it, which represents the length of the longest increasing subsequence.

## Solution
```python
def lengthOfLIS(nums):
    """
    This function calculates the length of the longest increasing subsequence in a given array of integers.
    
    Parameters:
    nums (list): The input list of integers.
    
    Returns:
    int: The length of the longest increasing subsequence.
    """
    # If the list is empty, return 0
    if not nums:
        return 0
    
    # Initialize a list to store the length of the longest increasing subsequence ending at each position
    dp = [1] * len(nums)
    
    # Iterate over the list
    for i in range(1, len(nums)):
        # For each element, compare it with all previous elements
        for j in range(i):
            # If the current element is greater than the previous element, update dp[i] if necessary
            if nums[i] > nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)
    
    # Return the maximum length found
    return max(dp)

# Example usage
print(lengthOfLIS([10, 9, 2, 5, 3, 7, 101, 18]))  # Output: 4
print(lengthOfLIS([0, 1, 0, 3, 2, 3]))  # Output: 4
print(lengthOfLIS([7, 6, 5, 4, 3, 2]))  # Output: 1
```

## Complexity
- Time: O(n^2) — The algorithm has a nested loop structure where for each element in the array, we potentially compare it with every previous element. This leads to a quadratic time complexity.
- Space: O(n) — We use an additional array `dp` of the same length as the input array to store the lengths of the longest increasing subsequences ending at each position. This requires linear space.

## Key Insight
The core trick to solving this problem efficiently is recognizing that the length of the longest increasing subsequence ending at any position can be determined by considering the maximum length of increasing subsequences ending at all previous positions that could potentially be extended by the current element.