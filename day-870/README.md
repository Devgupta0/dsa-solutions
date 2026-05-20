## Problem
The Longest Increasing Subsequence problem is a classic problem in computer science and mathematics, where we are given an array of integers and we need to find the length of the longest subsequence that is strictly increasing. A subsequence is a sequence that can be derived from another sequence by deleting some elements without changing the order of the remaining elements. In this case, we also need to determine the subsequence itself.

## Examples
* Input: `arr = [10, 22, 9, 33, 21, 50, 41, 60, 80]`
  Output: `Length: 6`, `Subsequence: [10, 22, 33, 50, 60, 80]`
* Input: `arr = [1, 2, 3, 4, 5]`
  Output: `Length: 5`, `Subsequence: [1, 2, 3, 4, 5]`
* Input: `arr = [5, 4, 3, 2, 1]`
  Output: `Length: 1`, `Subsequence: [5]`

## Approach
To solve this problem, we will use dynamic programming. The idea is to build a table where each cell stores the length of the longest increasing subsequence ending at that index. We start by initializing the table with all ones, since a single element is always an increasing subsequence of length one. Then, for each element in the array, we compare it with all previous elements. If the current element is greater than a previous element, we update the length of the longest increasing subsequence ending at the current index if necessary. We also keep track of the previous element in the longest increasing subsequence ending at each index, so we can reconstruct the subsequence at the end.

Here are the steps:
1. Initialize a table `dp` of the same length as the input array, with all elements set to one.
2. Initialize a table `prev` of the same length as the input array, to keep track of the previous element in the longest increasing subsequence.
3. Iterate over the input array, and for each element, compare it with all previous elements.
4. If the current element is greater than a previous element, update the length of the longest increasing subsequence ending at the current index if necessary, and update the `prev` table.
5. Find the index of the maximum value in the `dp` table, which represents the length of the longest increasing subsequence.
6. Reconstruct the longest increasing subsequence by following the `prev` table from the index of the maximum value.

## Solution
```python
def longest_increasing_subsequence(arr):
    # Initialize the dp table with all ones
    dp = [1] * len(arr)
    # Initialize the prev table to keep track of the previous element
    prev = [-1] * len(arr)
    # Initialize the maximum length and the index of the maximum length
    max_length = 1
    max_index = 0
    
    # Iterate over the input array
    for i in range(1, len(arr)):
        # Compare the current element with all previous elements
        for j in range(i):
            # If the current element is greater than a previous element
            if arr[i] > arr[j]:
                # Update the length of the longest increasing subsequence ending at the current index if necessary
                if dp[i] < dp[j] + 1:
                    dp[i] = dp[j] + 1
                    # Update the prev table
                    prev[i] = j
        # Update the maximum length and the index of the maximum length
        if dp[i] > max_length:
            max_length = dp[i]
            max_index = i
    
    # Reconstruct the longest increasing subsequence
    subsequence = []
    index = max_index
    while index != -1:
        subsequence.append(arr[index])
        index = prev[index]
    # Return the length and the subsequence in reverse order
    return max_length, subsequence[::-1]

# Example usage
arr = [10, 22, 9, 33, 21, 50, 41, 60, 80]
length, subsequence = longest_increasing_subsequence(arr)
print("Length:", length)
print("Subsequence:", subsequence)
```

## Complexity
- Time: O(n^2) — The time complexity is O(n^2) because we are using two nested loops to compare each element with all previous elements.
- Space: O(n) — The space complexity is O(n) because we are using two tables of the same length as the input array to store the lengths of the longest increasing subsequences and the previous elements.

## Key Insight
The core trick to solve this problem is to use dynamic programming to build a table of lengths of the longest increasing subsequences, and to keep track of the previous elements to reconstruct the subsequence at the end.