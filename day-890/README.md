## Problem
Given an array of integers and an integer k, find the maximum sum of a subarray that contains at most k distinct elements. This problem requires finding the optimal subarray within the given array that maximizes the sum while not exceeding the limit of k distinct elements.

## Examples
- Example 1:
  - Input: `nums = [1, 2, 1, 2, 3], k = 2`
  - Output: `6`
  - Explanation: The subarray `[1, 2, 1, 2]` has the maximum sum of `6` with `2` distinct elements.
- Example 2:
  - Input: `nums = [1, 2, 1, 3], k = 1`
  - Output: `3`
  - Explanation: The subarray `[1, 1]` or `[2]` or `[3]` has the maximum sum of `3` or `2` or `3` respectively with `1` distinct element, thus the maximum sum is `3`.
- Example 3:
  - Input: `nums = [1, 2, 3, 4, 5], k = 5`
  - Output: `15`
  - Explanation: The subarray `[1, 2, 3, 4, 5]` has the maximum sum of `15` with `5` distinct elements.

## Approach
To solve this problem, we will use the sliding window technique along with a hash map to keep track of the distinct elements within the current window. The approach can be broken down into the following steps:
1. Initialize two pointers, `left` and `right`, to represent the start and end of the sliding window.
2. Initialize a hash map to store the frequency of each element within the current window.
3. Initialize a variable to store the maximum sum found so far.
4. Expand the window to the right by moving the `right` pointer and updating the hash map and the current sum.
5. When the number of distinct elements exceeds `k`, shrink the window from the left by moving the `left` pointer and updating the hash map and the current sum.
6. Repeat steps 4 and 5 until the `right` pointer reaches the end of the array.

## Solution
```python
from collections import defaultdict

def maximum_subarray_sum(nums, k):
    """
    Find the maximum sum of a subarray with at most k distinct elements.
    
    Args:
    nums (list): A list of integers.
    k (int): The maximum number of distinct elements allowed.
    
    Returns:
    int: The maximum sum of a subarray with at most k distinct elements.
    """
    # Initialize variables to store the maximum sum and the current sum
    max_sum = float('-inf')
    current_sum = 0
    
    # Initialize a hash map to store the frequency of each element
    freq_map = defaultdict(int)
    
    # Initialize the left pointer
    left = 0
    
    # Iterate over the array with the right pointer
    for right in range(len(nums)):
        # Add the current element to the current sum
        current_sum += nums[right]
        
        # Increment the frequency of the current element
        freq_map[nums[right]] += 1
        
        # While there are more than k distinct elements, shrink the window
        while len(freq_map) > k:
            # Decrement the frequency of the leftmost element
            freq_map[nums[left]] -= 1
            
            # If the frequency of the leftmost element is 0, remove it from the hash map
            if freq_map[nums[left]] == 0:
                del freq_map[nums[left]]
            
            # Subtract the leftmost element from the current sum
            current_sum -= nums[left]
            
            # Move the left pointer to the right
            left += 1
        
        # Update the maximum sum
        max_sum = max(max_sum, current_sum)
    
    return max_sum
```

## Complexity
- Time: O(n) — The time complexity is linear because each element is visited at most twice, once by the `right` pointer and once by the `left` pointer.
- Space: O(k) — The space complexity is proportional to the number of distinct elements allowed, because in the worst case, the hash map will store the frequency of `k` distinct elements.

## Key Insight
The core trick to solve this problem is using a sliding window approach along with a hash map to efficiently track the distinct elements within the current window and to ensure that the number of distinct elements does not exceed `k`.