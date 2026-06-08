## Problem
Given two sorted arrays, the goal is to find the median of the merged array. The merged array can have an odd or even number of elements. If the merged array has an odd number of elements, the median is the middle value. If it has an even number of elements, the median is the average of the two middle values.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`, Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`, Output: `(2 + 3) / 2 = 2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`, Output: `0.0`

## Approach
To solve this problem, we can use a binary search approach. The idea is to partition both arrays such that the elements on the left side of the partition in both arrays are smaller than the elements on the right side. We can then find the median based on the partition. 
Here are the steps:
1. Calculate the total length of the merged array.
2. Determine if the total length is odd or even.
3. Initialize two pointers, one for each array, to track the current position in each array.
4. Use binary search to find the correct partition for the two arrays.
5. Once the correct partition is found, calculate the median based on whether the total length is odd or even.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Make sure that nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of the merged array
    total_length = len(nums1) + len(nums2)
    
    # Initialize the low and high pointers for binary search
    low = 0
    high = len(nums1)
    
    while low <= high:
        # Partition the first array
        partition1 = (low + high) // 2
        
        # Partition the second array based on the partition of the first array
        partition2 = (total_length + 1) // 2 - partition1
        
        # Calculate the values at the partitions
        max_left_num1 = float('-inf') if partition1 == 0 else nums1[partition1 - 1]
        min_right_num1 = float('inf') if partition1 == len(nums1) else nums1[partition1]
        
        max_left_num2 = float('-inf') if partition2 == 0 else nums2[partition2 - 1]
        min_right_num2 = float('inf') if partition2 == len(nums2) else nums2[partition2]
        
        # Check if the partitions are correct
        if max_left_num1 <= min_right_num2 and max_left_num2 <= min_right_num1:
            # If the total length is odd, return the max of the left side
            if total_length % 2 == 1:
                return max(max_left_num1, max_left_num2)
            # If the total length is even, return the average of the max of the left side and the min of the right side
            else:
                return (max(max_left_num1, max_left_num2) + min(min_right_num1, min_right_num2)) / 2
        # If the partition is not correct, adjust the pointers
        elif max_left_num1 > min_right_num2:
            high = partition1 - 1
        else:
            low = partition1 + 1
```

## Complexity
- Time: O(log(min(m, n))) — where m and n are the lengths of the input arrays. This is because we are using binary search to find the correct partition, and the number of steps in the binary search is proportional to the logarithm of the size of the smaller array.
- Space: O(1) — because we are only using a constant amount of space to store the pointers and the values at the partitions.

## Key Insight
The core trick to solving this problem is to use binary search to find the correct partition of the two arrays, which allows us to find the median in logarithmic time.