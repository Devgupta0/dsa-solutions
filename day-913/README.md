## Problem
Given an array of integers, find the length of the longest subsequence that is strictly increasing and determine the subsequence itself. This problem involves identifying a subsequence where every element is greater than its previous element, and we need to find the longest such subsequence. The subsequence does not have to be contiguous, meaning the elements can be anywhere in the array as long as they are in increasing order.

## Examples
* Input: `[10, 22, 9, 33, 21, 50, 41, 60, 80]`
  Output: Length of LIS is `6` and the subsequence is `[10, 22, 33, 50, 60, 80]`.
* Input: `[3, 4, -1, 0, 6, 2, 3]`
  Output: Length of LIS is `4` and the subsequence is `[-1, 0, 2, 3]`.
* Input: `[1, 2, 3, 4, 5, 6, 7, 8, 9]`
  Output: Length of LIS is `9` and the subsequence is `[1, 2, 3, 4, 5, 6, 7, 8, 9]`.

## Approach
The approach to solving this problem involves using dynamic programming to track the longest increasing subsequence ending at each position in the array. The idea is to compare each element with all its previous elements and update the length of the longest increasing subsequence if a longer subsequence is found. This comparison is repeated for all elements in the array.

Here are the steps:
1. Initialize an array `LIS` where `LIS[i]` will store the length of the longest increasing subsequence ending at index `i`.
2. Initialize another array `prev` to keep track of the previous element in the longest increasing subsequence ending at each index.
3. For each element in the array, compare it with all its previous elements.
4. If the current element is greater than a previous element, it could be part of an increasing subsequence. Update `LIS[i]` and `prev[i]` accordingly.
5. After filling up the `LIS` array, find the maximum length and reconstruct the longest increasing subsequence using the `prev` array.

## Solution
```python
def longest_increasing_subsequence(arr):
    # Initialize arrays to store lengths of LIS and previous elements
    n = len(arr)
    LIS = [1] * n  # Every element is a subsequence of length 1
    prev = [-1] * n  # Initialize previous element as -1

    # Compute LIS lengths and previous elements
    for i in range(1, n):
        for j in range(i):
            if arr[i] > arr[j] and LIS[i] < LIS[j] + 1:
                LIS[i] = LIS[j] + 1
                prev[i] = j

    # Find the maximum length
    max_length_idx = max(range(n), key=lambda i: LIS[i])

    # Reconstruct the longest increasing subsequence
    sequence = []
    while max_length_idx != -1:
        sequence.append(arr[max_length_idx])
        max_length_idx = prev[max_length_idx]

    return LIS[max(range(n), key=lambda i: LIS[i])], sequence[::-1]

# Test the function
arr = [10, 22, 9, 33, 21, 50, 41, 60, 80]
length, subsequence = longest_increasing_subsequence(arr)
print("Length of LIS is", length, "and the subsequence is", subsequence)
```

## Complexity
- Time: O(n^2) — The algorithm involves two nested loops over the array, resulting in a time complexity of O(n^2), where n is the number of elements in the input array.
- Space: O(n) — The space complexity is O(n) because we use additional arrays of the same length as the input array to store the lengths of the longest increasing subsequences and the previous elements.

## Key Insight
The core trick to solving this problem is to use dynamic programming to efficiently track and update the lengths of the longest increasing subsequences ending at each position, allowing for the reconstruction of the longest subsequence itself.