## Problem
The Longest Increasing Subsequence problem is a classic problem in computer science and mathematics, where we are given a sequence of integers and need to find the length of the longest subsequence that is strictly increasing. The condition is that each element in the sequence can only be used once. This means that if we have a sequence like [1, 2, 3, 4, 5], the longest increasing subsequence would be [1, 2, 3, 4, 5] itself, with a length of 5. However, if the sequence is [5, 4, 3, 2, 1], the longest increasing subsequence would be [5] or [4] or [3] or [2] or [1], each with a length of 1.

## Examples
* Input: `nums = [10, 9, 2, 5, 3, 7, 101, 18]`, Output: `4` (The longest increasing subsequence is [2, 3, 7, 101])
* Input: `nums = [0, 1, 0, 3, 2, 3]`, Output: `4` (The longest increasing subsequence is [0, 1, 2, 3])
* Input: `nums = [7, 6, 5, 4, 3, 2]`, Output: `1` (The longest increasing subsequence is [7] or [6] or [5] or [4] or [3] or [2])

## Approach
To solve this problem using dynamic programming, we can think of it as building a solution from smaller sub-problems. The idea is to maintain an array `dp` where `dp[i]` represents the length of the longest increasing subsequence ending at index `i`. We start by initializing all elements in `dp` to 1, since the smallest possible increasing subsequence is of length 1 (the element itself). Then, for each element in the sequence, we compare it with all previous elements. If the current element is greater than a previous element, it means we can potentially extend the increasing subsequence ending at the previous element by appending the current element. Therefore, we update `dp[i]` to be the maximum of its current value and `dp[j] + 1`, where `j` is the index of the previous element being compared.

Here's the step-by-step breakdown:
1. Initialize a `dp` array with the same length as the input sequence, with all elements set to 1.
2. Iterate over the sequence from the second element to the last.
3. For each element, compare it with all previous elements.
4. If the current element is greater than a previous element, update `dp[i]` to be the maximum of its current value and `dp[j] + 1`.
5. After filling the `dp` array, the length of the longest increasing subsequence is the maximum value in the `dp` array.

## Solution
```python
def lengthOfLIS(nums):
    if not nums:
        return 0

    # Initialize dp array with all elements as 1
    dp = [1] * len(nums)

    # Iterate over the sequence
    for i in range(1, len(nums)):
        # Compare with all previous elements
        for j in range(i):
            # If current element is greater, update dp[i]
            if nums[i] > nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)

    # Return the maximum value in dp array
    return max(dp)
```

## Complexity
- Time: O(n^2) — The time complexity is quadratic because we have a nested loop structure where for each element in the sequence, we are comparing it with all previous elements. The outer loop runs in O(n) time and the inner loop also runs in O(n) time, resulting in O(n^2) overall.
- Space: O(n) — The space complexity is linear because we are using a `dp` array of the same length as the input sequence to store the lengths of the longest increasing subsequences ending at each position.

## Key Insight
The core trick to solving this problem is to recognize that the length of the longest increasing subsequence ending at any position can be determined by considering the lengths of the longest increasing subsequences of all previous positions that have a smaller value.