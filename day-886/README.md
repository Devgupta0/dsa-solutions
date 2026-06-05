## Problem
Given two sorted arrays, find the median of the combined array formed by merging the two arrays. The median is the middle value in an ordered integer list. If the total number of elements in the combined array is even, the median is the average of the two middle values.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`
  Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  Output: `(2 + 3) / 2 = 2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  Output: `0.0`

## Approach
To solve this problem, we can use a binary search approach to find the median of the combined array. The idea is to partition both arrays such that the elements on the left side of the partition in both arrays are less than the elements on the right side. We then need to find the partition that divides the combined array into two halves with the same number of elements.

Here are the steps:
1. Ensure that `nums1` is the smaller array to simplify the logic.
2. Calculate the total length of the combined array and determine if it's odd or even.
3. Initialize the binary search range to be from 0 to the length of `nums1`.
4. For each possible partition of `nums1`, calculate the corresponding partition of `nums2`.
5. Check if the current partition is valid by comparing the elements at the partition boundaries.
6. If the partition is valid, calculate the median based on the elements at the partition boundaries.
7. If the partition is not valid, adjust the binary search range and repeat the process.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of the combined array
    total_length = len(nums1) + len(nums2)
    
    # Initialize the binary search range
    left, right = 0, len(nums1)
    
    while left <= right:
        # Calculate the partition of nums1
        partition_nums1 = (left + right) // 2
        
        # Calculate the partition of nums2
        partition_nums2 = (total_length + 1) // 2 - partition_nums1
        
        # Calculate the elements at the partition boundaries
        max_left_nums1 = float('-inf') if partition_nums1 == 0 else nums1[partition_nums1 - 1]
        min_right_nums1 = float('inf') if partition_nums1 == len(nums1) else nums1[partition_nums1]
        
        max_left_nums2 = float('-inf') if partition_nums2 == 0 else nums2[partition_nums2 - 1]
        min_right_nums2 = float('inf') if partition_nums2 == len(nums2) else nums2[partition_nums2]
        
        # Check if the current partition is valid
        if max_left_nums1 <= min_right_nums2 and max_left_nums2 <= min_right_nums1:
            # Calculate the median
            if total_length % 2 == 0:
                return (max(max_left_nums1, max_left_nums2) + min(min_right_nums1, min_right_nums2)) / 2
            else:
                return max(max_left_nums1, max_left_nums2)
        # Adjust the binary search range
        elif max_left_nums1 > min_right_nums2:
            right = partition_nums1 - 1
        else:
            left = partition_nums1 + 1
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is logarithmic because we're performing a binary search on the smaller array. The search space is reduced by half at each step, resulting in a logarithmic time complexity.
- Space: O(1) — The space complexity is constant because we're only using a constant amount of space to store the partition boundaries and the elements at those boundaries.

## Key Insight
The core trick to solving this problem is to use a binary search approach to find the partition that divides the combined array into two halves with the same number of elements, allowing us to efficiently calculate the median.