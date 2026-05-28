## Problem
Given two sorted arrays, find the median of the combined array without merging them. The arrays can be of different lengths, and the median is the middle value in the sorted array. If the combined array has an even number of elements, the median is the average of the two middle values.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`
  Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  Output: `2.5`
* Input: `nums1 = [1, 3, 5]`, `nums2 = [2, 4]`
  Output: `3.0`

## Approach
The approach to solve this problem is to use binary search to find the partition point for both arrays such that the elements on the left side of the partition point in both arrays are less than or equal to the elements on the right side. This is based on the fact that the median is the middle value in the sorted array. We can use binary search to find this partition point in O(log(min(m, n))) time, where m and n are the lengths of the two arrays.

Here are the steps to solve the problem:
1. Calculate the total length of the combined array.
2. Determine if the total length is odd or even, as this will affect how we calculate the median.
3. Initialize two pointers, one at the start of the first array and one at the start of the second array.
4. Use binary search to find the partition point for both arrays.
5. Calculate the median based on the partition point and the total length of the combined array.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Make sure that nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of the combined array
    total_length = len(nums1) + len(nums2)
    
    # Initialize the low and high pointers for binary search
    low = 0
    high = len(nums1)
    
    while low <= high:
        # Calculate the partition point for nums1
        partition_nums1 = (low + high) // 2
        
        # Calculate the partition point for nums2
        partition_nums2 = (total_length + 1) // 2 - partition_nums1
        
        # Calculate the values at the partition points
        max_left_nums1 = float('-inf') if partition_nums1 == 0 else nums1[partition_nums1 - 1]
        min_right_nums1 = float('inf') if partition_nums1 == len(nums1) else nums1[partition_nums1]
        
        max_left_nums2 = float('-inf') if partition_nums2 == 0 else nums2[partition_nums2 - 1]
        min_right_nums2 = float('inf') if partition_nums2 == len(nums2) else nums2[partition_nums2]
        
        # Check if the partition is correct
        if max_left_nums1 <= min_right_nums2 and max_left_nums2 <= min_right_nums1:
            # Calculate the median
            if total_length % 2 == 0:
                return (max(max_left_nums1, max_left_nums2) + min(min_right_nums1, min_right_nums2)) / 2
            else:
                return max(max_left_nums1, max_left_nums2)
        # If the partition is not correct, adjust the pointers
        elif max_left_nums1 > min_right_nums2:
            high = partition_nums1 - 1
        else:
            low = partition_nums1 + 1
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is O(log(min(m, n))) because we use binary search to find the partition point, and the number of iterations is proportional to the logarithm of the length of the smaller array.
- Space: O(1) — The space complexity is O(1) because we only use a constant amount of space to store the pointers and the values at the partition points.

## Key Insight
The core trick to solve this problem is to use binary search to find the partition point for both arrays such that the elements on the left side of the partition point in both arrays are less than or equal to the elements on the right side, allowing us to calculate the median in O(log(min(m, n))) time.