## Problem
Given two sorted arrays, find the median of the combined array. The combined array's length can be either odd or even. If the length is odd, the median is the middle element. If the length is even, the median is the average of the two middle elements.

## Examples
- Example 1: 
  - Input: `nums1 = [1, 3]`, `nums2 = [2]`
  - Output: `2.0`
- Example 2: 
  - Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  - Output: `2.5`
- Example 3: 
  - Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  - Output: `0.0`

## Approach
To find the median of two sorted arrays, we can use a binary search approach. The idea is to partition both arrays such that elements on the left side of the partition in both arrays are less than or equal to elements on the right side. We then adjust the partition based on whether the total length of the left side is less than, equal to, or greater than half the total length of the combined array.
Here are the steps:
1. Calculate the total length of the combined array.
2. Determine the target length for the left side (half of the total length).
3. Initialize the low and high pointers for the binary search.
4. Perform the binary search, adjusting the partition as needed.
5. Calculate the median based on whether the total length is odd or even.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Make sure nums1 is the smaller array to simplify the logic
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of the combined array
    total_length = len(nums1) + len(nums2)
    
    # Determine the target length for the left side
    left_length = (total_length + 1) // 2
    
    # Initialize the low and high pointers for the binary search
    low = 0
    high = len(nums1)
    
    while low <= high:
        # Calculate the partition for nums1
        partition1 = (low + high) // 2
        
        # Calculate the partition for nums2 based on the partition for nums1
        partition2 = left_length - partition1
        
        # Calculate the values at the partitions
        max_left_nums1 = float('-inf') if partition1 == 0 else nums1[partition1 - 1]
        min_right_nums1 = float('inf') if partition1 == len(nums1) else nums1[partition1]
        
        max_left_nums2 = float('-inf') if partition2 == 0 else nums2[partition2 - 1]
        min_right_nums2 = float('inf') if partition2 == len(nums2) else nums2[partition2]
        
        # Check if the partition is correct
        if max_left_nums1 <= min_right_nums2 and max_left_nums2 <= min_right_nums1:
            # Calculate the median
            if total_length % 2 == 0:
                median = (max(max_left_nums1, max_left_nums2) + min(min_right_nums1, min_right_nums2)) / 2
            else:
                median = max(max_left_nums1, max_left_nums2)
            return median
        # Adjust the partition
        elif max_left_nums1 > min_right_nums2:
            high = partition1 - 1
        else:
            low = partition1 + 1
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is logarithmic with respect to the smaller array because we are performing a binary search on the smaller array.
- Space: O(1) — The space complexity is constant because we are only using a fixed amount of space to store the variables.

## Key Insight
The core trick to solving this problem is to use a binary search approach to find the correct partition of the two arrays, allowing us to find the median in logarithmic time.