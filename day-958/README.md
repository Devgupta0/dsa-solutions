## Problem
Given two sorted arrays, find the median of the combined array without merging them. The input arrays are sorted in ascending order, and the goal is to calculate the median of the combined array, which would be the middle value if the arrays were merged. If the total length of the combined array is even, the median is the average of the two middle values.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`
  Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  Output: `(2 + 3) / 2 = 2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  Output: `0`

## Approach
To find the median of two sorted arrays without merging them, we can utilize a binary search approach. The idea is to partition both arrays such that the elements on the left side of the partition in both arrays are less than the elements on the right side. This partition should be such that the total number of elements on the left side of the partition in both arrays is equal to half the total number of elements in both arrays. 
Here are the step-by-step details:
1. Calculate the total length of both arrays combined.
2. Determine if the total length is odd or even to decide whether the median will be a single middle value or an average of two middle values.
3. Initialize the low and high pointers for binary search, considering the smaller array to simplify the logic.
4. Perform binary search to find the correct partition point that satisfies the condition for the median calculation.
5. Based on the partition, calculate the values of the elements at the partition points and use them to find the median.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure nums1 is the smaller array to simplify the logic
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1

    # Calculate the total length
    total_length = len(nums1) + len(nums2)
    
    # Initialize the low and high pointers for binary search
    low, high = 0, len(nums1)
    
    while low <= high:
        # Partition point for nums1
        partition_nums1 = (low + high) // 2
        
        # Partition point for nums2 based on the total length and the partition of nums1
        partition_nums2 = (total_length + 1) // 2 - partition_nums1
        
        # Calculate the values of the elements at the partition points
        max_left_nums1 = float('-inf') if partition_nums1 == 0 else nums1[partition_nums1 - 1]
        min_right_nums1 = float('inf') if partition_nums1 == len(nums1) else nums1[partition_nums1]
        
        max_left_nums2 = float('-inf') if partition_nums2 == 0 else nums2[partition_nums2 - 1]
        min_right_nums2 = float('inf') if partition_nums2 == len(nums2) else nums2[partition_nums2]
        
        # Check if the partition is correct
        if max_left_nums1 <= min_right_nums2 and max_left_nums2 <= min_right_nums1:
            # If the total length is odd, return the max of the left elements
            if total_length % 2:
                return max(max_left_nums1, max_left_nums2)
            # If the total length is even, return the average of the max of the left elements and the min of the right elements
            else:
                return (max(max_left_nums1, max_left_nums2) + min(min_right_nums1, min_right_nums2)) / 2
        # If the partition is not correct, adjust the pointers and continue the binary search
        elif max_left_nums1 > min_right_nums2:
            high = partition_nums1 - 1
        else:
            low = partition_nums1 + 1
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is logarithmic with respect to the size of the smaller array because we are performing a binary search on the smaller array to find the correct partition.
- Space: O(1) — The space complexity is constant because we are not using any additional data structures that scale with the input size.

## Key Insight
The core trick to solving this problem efficiently is to use binary search to find the correct partition points in the two sorted arrays, ensuring that the elements on the left side of the partition are less than the elements on the right side, which directly leads to the calculation of the median.