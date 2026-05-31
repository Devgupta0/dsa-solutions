## Problem
Given two sorted arrays, find the median of the combined array without merging or sorting the entire array. The goal is to achieve this efficiently, considering the arrays are already sorted, and we should utilize this property to minimize computational overhead.

## Examples
- Example 1: 
  - Input: `nums1 = [1, 3]`, `nums2 = [2]`
  - Output: `2.0`
- Example 2: 
  - Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  - Output: `(2 + 3) / 2 = 2.5`
- Example 3: 
  - Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  - Output: `0.0`

## Approach
To find the median of two sorted arrays without merging them, we can use a binary search approach. The idea is to partition both arrays into two parts each, such that the elements on the left side of the partition in both arrays are less than the elements on the right side. This partitioning should be done in a way that the total number of elements on the left side of the partition in both arrays is equal to the total number of elements on the right side if the total number of elements is odd, or one more if the total number of elements is even (since we are looking for the median). 
Here are the steps to achieve this:
1. Ensure that `nums1` is the smaller array to simplify the code and reduce the number of edge cases.
2. Calculate the total length of both arrays combined.
3. Determine if the total length is odd or even to decide how to calculate the median.
4. Perform a binary search on `nums1` to find the correct partition point that satisfies the condition mentioned above.
5. For each partition point in `nums1`, calculate the corresponding partition point in `nums2` based on the total number of elements that should be on the left side of the partition.
6. Check if the partition is correct by comparing the last element on the left side of `nums1` and `nums2` with the first element on the right side of `nums1` and `nums2`. If the partition is not correct, adjust the binary search range accordingly.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1

    # Calculate the total length
    total_length = len(nums1) + len(nums2)

    # Initialize the binary search range
    left, right = 0, len(nums1)

    while left <= right:
        # Partition point for nums1
        partition_nums1 = (left + right) // 2

        # Calculate the partition point for nums2
        partition_nums2 = (total_length + 1) // 2 - partition_nums1

        # Calculate the max element on the left side and min element on the right side for both arrays
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
        # Adjust the binary search range
        elif max_left_nums1 > min_right_nums2:
            right = partition_nums1 - 1
        else:
            left = partition_nums1 + 1
```

## Complexity
- Time: O(log(min(n, m))) — The time complexity is logarithmic in the size of the smaller array because we perform a binary search on the smaller array to find the correct partition point.
- Space: O(1) — The space complexity is constant because we only use a constant amount of space to store the partition points and the max and min elements on both sides of the partition.

## Key Insight
The core trick to solving this problem is to utilize a binary search approach on the smaller array to find the correct partition point that allows us to calculate the median of the combined array without actually merging or sorting the entire array.