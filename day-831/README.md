## Problem
The Longest Increasing Subsequence (LIS) problem is a classic problem in computer science and mathematics. Given an array of integers, the goal is to find the length of the longest subsequence that is strictly increasing. This means that for any two elements in the subsequence, the element that appears later in the subsequence must be greater than the element that appears earlier. The subsequence does not need to be contiguous, meaning that the elements do not need to appear next to each other in the original array.

## Examples
* Input: `arr = [10, 22, 9, 33, 21, 50, 41, 60]`
  Output: `5`
  Explanation: The longest increasing subsequence is `[10, 22, 33, 50, 60]`.
* Input: `arr = [1, 2, 3, 4, 5]`
  Output: `5`
  Explanation: The longest increasing subsequence is `[1, 2, 3, 4, 5]`.
* Input: `arr = [5, 4, 3, 2, 1]`
  Output: `1`
  Explanation: The longest increasing subsequence is `[5]`.

## Approach
The Longest Increasing Subsequence problem can be solved using dynamic programming. The idea is to create an array `dp` where `dp[i]` represents the length of the longest increasing subsequence ending at index `i`. We initialize `dp` with all elements as 1, because the minimum length of the longest increasing subsequence ending at any index is 1 (the element itself). Then, for each element in the array, we compare it with all previous elements. If the current element is greater than a previous element, it means that we can extend the increasing subsequence ending at the previous element by appending the current element. Therefore, we update `dp[i]` to be the maximum of its current value and `dp[j] + 1`, where `j` is the index of the previous element.

## Solution
```python
def length_of_lis(arr):
    """
    Returns the length of the longest increasing subsequence in the given array.
    
    Args:
        arr (list): A list of integers.
    
    Returns:
        int: The length of the longest increasing subsequence.
    """
    if not arr:
        return 0
    
    # Initialize dp array with all elements as 1
    dp = [1] * len(arr)
    
    # Iterate over the array
    for i in range(1, len(arr)):
        # Compare the current element with all previous elements
        for j in range(i):
            # If the current element is greater than the previous element, update dp[i]
            if arr[i] > arr[j]:
                dp[i] = max(dp[i], dp[j] + 1)
    
    # Return the maximum value in dp array
    return max(dp)

# Test the function
print(length_of_lis([10, 22, 9, 33, 21, 50, 41, 60]))  # Output: 5
print(length_of_lis([1, 2, 3, 4, 5]))  # Output: 5
print(length_of_lis([5, 4, 3, 2, 1]))  # Output: 1
```

## Complexity
- Time: O(n^2) — The solution has two nested loops, each iterating over the array of length n. This results in a quadratic time complexity.
- Space: O(n) — The solution uses an additional array `dp` of length n to store the lengths of the longest increasing subsequences ending at each index.

## Key Insight
The core trick to solve this problem is to use dynamic programming to build up a table of lengths of the longest increasing subsequences ending at each index, by comparing each element with all previous elements and updating the table accordingly.