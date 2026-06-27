## Problem
Given two sorted arrays, find the median of the combined array without merging them. The goal is to calculate the median of the combined array in an efficient manner, taking advantage of the fact that the input arrays are already sorted.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`, Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`, Output: `2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`, Output: `0.0`

## Approach
To solve this problem, we can use a binary search approach. The idea is to find the partition point for both arrays such that the elements on the left side of the partition point in both arrays are less than or equal to the elements on the right side. This is based on the fact that the median of a sorted array is the middle element (or the average of the two middle elements if the array has an even number of elements). We will use binary search to find the correct partition point.

Here are the steps:
1. Calculate the total length of the combined array.
2. Determine if the total length is odd or even to decide how to calculate the median.
3. Initialize the low and high pointers for the binary search.
4. Perform the binary search to find the correct partition point.
5. Calculate the median based on the partition point.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Make sure that nums1 is the smaller array to simplify the logic
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of the combined array
    total_length = len(nums1) + len(nums2)
    
    # Initialize the low and high pointers for the binary search
    low = 0
    high = len(nums1)
    
    while low <= high:
        # Calculate the partition point for nums1
        partition_x = (low + high) // 2
        
        # Calculate the partition point for nums2
        partition_y = (total_length + 1) // 2 - partition_x
        
        # Calculate the values at the partition points
        max_left_x = float('-inf') if partition_x == 0 else nums1[partition_x - 1]
        min_right_x = float('inf') if partition_x == len(nums1) else nums1[partition_x]
        
        max_left_y = float('-inf') if partition_y == 0 else nums2[partition_y - 1]
        min_right_y = float('inf') if partition_y == len(nums2) else nums2[partition_y]
        
        # Check if the partition is correct
        if max_left_x <= min_right_y and max_left_y <= min_right_x:
            # Calculate the median
            if total_length % 2 == 0:
                return (max(max_left_x, max_left_y) + min(min_right_x, min_right_y)) / 2
            else:
                return max(max_left_x, max_left_y)
        # Adjust the partition point
        elif max_left_x > min_right_y:
            high = partition_x - 1
        else:
            low = partition_x + 1
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is logarithmic because we are using binary search to find the partition point. The search space is the smaller of the two input arrays.
- Space: O(1) — The space complexity is constant because we are only using a constant amount of space to store the pointers and variables.

## Key Insight
The core trick to solving this problem is to use binary search to find the correct partition point for both arrays, allowing us to calculate the median of the combined array without actually merging the arrays.