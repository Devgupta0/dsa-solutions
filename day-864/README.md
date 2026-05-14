## Problem
Given an array of integers, find the length of the longest increasing subsequence that contains only unique elements, with no element repeated. This means that each element in the subsequence must be greater than the previous element and cannot be duplicated.

## Examples
* Input: `arr = [1, 2, 3, 4, 5]`, Output: `5` (the longest increasing subsequence is `[1, 2, 3, 4, 5]`)
* Input: `arr = [5, 4, 3, 2, 1]`, Output: `1` (the longest increasing subsequence is `[5]`)
* Input: `arr = [1, 3, 5, 4, 7]`, Output: `4` (the longest increasing subsequence is `[1, 3, 5, 7]`)

## Approach
The approach to solve this problem involves using dynamic programming to keep track of the longest increasing subsequence ending at each position in the array. The algorithm works by iterating through the array and for each element, checking all previous elements to see if they are smaller. If a previous element is smaller, it means we can potentially extend the increasing subsequence ending at that previous element by appending the current element.

Here's a step-by-step breakdown:
1. Initialize an array `dp` of the same length as the input array, where `dp[i]` will store the length of the longest increasing subsequence ending at index `i`.
2. Initialize all values in `dp` to 1, since the minimum length of an increasing subsequence ending at any position is 1 (the element itself).
3. Iterate through the array from the second element to the last.
4. For each element, compare it with all previous elements.
5. If the current element is greater than a previous element, update `dp[i]` to be the maximum of its current value and `dp[j] + 1`, where `j` is the index of the previous element.
6. After iterating through the entire array, the maximum value in `dp` represents the length of the longest increasing subsequence.

## Solution
```python
def length_of_lis(arr):
    """
    Given an array of integers, find the length of the longest increasing subsequence that contains only unique elements.
    
    Parameters:
    arr (list): The input array of integers.
    
    Returns:
    int: The length of the longest increasing subsequence.
    """
    if not arr:
        return 0
    
    # Initialize dp array with all values set to 1
    dp = [1] * len(arr)
    
    # Iterate through the array
    for i in range(1, len(arr)):
        # Compare the current element with all previous elements
        for j in range(i):
            # If the current element is greater than the previous element, update dp[i]
            if arr[i] > arr[j]:
                dp[i] = max(dp[i], dp[j] + 1)
    
    # The maximum value in dp represents the length of the longest increasing subsequence
    return max(dp)

# Test the function
print(length_of_lis([1, 2, 3, 4, 5]))  # Output: 5
print(length_of_lis([5, 4, 3, 2, 1]))  # Output: 1
print(length_of_lis([1, 3, 5, 4, 7]))  # Output: 4
```

## Complexity
- Time: O(n^2) — The algorithm has a nested loop structure, where for each element in the array, we are potentially comparing it with all previous elements. This results in a quadratic time complexity.
- Space: O(n) — We are using an additional array `dp` of the same length as the input array to store the lengths of the longest increasing subsequences ending at each position.

## Key Insight
The core trick to solving this problem efficiently lies in using dynamic programming to avoid redundant computations by storing and reusing the lengths of the longest increasing subsequences ending at each position.