## Problem
The problem requires finding the length of the longest subsequence in a given array of integers that is strictly increasing. The condition is that elements can't be compared if they are not adjacent in the subsequence, meaning we can only consider the previous elements in the subsequence when determining if the current element can be added. This is a classic example of a problem that can be solved using dynamic programming.

## Examples
- Example 1: Input: `[10, 9, 2, 5, 3, 7, 101, 18]`, Output: `4` (The longest increasing subsequence is `[2, 3, 7, 101]` or `[2, 5, 7, 101]`).
- Example 2: Input: `[0, 1, 0, 3, 2, 3]`, Output: `4` (The longest increasing subsequence is `[0, 1, 2, 3]`).
- Example 3: Input: `[7, 6, 5, 4, 3, 2]`, Output: `1` (The longest increasing subsequence is any single element, e.g., `[7]`).

## Approach
To solve this problem, we'll use dynamic programming. The idea is to maintain an array, `dp`, where `dp[i]` represents the length of the longest increasing subsequence ending at index `i`. We start by initializing all elements in `dp` to 1, assuming the longest increasing subsequence ending at any position is at least 1 (the element itself). Then, for each element in the array, we compare it with all previous elements. If the current element is greater than a previous element, we update `dp[i]` if the length of the subsequence ending at the previous element plus one is greater than the current `dp[i]`. This way, `dp[i]` will always hold the length of the longest increasing subsequence ending at `i`.

Step by step:
1. Initialize a dynamic programming array `dp` of the same length as the input array, with all elements set to 1.
2. Iterate through the input array. For each element at position `i`:
   - Compare the current element with all elements before it.
   - If the current element is greater than an element at position `j` (where `j < i`), update `dp[i]` to be the maximum of its current value and `dp[j] + 1`.
3. After filling up the `dp` array, the maximum value in `dp` represents the length of the longest increasing subsequence.

## Solution
```python
def lengthOfLIS(nums):
    # Handle edge case where input list is empty
    if not nums:
        return 0

    # Initialize dp array with 1s, assuming the longest subsequence ending at any position is at least 1
    dp = [1] * len(nums)

    # Iterate through the array
    for i in range(1, len(nums)):
        # For each element, compare with all previous elements
        for j in range(i):
            # If current element is greater, update dp[i] if necessary
            if nums[i] > nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)

    # The maximum value in dp is the length of the longest increasing subsequence
    return max(dp)
```

## Complexity
- Time: O(n^2) — This is because we are iterating through the array and for each element, we are potentially comparing it with all previous elements, resulting in a quadratic time complexity.
- Space: O(n) — The space complexity is linear because we are using a dynamic programming array of the same length as the input array to store the lengths of the longest increasing subsequences ending at each position.

## Key Insight
The core trick to solving this problem is recognizing that the length of the longest increasing subsequence ending at any position can be determined by comparing the current element with all previous elements and updating the length if a longer subsequence is found.