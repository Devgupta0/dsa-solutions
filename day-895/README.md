## Problem
The problem requires finding the length of the longest increasing subsequence with unique elements in a given array of integers. This means that each element in the array can be used only once, and the subsequence must be strictly increasing. The goal is to determine the maximum length of such a subsequence.

## Examples
* Input: `arr = [10, 22, 9, 33, 21, 50, 41, 60, 80]`
  Output: `6` (one possible subsequence is `[10, 22, 33, 50, 60, 80]`)
* Input: `arr = [1, 2, 3, 4, 5]`
  Output: `5` (the entire array is an increasing subsequence)
* Input: `arr = [5, 4, 3, 2, 1]`
  Output: `1` (only single-element subsequences are possible)

## Approach
To solve this problem, we can use dynamic programming to build up a solution. The idea is to maintain an array where each element represents the length of the longest increasing subsequence ending at that index. We iterate through the array, and for each element, we check all previous elements to see if they are smaller. If they are, we update the length of the subsequence at the current index if necessary.
Here are the steps:
1. Initialize an array `dp` of the same length as the input array, with all elements set to 1 (since a single element is always an increasing subsequence of length 1).
2. Iterate through the array from left to right.
3. For each element, iterate through all previous elements.
4. If a previous element is smaller than the current element, update the length of the subsequence at the current index if the length of the subsequence at the previous index plus one is greater than the current length.
5. Keep track of the maximum length found so far.

## Solution
```python
def length_of_lis(arr):
    """
    Find the length of the longest increasing subsequence with unique elements.
    
    Args:
    arr (list): A list of integers.
    
    Returns:
    int: The length of the longest increasing subsequence.
    """
    if not arr:
        return 0
    
    # Initialize the dp array with all elements set to 1
    dp = [1] * len(arr)
    
    # Initialize the maximum length found so far
    max_length = 1
    
    # Iterate through the array from left to right
    for i in range(1, len(arr)):
        # Iterate through all previous elements
        for j in range(i):
            # If the previous element is smaller, update the length of the subsequence
            if arr[j] < arr[i]:
                dp[i] = max(dp[i], dp[j] + 1)
        
        # Update the maximum length found so far
        max_length = max(max_length, dp[i])
    
    return max_length
```

## Complexity
- Time: O(n^2) — The time complexity is quadratic because we have two nested loops, each iterating through the array. The outer loop runs in O(n) time, and the inner loop also runs in O(n) time, resulting in a total time complexity of O(n^2).
- Space: O(n) — The space complexity is linear because we need to store the `dp` array, which has the same length as the input array.

## Key Insight
The core trick to solving this problem is to use dynamic programming to build up a solution by maintaining an array of lengths of the longest increasing subsequences ending at each index.