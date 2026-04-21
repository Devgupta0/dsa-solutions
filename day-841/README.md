## Problem
Given two sorted arrays, find the median of the combined array when the two sorted arrays are merged. The overall size of the merged array can be either odd or even. When the size is odd, the median is the middle element, and when the size is even, the median is the average of the two middle elements.

## Examples
- Example 1: Input: `nums1 = [1, 3]`, `nums2 = [2]`, Output: `2.0`. Explanation: Merged array is `[1, 2, 3]`, and the median is `2.0`.
- Example 2: Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`, Output: `(2 + 3) / 2 = 2.5`. Explanation: Merged array is `[1, 2, 3, 4]`, and the median is the average of `2` and `3`, which is `2.5`.
- Example 3: Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`, Output: `0.0`. Explanation: Merged array is `[0, 0, 0, 0]`, and the median is the average of `0` and `0`, which is `0.0`.

## Approach
The problem can be solved using binary search. The idea is to partition both arrays into two halves each and compare the elements at the partition points. If the elements at the partition points are in the correct order, then the median can be calculated. Otherwise, we need to adjust the partition points.

Here are the steps:
1. Calculate the total length of the merged array.
2. Determine if the total length is odd or even to decide how to calculate the median.
3. Initialize the low and high pointers for the binary search.
4. Loop until the correct partition is found.
5. Inside the loop, calculate the partition points for both arrays.
6. Compare the elements at the partition points and adjust the partition points if necessary.
7. If the correct partition is found, calculate the median.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array to simplify the logic
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1

    # Calculate the total length of the merged array
    total_length = len(nums1) + len(nums2)

    # Initialize the low and high pointers for the binary search
    low = 0
    high = len(nums1)

    while low <= high:
        # Calculate the partition point for nums1
        partition1 = (low + high) // 2

        # Calculate the partition point for nums2
        partition2 = (total_length + 1) // 2 - partition1

        # Calculate the elements at the partition points
        max_left_x = float('-inf') if partition1 == 0 else nums1[partition1 - 1]
        min_right_x = float('inf') if partition1 == len(nums1) else nums1[partition1]

        max_left_y = float('-inf') if partition2 == 0 else nums2[partition2 - 1]
        min_right_y = float('inf') if partition2 == len(nums2) else nums2[partition2]

        # Check if the partition is correct
        if max_left_x <= min_right_y and max_left_y <= min_right_x:
            # If the total length is odd, return the max of the left elements
            if total_length % 2 == 1:
                return max(max_left_x, max_left_y)
            # If the total length is even, return the average of the max of the left elements and the min of the right elements
            else:
                return (max(max_left_x, max_left_y) + min(min_right_x, min_right_y)) / 2
        # If the partition is not correct, adjust the partition points
        elif max_left_x > min_right_y:
            high = partition1 - 1
        else:
            low = partition1 + 1
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is logarithmic because we are using binary search to find the correct partition. The search space is reduced by half at each step, and the number of steps is proportional to the logarithm of the size of the smaller array.
- Space: O(1) — The space complexity is constant because we are only using a constant amount of space to store the partition points and the elements at the partition points.

## Key Insight
The core trick to solve this problem is to use binary search to find the correct partition of the two sorted arrays, allowing us to find the median in logarithmic time complexity.