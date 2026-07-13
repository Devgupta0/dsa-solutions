## Problem
Given two sorted arrays, the task is to find the median of the combined array. The combined array's length could be either odd or even, requiring a solution that handles both cases. The input arrays are already sorted, but their lengths can be different.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`
  Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  Output: `(2 + 3) / 2 = 2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  Output: `0.0`

## Approach
To solve this problem, we can utilize a binary search approach. The idea is to partition both arrays into two halves each and compare the elements at the partition points. If the elements are in the correct order (i.e., all elements in the left halves are less than or equal to all elements in the right halves), we can calculate the median based on the partition points. If not, we adjust the partition points and repeat the process until we find the correct partition.
Here are the steps:
1. Determine the total length of the combined array and calculate the target partition point (the point at which the left half would be the same size as the right half if the total length is odd, or one element more in the left half if the total length is even).
2. Initialize the low and high pointers for the binary search.
3. Partition both arrays based on the current mid value.
4. Compare the elements at the partition points. If they are in the correct order, calculate the median.
5. If the elements are not in the correct order, adjust the partition points and repeat the process.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array to simplify the logic
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of the combined array
    total_length = len(nums1) + len(nums2)
    
    # Initialize the low and high pointers for the binary search
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
                median = (max(max_left_nums1, max_left_nums2) + min(min_right_nums1, min_right_nums2)) / 2
            else:
                median = max(max_left_nums1, max_left_nums2)
            return median
        # Adjust the partition points if the partition is not correct
        elif max_left_nums1 > min_right_nums2:
            high = partition_nums1 - 1
        else:
            low = partition_nums1 + 1
```

## Complexity
- Time: O(log(min(n, m))) — The time complexity is logarithmic in the length of the smaller array because we are performing a binary search on the smaller array.
- Space: O(1) — The space complexity is constant because we are only using a constant amount of space to store the partition points and the values at the partition points.

## Key Insight
The core trick to solving this problem is to use a binary search approach to find the correct partition point for the smaller array, allowing us to efficiently find the median of the combined array.