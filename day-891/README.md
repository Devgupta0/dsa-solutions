## Problem
Given two sorted arrays, find the median of the merged array without actually merging them, considering the overall time complexity and edge cases for both even and odd total lengths. This problem requires an efficient solution that can handle large inputs and avoid unnecessary computations.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`, Output: `2.0` (since the merged array is `[1, 2, 3]`, which has an odd length, the median is the middle element)
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`, Output: `2.5` (since the merged array is `[1, 2, 3, 4]`, which has an even length, the median is the average of the two middle elements)
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`, Output: `0.0` (since the merged array is `[0, 0, 0, 0]`, which has an even length, the median is the average of the two middle elements)

## Approach
To solve this problem efficiently, we can use a binary search approach to find the median without actually merging the two arrays. We will partition both arrays such that the elements on the left side of the partition are less than or equal to the elements on the right side. We will then use binary search to adjust the partition until we find the correct median.

Here are the steps:

1. Calculate the total length of the merged array.
2. Determine whether the total length is odd or even to decide how to calculate the median.
3. Initialize two pointers, one for each array, to keep track of the current partition.
4. Use binary search to adjust the partition until we find the correct median.
5. If the total length is odd, the median is the maximum element on the left side of the partition.
6. If the total length is even, the median is the average of the maximum element on the left side and the minimum element on the right side.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Make sure that nums1 is the smaller array to simplify the logic
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of the merged array
    total_length = len(nums1) + len(nums2)
    
    # Initialize the low and high pointers for binary search
    low = 0
    high = len(nums1)
    
    while low <= high:
        # Calculate the partition point for nums1
        partition_nums1 = (low + high) // 2
        
        # Calculate the partition point for nums2 based on the partition point for nums1
        partition_nums2 = (total_length + 1) // 2 - partition_nums1
        
        # Calculate the maximum element on the left side of the partition for nums1
        max_left_nums1 = float('-inf') if partition_nums1 == 0 else nums1[partition_nums1 - 1]
        
        # Calculate the minimum element on the right side of the partition for nums1
        min_right_nums1 = float('inf') if partition_nums1 == len(nums1) else nums1[partition_nums1]
        
        # Calculate the maximum element on the left side of the partition for nums2
        max_left_nums2 = float('-inf') if partition_nums2 == 0 else nums2[partition_nums2 - 1]
        
        # Calculate the minimum element on the right side of the partition for nums2
        min_right_nums2 = float('inf') if partition_nums2 == len(nums2) else nums2[partition_nums2]
        
        # Check if the partition is correct
        if max_left_nums1 <= min_right_nums2 and max_left_nums2 <= min_right_nums1:
            # If the total length is odd, return the maximum element on the left side
            if total_length % 2 == 1:
                return max(max_left_nums1, max_left_nums2)
            # If the total length is even, return the average of the maximum element on the left side and the minimum element on the right side
            else:
                return (max(max_left_nums1, max_left_nums2) + min(min_right_nums1, min_right_nums2)) / 2
        # If the partition is not correct, adjust the partition
        elif max_left_nums1 > min_right_nums2:
            high = partition_nums1 - 1
        else:
            low = partition_nums1 + 1
```

## Complexity
- Time: O(log(min(n, m))) — where n and m are the lengths of the two arrays, this is because we use binary search to find the correct partition.
- Space: O(1) — this is because we only use a constant amount of space to store the pointers and the maximum and minimum elements.

## Key Insight
The core trick to solving this problem is to use binary search to find the correct partition point for the two arrays, allowing us to find the median without actually merging them.