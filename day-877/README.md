## Problem
Given two sorted arrays, find the median of the combined array. The overall length of the combined array is large, and the arrays cannot fit into memory. This means we need to find a solution that doesn't require loading the entire arrays into memory.

## Examples
- Input: `nums1 = [1, 3]`, `nums2 = [2]`
  Output: `2.0`
- Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  Output: `(2 + 3) / 2 = 2.5`
- Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  Output: `0.0`

## Approach
To solve this problem without loading the entire arrays into memory, we can use a binary search approach. The idea is to partition the two arrays into two halves, such that the elements in the left halves are smaller than the elements in the right halves. We can then adjust the partition points based on the comparison of the elements at the partition points.

Here are the steps:
1. Calculate the total length of the combined array.
2. Determine the partition point for the first array based on the total length.
3. Calculate the corresponding partition point for the second array.
4. Compare the elements at the partition points and adjust the partition points accordingly.
5. Repeat steps 2-4 until the partition points are correct.
6. Calculate the median based on the elements at the partition points.

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
        # Calculate the partition point for the first array
        partition_x = (low + high) // 2

        # Calculate the corresponding partition point for the second array
        partition_y = (total_length + 1) // 2 - partition_x

        # Calculate the values at the partition points
        max_left_x = float('-inf') if partition_x == 0 else nums1[partition_x - 1]
        min_right_x = float('inf') if partition_x == len(nums1) else nums1[partition_x]

        max_left_y = float('-inf') if partition_y == 0 else nums2[partition_y - 1]
        min_right_y = float('inf') if partition_y == len(nums2) else nums2[partition_y]

        # Check if the partition points are correct
        if max_left_x <= min_right_y and max_left_y <= min_right_x:
            # Calculate the median
            if total_length % 2 == 0:
                return (max(max_left_x, max_left_y) + min(min_right_x, min_right_y)) / 2
            else:
                return max(max_left_x, max_left_y)
        # Adjust the partition points
        elif max_left_x > min_right_y:
            high = partition_x - 1
        else:
            low = partition_x + 1
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is logarithmic because we are using binary search to find the partition points. The search space is reduced by half at each step, resulting in a logarithmic time complexity. The `min(m, n)` term is used because we ensure that `nums1` is the smaller array, and the binary search is performed on `nums1`.
- Space: O(1) — The space complexity is constant because we only use a fixed amount of space to store the partition points and the values at the partition points.

## Key Insight
The core trick to solving this problem is to use binary search to find the correct partition points for the two arrays, allowing us to calculate the median without loading the entire arrays into memory.