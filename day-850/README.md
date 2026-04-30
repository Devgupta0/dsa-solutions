## Problem
Given two sorted arrays, find the median of the combined array. The overall size of the combined array can be very large, so an efficient solution is required. The median is the middle value in the sorted array. If the total number of elements is even, the median is the average of the two middle values.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`
  Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  Output: `(2 + 3) / 2 = 2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  Output: `0.0`

## Approach
To solve this problem efficiently, we will use a binary search approach. The idea is to partition both arrays into two halves each, such that the elements in the left halves are less than or equal to the elements in the right halves. We will then use binary search to find the correct partition.

Here are the steps:
1. Calculate the total length of the combined array.
2. Determine if the total length is odd or even.
3. Initialize the low and high pointers for binary search.
4. Partition both arrays based on the current mid value.
5. Compare the elements at the partition boundaries and adjust the pointers accordingly.
6. Repeat the process until the correct partition is found.
7. Calculate the median based on the partition.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1

    # Calculate the total length of the combined array
    total_length = len(nums1) + len(nums2)

    # Initialize the low and high pointers for binary search
    low = 0
    high = len(nums1)

    while low <= high:
        # Partition nums1
        partition_num1 = (low + high) // 2
        # Partition nums2
        partition_num2 = (total_length + 1) // 2 - partition_num1

        # Calculate the values at the partition boundaries
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
        # Adjust the pointers
        elif max_left_num1 > min_right_num2:
            high = partition_num1 - 1
        else:
            low = partition_num1 + 1
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is logarithmic because we are using binary search to find the correct partition. The search space is reduced by half at each step, resulting in a logarithmic time complexity. The `min(m, n)` term represents the smaller array, which determines the number of iterations required for the binary search.
- Space: O(1) — The space complexity is constant because we are only using a fixed amount of space to store the pointers and variables, regardless of the input size.

## Key Insight
The core trick to solving this problem is to use binary search to find the correct partition of the two arrays, ensuring that the elements in the left halves are less than or equal to the elements in the right halves, allowing for efficient calculation of the median.