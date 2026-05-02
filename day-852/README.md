## Problem
Given two sorted arrays, find the median of the combined array. The median is the middle value in the sorted array. If the total number of elements is odd, the median is the middle element. If the total number of elements is even, the median is the average of the two middle elements.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`
  Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  Output: `2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  Output: `0.0`

## Approach
To find the median of the combined array, we can use a binary search approach. The idea is to partition both arrays such that the elements on the left side of the partition in both arrays are less than or equal to the elements on the right side. We can then use this partition to find the median.

Here's a step-by-step explanation:
1. Calculate the total length of both arrays.
2. If the total length is odd, the median will be the middle element. If it's even, the median will be the average of the two middle elements.
3. Initialize two pointers, one for each array, to the start of each array.
4. Use binary search to find the partition point for the first array.
5. Based on the partition point, calculate the corresponding partition point for the second array.
6. Check if the partition is correct by comparing the elements at the partition points.
7. If the partition is not correct, adjust the partition point and repeat steps 4-6.
8. Once the correct partition is found, calculate the median.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Make sure nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length
    total_length = len(nums1) + len(nums2)
    
    # Initialize the low and high pointers
    low = 0
    high = len(nums1)
    
    while low <= high:
        # Partition point for nums1
        partition_nums1 = (low + high) // 2
        
        # Partition point for nums2
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
        # Adjust the partition point
        elif max_left_nums1 > min_right_nums2:
            high = partition_nums1 - 1
        else:
            low = partition_nums1 + 1
```

## Complexity
- Time: O(log(min(n, m))) — The time complexity is O(log(min(n, m))) because we are using binary search to find the partition point. The search space is reduced by half at each step, resulting in a logarithmic time complexity.
- Space: O(1) — The space complexity is O(1) because we are only using a constant amount of space to store the pointers and variables.

## Key Insight
The core trick to solving this problem is to use binary search to find the partition point for the smaller array, which allows us to efficiently find the median of the combined array.