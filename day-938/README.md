## Problem
Given two sorted arrays, find the median of the combined array formed by merging the two arrays. The median is the middle value in an ordered integer list. If the total number of elements in the combined array is even, the median is the average of the two middle values.

## Examples
- Input: `nums1 = [1, 3]`, `nums2 = [2]`, Output: `2.0`
- Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`, Output: `2.5`
- Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`, Output: `0.0`

## Approach
To solve this problem, we can use a binary search approach. The idea is to find a partition point for both arrays such that the elements on the left side of the partition point in both arrays are less than or equal to the elements on the right side. We then calculate the median based on the elements at the partition points. 
Here are the steps:
1. Ensure that `nums1` is the smaller array to simplify the logic.
2. Calculate the total length of the combined array.
3. Initialize the binary search range as the length of `nums1`.
4. Perform binary search to find the partition point for `nums1`.
5. Calculate the corresponding partition point for `nums2` based on the total length and the partition point of `nums1`.
6. Check if the partition is correct by comparing the elements at the partition points.
7. If the partition is not correct, adjust the binary search range and repeat steps 4-6.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length
    total_length = len(nums1) + len(nums2)
    
    # Initialize the binary search range
    left, right = 0, len(nums1)
    
    while left <= right:
        # Calculate the partition point for nums1
        partition_num1 = (left + right) // 2
        
        # Calculate the partition point for nums2
        partition_num2 = (total_length + 1) // 2 - partition_num1
        
        # Calculate the values at the partition points
        max_left_num1 = float('-inf') if partition_num1 == 0 else nums1[partition_num1 - 1]
        min_right_num1 = float('inf') if partition_num1 == len(nums1) else nums1[partition_num1]
        
        max_left_num2 = float('-inf') if partition_num2 == 0 else nums2[partition_num2 - 1]
        min_right_num2 = float('inf') if partition_num2 == len(nums2) else nums2[partition_num2]
        
        # Check if the partition is correct
        if max_left_num1 <= min_right_num2 and max_left_num2 <= min_right_num1:
            # Calculate the median
            if total_length % 2 == 0:
                return (max(max_left_num1, max_left_num2) + min(min_right_num1, min_right_num2)) / 2
            else:
                return max(max_left_num1, max_left_num2)
        # Adjust the binary search range
        elif max_left_num1 > min_right_num2:
            right = partition_num1 - 1
        else:
            left = partition_num1 + 1
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is logarithmic because we are performing a binary search on the smaller array. The search space is reduced by half at each step, resulting in a logarithmic time complexity.
- Space: O(1) — The space complexity is constant because we are not using any data structures that scale with the input size. We only use a constant amount of space to store the variables.

## Key Insight
The core trick to solve this problem is to use binary search to find the partition point that divides the combined array into two halves with the same total length, allowing us to efficiently calculate the median.