## Problem
Given two sorted arrays, find the median of the combined array formed by merging the two arrays. The median is the middle value in the sorted combined array. If the total number of elements is even, the median is the average of the two middle values.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`, Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`, Output: `2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`, Output: `0.0`

## Approach
To solve this problem, we can use a binary search approach. The idea is to partition both arrays such that the elements on the left side of the partition in both arrays are less than or equal to the elements on the right side. We can achieve this by finding the correct partition point for both arrays. 
Here are the steps:
1. Calculate the total length of both arrays and determine if the total length is odd or even.
2. Determine the target partition point, which is half of the total length.
3. Initialize the low and high pointers for the partition point of the first array.
4. Calculate the corresponding partition point for the second array based on the partition point of the first array.
5. Compare the elements at the partition points and adjust the pointers accordingly.
6. Repeat steps 4-5 until the correct partition point is found.
7. Calculate the median based on the elements at the partition points.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1

    # Calculate the total length
    total_length = len(nums1) + len(nums2)

    # Initialize the low and high pointers
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
                return (max(max_left_nums1, max_left_nums2) + min(min_right_nums1, min_right_nums2)) / 2
            else:
                return max(max_left_nums1, max_left_nums2)
        # Adjust the pointers
        elif max_left_nums1 > min_right_nums2:
            high = partition_nums1 - 1
        else:
            low = partition_nums1 + 1
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is logarithmic because we are using a binary search approach to find the correct partition point. The search space is reduced by half at each step, resulting in a logarithmic time complexity. The minimum of `m` and `n` is used because we ensure that `nums1` is the smaller array.
- Space: O(1) — The space complexity is constant because we only use a constant amount of space to store the pointers and the values at the partition points.

## Key Insight
The core trick to solve this problem is to use a binary search approach to find the correct partition point for both arrays, ensuring that the elements on the left side of the partition are less than or equal to the elements on the right side.