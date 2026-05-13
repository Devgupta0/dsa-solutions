## Problem
Given two sorted arrays, find the median of the combined array without merging them, considering the total length of both arrays is large and merging is not efficient. The goal is to calculate the median of the combined array in an efficient manner, avoiding the need to merge the two arrays.

## Examples
* Example 1: 
  Input: `nums1 = [1, 3]`, `nums2 = [2]`
  Output: `2.0`
* Example 2: 
  Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  Output: `2.5`
* Example 3: 
  Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  Output: `0.0`

## Approach
The problem can be solved using binary search. The idea is to find a partition point for both arrays such that the elements on the left side of the partition point in both arrays are less than or equal to the elements on the right side. The median will then be the average of the maximum element on the left side and the minimum element on the right side. 

Here are the steps:
1. Calculate the total length of both arrays.
2. Determine if the total length is odd or even to decide whether the median will be a single middle element or an average of two middle elements.
3. Perform a binary search to find the partition point.
4. Adjust the partition point based on the comparison of elements at the partition point in both arrays.
5. Calculate the median once the correct partition point is found.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array to simplify the code
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of both arrays
    total_length = len(nums1) + len(nums2)
    
    # Determine if the total length is odd or even
    is_total_length_odd = total_length % 2 == 1
    
    # Initialize the low and high pointers for binary search
    low = 0
    high = len(nums1)
    
    while low <= high:
        # Calculate the partition point for nums1
        partition_nums1 = (low + high) // 2
        
        # Calculate the partition point for nums2
        partition_nums2 = (total_length + 1) // 2 - partition_nums1
        
        # Calculate the maximum element on the left side of the partition point in nums1
        max_left_nums1 = float('-inf') if partition_nums1 == 0 else nums1[partition_nums1 - 1]
        
        # Calculate the minimum element on the right side of the partition point in nums1
        min_right_nums1 = float('inf') if partition_nums1 == len(nums1) else nums1[partition_nums1]
        
        # Calculate the maximum element on the left side of the partition point in nums2
        max_left_nums2 = float('-inf') if partition_nums2 == 0 else nums2[partition_nums2 - 1]
        
        # Calculate the minimum element on the right side of the partition point in nums2
        min_right_nums2 = float('inf') if partition_nums2 == len(nums2) else nums2[partition_nums2]
        
        # Check if the partition point is correct
        if max_left_nums1 <= min_right_nums2 and max_left_nums2 <= min_right_nums1:
            # Calculate the median
            if is_total_length_odd:
                median = max(max_left_nums1, max_left_nums2)
            else:
                median = (max(max_left_nums1, max_left_nums2) + min(min_right_nums1, min_right_nums2)) / 2
            return median
        # Adjust the partition point if the maximum element on the left side of nums1 is greater than the minimum element on the right side of nums2
        elif max_left_nums1 > min_right_nums2:
            high = partition_nums1 - 1
        # Adjust the partition point if the maximum element on the left side of nums2 is greater than the minimum element on the right side of nums1
        else:
            low = partition_nums1 + 1
```

## Complexity
- Time: O(log(min(n, m))) — The time complexity is logarithmic because we are performing a binary search on the smaller array. The binary search reduces the search space by half at each step, resulting in a logarithmic time complexity.
- Space: O(1) — The space complexity is constant because we are not using any additional space that scales with the input size.

## Key Insight
The core trick to solve this problem is to use binary search to find the partition point for both arrays, ensuring that the elements on the left side of the partition point in both arrays are less than or equal to the elements on the right side, allowing us to calculate the median efficiently.