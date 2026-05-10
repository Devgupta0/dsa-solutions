## Problem
The problem requires finding the length of the longest subsequence in a given array of integers that is strictly increasing. A subsequence is considered strictly increasing if each element is greater than the previous one. The solution should also be able to determine the subsequence itself.

## Examples
- Input: `arr = [10, 22, 9, 33, 21, 50, 41, 60]`
  Output: `Length of LIS: 5, LIS: [10, 22, 33, 50, 60]`
- Input: `arr = [3, 4, -1, 0, 6, 2, 3]`
  Output: `Length of LIS: 4, LIS: [-1, 0, 2, 3]`

## Approach
To solve this problem, we will use dynamic programming. The idea is to maintain an array where each element represents the length of the longest increasing subsequence ending at that index. We start by initializing all elements of this array to 1, since a single element is always an increasing subsequence of length 1. Then, for each element in the array, we compare it with all previous elements. If the current element is greater than a previous element, it means we can extend the increasing subsequence ending at the previous element by appending the current element. We update the length of the longest increasing subsequence ending at the current index if a longer subsequence is found. After filling up the array, the maximum value in the array will represent the length of the longest increasing subsequence. To reconstruct the subsequence itself, we can backtrack from the index with the maximum value.

## Solution
```python
def longest_increasing_subsequence(arr):
    if not arr:
        return 0, []

    n = len(arr)
    # Initialize array to store lengths of LIS ending at each index
    lengths = [1] * n
    # Initialize array to store predecessors in LIS
    predecessors = [None] * n

    # Compute lengths of LIS ending at each index
    for i in range(1, n):
        for j in range(i):
            if arr[i] > arr[j] and lengths[i] < lengths[j] + 1:
                lengths[i] = lengths[j] + 1
                predecessors[i] = j

    # Find index of maximum length
    max_length_idx = max(range(n), key=lambda i: lengths[i])

    # Reconstruct LIS
    lis = []
    while max_length_idx is not None:
        lis.append(arr[max_length_idx])
        max_length_idx = predecessors[max_length_idx]

    # Return length and LIS in correct order
    return lengths[max(range(n), key=lambda i: lengths[i])], lis[::-1]

# Test the function
arr = [10, 22, 9, 33, 21, 50, 41, 60]
length, lis = longest_increasing_subsequence(arr)
print(f"Length of LIS: {length}, LIS: {lis}")
```

## Complexity
- Time: O(n^2) — The solution involves two nested loops over the array of length n, hence the quadratic time complexity.
- Space: O(n) — We use two additional arrays of length n to store the lengths of the longest increasing subsequences and their predecessors, resulting in linear space complexity.

## Key Insight
The core trick to solving this problem lies in utilizing dynamic programming to efficiently compute the lengths of the longest increasing subsequences ending at each index, avoiding redundant computations and enabling the reconstruction of the longest increasing subsequence itself.