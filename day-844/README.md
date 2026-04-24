## Problem
Given two sorted arrays, find the median of the combined array. The overall length of the combined array is large, making a single merge potentially inefficient. We need to find an efficient solution that can handle large inputs.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`
  Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  Output: `(2 + 3) / 2 = 2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  Output: `0.0`

## Approach
To find the median of the combined array, we can use a binary search approach. The idea is to partition both arrays such that the elements on the left side of the partition are smaller than the elements on the right side. We can then use the partition to find the median.

Here are the steps:
1. Calculate the total length of the combined array.
2. Determine if the total length is odd or even. If it's odd, the median is the middle element. If it's even, the median is the average of the two middle elements.
3. Initialize two pointers, one for each array, to the start of the arrays.
4. Use binary search to find the partition point for the first array.
5. Based on the partition point, calculate the corresponding partition point for the second array.
6. Check if the partition is correct by comparing the elements at the partition points.
7. If the partition is not correct, adjust the pointers and repeat the process.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of the combined array
    total_length = len(nums1) + len(nums2)
    
    # Initialize the low and high pointers for binary search
    low, high = 0, len(nums1)
    
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
            # If the total length is odd, return the max of the left side
            if total_length % 2 == 1:
                return max(max_left_nums1, max_left_nums2)
            # If the total length is even, return the average of the max of the left side and the min of the right side
            else:
                return (max(max_left_nums1, max_left_nums2) + min(min_right_nums1, min_right_nums2)) / 2
        # If the partition is not correct, adjust the pointers
        elif max_left_nums1 > min_right_nums2:
            high = partition_nums1 - 1
        else:
            low = partition_nums1 + 1
```

## Complexity
- Time: O(log(min(n, m))) — where n and m are the lengths of the two input arrays. This is because we are using binary search to find the partition point, and the search space is reduced by half at each step.
- Space: O(1) — because we are only using a constant amount of space to store the pointers and the values at the partition points.

## Key Insight
The core trick to solving this problem is to use binary search to find the partition point for the smaller array, which allows us to efficiently find the median of the combined array without having to merge the entire arrays.