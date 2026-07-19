## Problem
Given two sorted arrays, find the median of the combined array. The overall size of the combined array can be very large, so a solution that minimizes memory usage and has an efficient time complexity is required.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`
  Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  Output: `(2 + 3) / 2 = 2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  Output: `0.0`

## Approach
To find the median of the combined array, we can use a binary search approach. The idea is to find the partition point for both arrays such that the elements on the left side of the partition point in both arrays are less than or equal to the elements on the right side. 
Here are the steps:
1. Calculate the total length of the combined array.
2. Determine if the total length is odd or even to decide whether the median will be a single middle element or the average of two middle elements.
3. Perform a binary search to find the partition point. The binary search will be performed on the smaller array to minimize the number of comparisons.
4. Compare the elements at the partition points and adjust the partition as needed.
5. Once the correct partition is found, calculate the median based on whether the total length is odd or even.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array to minimize the number of comparisons
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

        # Calculate the partition point for nums2 based on the partition point for nums1
        partition_nums2 = (total_length + 1) // 2 - partition_nums1

        # Calculate the values at the partition points
        max_left_nums1 = float('-inf') if partition_nums1 == 0 else nums1[partition_nums1 - 1]
        min_right_nums1 = float('inf') if partition_nums1 == len(nums1) else nums1[partition_nums1]

        max_left_nums2 = float('-inf') if partition_nums2 == 0 else nums2[partition_nums2 - 1]
        min_right_nums2 = float('inf') if partition_nums2 == len(nums2) else nums2[partition_nums2]

        # Check if the partition is correct
        if max_left_nums1 <= min_right_nums2 and max_left_nums2 <= min_right_nums1:
            # Calculate the median based on whether the total length is odd or even
            if total_length % 2 == 0:
                return (max(max_left_nums1, max_left_nums2) + min(min_right_nums1, min_right_nums2)) / 2
            else:
                return max(max_left_nums1, max_left_nums2)
        # Adjust the partition if necessary
        elif max_left_nums1 > min_right_nums2:
            high = partition_nums1 - 1
        else:
            low = partition_nums1 + 1
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is logarithmic because we are performing a binary search on the smaller array. The number of comparisons is proportional to the logarithm of the size of the smaller array.
- Space: O(1) — The space complexity is constant because we are only using a constant amount of space to store the partition points and the values at the partition points.

## Key Insight
The core trick to solving this problem is to use a binary search approach to find the partition point for both arrays, which allows us to find the median in logarithmic time complexity.