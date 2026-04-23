## Problem
Given two sorted arrays, find the median of the combined array without merging them, considering the overall size of the combined array could be large. The goal is to achieve this efficiently, avoiding the need to sort the entire combined array, which could be very large.

## Examples
* Example 1: Input: `nums1 = [1, 3]`, `nums2 = [2]`, Output: `2.00000`
* Example 2: Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`, Output: `(2 + 3) / 2 = 2.5`
* Example 3: Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`, Output: `0.0`

## Approach
The approach to solving this problem involves using binary search to find the partition point for both arrays such that elements on the left side of the partition in both arrays are less than the elements on the right side. This is based on the concept that if we were to merge the two sorted arrays, the median would be the middle element(s) in the combined sorted array. We aim to find a way to partition both arrays so that the elements on the left side of the partition in both arrays and the elements on the right side of the partition in both arrays are balanced in terms of their contribution to the median calculation.

Step by step:
1. Calculate the total length of both arrays combined.
2. Determine if the total length is odd or even to decide how the median will be calculated (one middle element for odd lengths, average of two middle elements for even lengths).
3. Use binary search to find the appropriate partition points for both arrays.
4. For each potential partition point, compare the elements at the partition boundaries to ensure that the partition is correct (i.e., elements on the left are less than or equal to elements on the right).
5. Adjust the partition points based on comparisons until the correct partition is found that allows for the calculation of the median.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array to simplify the logic
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of both arrays combined
    total_length = len(nums1) + len(nums2)
    
    # Initialize the binary search range
    low = 0
    high = len(nums1)
    
    while low <= high:
        # Calculate the partition point for nums1
        partition_nums1 = (low + high) // 2
        
        # Calculate the corresponding partition point for nums2
        partition_nums2 = (total_length + 1) // 2 - partition_nums1
        
        # Calculate the values at the partition boundaries
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
        elif max_left_nums1 > min_right_nums2:
            # Adjust the partition for nums1 to the left
            high = partition_nums1 - 1
        else:
            # Adjust the partition for nums1 to the right
            low = partition_nums1 + 1
```

## Complexity
- Time: O(log(min(n, m))) — The time complexity is logarithmic because we are using binary search to find the partition point. The search space is the size of the smaller array, hence `min(n, m)` where `n` and `m` are the sizes of the two input arrays.
- Space: O(1) — The space complexity is constant because we are not using any additional data structures that scale with the input size. The variables used are a constant number, regardless of the input array sizes.

## Key Insight
The core trick to solving this problem is using binary search to find the partition points for both arrays while ensuring that elements on the left side of the partition are less than or equal to elements on the right side, thus allowing the efficient calculation of the median without merging the arrays.