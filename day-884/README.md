## Problem
Given two sorted arrays, find the median of the combined array without merging them. This problem requires finding a way to calculate the median of two sorted arrays in a time-efficient manner, without combining them into a single array.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`, Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`, Output: `2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`, Output: `0.0`

## Approach
The approach to solve this problem involves using binary search to find the median of the combined array. We can think of this problem as finding a partition point for both arrays such that the elements on the left side of the partition point in both arrays are less than or equal to the elements on the right side. 
Here's a step-by-step breakdown:
1. Ensure that `nums1` is the smaller array to simplify the logic.
2. Calculate the total length of both arrays to determine if the median will be a single middle element or an average of two middle elements.
3. Initialize the binary search range to be from 0 to the length of `nums1`.
4. Perform binary search to find the partition point for `nums1`.
5. For each partition point of `nums1`, calculate the corresponding partition point for `nums2` based on the total length of both arrays.
6. Compare the elements at the partition points and adjust the binary search range accordingly.
7. Once the correct partition points are found, calculate the median based on the elements at these points.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of both arrays
    total_length = len(nums1) + len(nums2)
    
    # Initialize the binary search range
    left, right = 0, len(nums1)
    
    while left <= right:
        # Calculate the partition point for nums1
        partition_nums1 = (left + right) // 2
        
        # Calculate the partition point for nums2
        partition_nums2 = (total_length + 1) // 2 - partition_nums1
        
        # Calculate the max element on the left side of nums1 and the min element on the right side of nums1
        max_left_nums1 = float('-inf') if partition_nums1 == 0 else nums1[partition_nums1 - 1]
        min_right_nums1 = float('inf') if partition_nums1 == len(nums1) else nums1[partition_nums1]
        
        # Calculate the max element on the left side of nums2 and the min element on the right side of nums2
        max_left_nums2 = float('-inf') if partition_nums2 == 0 else nums2[partition_nums2 - 1]
        min_right_nums2 = float('inf') if partition_nums2 == len(nums2) else nums2[partition_nums2]
        
        # Check if the partition is correct
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
- Time: O(log(min(m, n))) — The time complexity is logarithmic because we are using binary search to find the partition point. The search space is reduced by half at each step, and the number of steps is proportional to the logarithm of the size of the smaller array.
- Space: O(1) — The space complexity is constant because we are only using a constant amount of space to store the variables and are not using any data structures that scale with the input size.

## Key Insight
The core trick to solve this problem is to use binary search to find the partition point for the smaller array, which allows us to find the median of the combined array without actually merging the two arrays.