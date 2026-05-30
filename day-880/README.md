## Problem
Given two sorted arrays, find the median of the combined array without merging the arrays. The time complexity of the solution should be O(log(min(n, m))), where n and m are the lengths of the two arrays.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`
  Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  Output: `2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  Output: `0.0`

## Approach
To find the median of two sorted arrays without merging them, we can use a binary search approach. The idea is to find the partition point for both arrays such that the elements on the left side of the partition point in both arrays are less than or equal to the elements on the right side. This is similar to how we would find the median in a single sorted array. 

Here are the steps to solve this problem:
1. Ensure that `nums1` is the smaller array to simplify the logic.
2. Calculate the total length of both arrays combined.
3. Determine if the total length is odd or even to decide how to calculate the median.
4. Initialize the low and high pointers for the binary search.
5. Perform the binary search, adjusting the partition point based on the comparison of elements from both arrays.
6. Once the correct partition point is found, calculate the median based on the total length being odd or even.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure nums1 is the smaller array
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

        # Calculate the max and min values for the partition points
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
- Time: O(log(min(n, m))) — The time complexity is O(log(min(n, m))) because we are performing a binary search on the smaller array. The while loop runs until we find the correct partition point, and in the worst case, it takes log(min(n, m)) iterations.
- Space: O(1) — The space complexity is O(1) because we only use a constant amount of space to store the partition points, max and min values, and other variables.

## Key Insight
The core trick to solving this problem is to use binary search to find the partition point that divides the combined array into two halves with the elements on the left side being less than or equal to the elements on the right side.