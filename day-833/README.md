## Problem
The problem requires finding the length of the longest subsequence in a given array of integers that is strictly increasing. A subsequence is a sequence that can be derived from another sequence by deleting some elements without changing the order of the remaining elements. The subsequence must be strictly increasing, meaning that each element is greater than the previous one.

## Examples
- Example 1: Input: `[10, 22, 9, 33, 21, 50, 41, 60]`, Output: Length = `5`, Subsequence = `[10, 22, 33, 50, 60]`
- Example 2: Input: `[1, 2, 3, 4, 5]`, Output: Length = `5`, Subsequence = `[1, 2, 3, 4, 5]`
- Example 3: Input: `[5, 4, 3, 2, 1]`, Output: Length = `1`, Subsequence = `[5]`

## Approach
To solve this problem, we will use dynamic programming. The algorithm works by maintaining an array where each element represents the length of the longest increasing subsequence ending at that position. We then fill up this array by comparing each element with all previous elements. If the current element is greater than the previous one, it can be a part of the increasing subsequence, so we update the length accordingly. We also keep track of the actual subsequence.

Here are the steps:
1. Initialize an array `LIS` of the same length as the input array, where `LIS[i]` will store the length of the longest increasing subsequence ending at index `i`.
2. Initialize another array `prev` to keep track of the previous element in the longest increasing subsequence ending at each position.
3. Initialize the maximum length of the longest increasing subsequence and its ending index.
4. Fill up the `LIS` and `prev` arrays by iterating through the input array and comparing each element with all previous elements.
5. Once the arrays are filled, find the maximum length and construct the longest increasing subsequence using the `prev` array.

## Solution
```python
def longest_increasing_subsequence(arr):
    """
    Find the length of the longest subsequence that is strictly increasing and determine the subsequence itself.

    Args:
        arr (list): The input array of integers.

    Returns:
        tuple: A tuple containing the length of the longest increasing subsequence and the subsequence itself.
    """
    n = len(arr)
    if n == 0:
        return 0, []

    # Initialize arrays to store lengths of LIS and previous elements
    LIS = [1] * n
    prev = [-1] * n  # -1 indicates no previous element

    # Initialize maximum length and its index
    max_length = 1
    max_index = 0

    # Fill up LIS and prev arrays
    for i in range(1, n):
        for j in range(i):
            if arr[i] > arr[j] and LIS[i] < LIS[j] + 1:
                LIS[i] = LIS[j] + 1
                prev[i] = j
        if LIS[i] > max_length:
            max_length = LIS[i]
            max_index = i

    # Construct the longest increasing subsequence
    subsequence = []
    index = max_index
    while index != -1:
        subsequence.append(arr[index])
        index = prev[index]

    # Return the length and the subsequence in the correct order
    return max_length, subsequence[::-1]

# Example usage
arr = [10, 22, 9, 33, 21, 50, 41, 60]
length, subsequence = longest_increasing_subsequence(arr)
print(f"Length: {length}, Subsequence: {subsequence}")
```

## Complexity
- Time: O(n^2) — The time complexity is quadratic due to the nested loops used to compare each element with all previous elements.
- Space: O(n) — The space complexity is linear because we need to store the lengths of the longest increasing subsequences and the previous elements for each position in the input array.

## Key Insight
The core trick to solving this problem is to maintain an array where each element represents the length of the longest increasing subsequence ending at that position, allowing us to efficiently compute the longest increasing subsequence by comparing each element with its predecessors.