## Problem
Given two sorted arrays, find the median of the combined array without merging or sorting the entire array. This problem requires finding a way to calculate the median of two sorted arrays in a time-efficient manner, considering the constraint of not merging or sorting the entire array.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`, Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`, Output: `2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`, Output: `0.0`

## Approach
To solve this problem, we can use a binary search approach. The idea is to partition both arrays into two halves each, such that the elements in the left halves are smaller than the elements in the right halves. We can then use binary search to find the correct partition that leads to the median. 
Here's a step-by-step breakdown:
1. Calculate the total length of the combined array.
2. Determine if the total length is odd or even, as this affects how we calculate the median.
3. Initialize the low and high pointers for binary search, considering the smaller array to ensure we don't go out of bounds.
4. Partition both arrays at the current mid point and compare the elements at the partition points.
5. Adjust the low and high pointers based on the comparison to narrow down the search range.
6. Repeat steps 4-5 until we find the correct partition that leads to the median.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array to simplify the logic
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of the combined array
    total_length = len(nums1) + len(nums2)
    
    # Initialize the low and high pointers for binary search
    low = 0
    high = len(nums1)
    
    while low <= high:
        # Calculate the partition point for nums1
        partition_num1 = (low + high) // 2
        
        # Calculate the partition point for nums2 based on the total length
        partition_num2 = (total_length + 1) // 2 - partition_num1
        
        # Calculate the max and min values at the partition points
        max_left_num1 = float('-inf') if partition_num1 == 0 else nums1[partition_num1 - 1]
        min_right_num1 = float('inf') if partition_num1 == len(nums1) else nums1[partition_num1]
        
        max_left_num2 = float('-inf') if partition_num2 == 0 else nums2[partition_num2 - 1]
        min_right_num2 = float('inf') if partition_num2 == len(nums2) else nums2[partition_num2]
        
        # Check if we've found the correct partition
        if max_left_num1 <= min_right_num2 and max_left_num2 <= min_right_num1:
            # Calculate the median based on the total length
            if total_length % 2 == 0:
                return (max(max_left_num1, max_left_num2) + min(min_right_num1, min_right_num2)) / 2
            else:
                return max(max_left_num1, max_left_num2)
        # Adjust the pointers based on the comparison
        elif max_left_num1 > min_right_num2:
            high = partition_num1 - 1
        else:
            low = partition_num1 + 1
```

## Complexity
- Time: O(log(min(n, m))) — The time complexity is logarithmic because we're performing a binary search on the smaller array. The search space is reduced by half at each step, resulting in a logarithmic time complexity.
- Space: O(1) — The space complexity is constant because we're only using a fixed amount of space to store the partition points, max and min values, and other variables, regardless of the input size.

## Key Insight
The core trick to solving this problem is to use binary search to find the correct partition of the two sorted arrays, allowing us to calculate the median in logarithmic time without merging or sorting the entire array.