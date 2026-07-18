## Problem
Given two sorted arrays, find the median of the combined array without merging them. This problem requires an efficient approach to calculate the median without physically combining the two arrays, which could be large and impractical to merge.

## Examples
- Example 1: 
  - Input: `nums1 = [1, 3]`, `nums2 = [2]`
  - Output: `2.0`
- Example 2: 
  - Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  - Output: `(2 + 3) / 2 = 2.5`

## Approach
To solve this problem, we can use a binary search approach. The idea is to find a partition point for both arrays such that the elements on the left side of the partition point in both arrays are less than or equal to the elements on the right side. This partition point will help us find the median of the combined array.

Here are the steps to the approach:
1. Calculate the total length of both arrays combined.
2. Determine if the total length is odd or even, as this affects how we calculate the median.
3. Initialize binary search boundaries based on the lengths of the two arrays.
4. Perform a binary search to find the correct partition point.
5. Calculate the median based on the partition point and whether the total length is odd or even.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array to simplify the logic
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of both arrays
    total_length = len(nums1) + len(nums2)
    
    # Initialize the binary search boundaries
    left, right = 0, len(nums1)
    
    while left <= right:
        # Calculate the partition point for nums1
        partition_nums1 = (left + right) // 2
        
        # Calculate the partition point for nums2 based on the partition point for nums1
        partition_nums2 = (total_length + 1) // 2 - partition_nums1
        
        # Calculate the max and min values for the left and right partitions
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
        # Adjust the partition if necessary
        elif max_left_nums1 > min_right_nums2:
            right = partition_nums1 - 1
        else:
            left = partition_nums1 + 1
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is logarithmic in the size of the smaller array because we are performing a binary search on the smaller array to find the correct partition point.
- Space: O(1) — The space complexity is constant because we are not using any additional data structures that scale with the input size.

## Key Insight
The core trick to solving this problem is to use binary search to find a partition point for both arrays such that the elements on the left side of the partition point in both arrays are less than or equal to the elements on the right side, allowing us to efficiently calculate the median without merging the arrays.