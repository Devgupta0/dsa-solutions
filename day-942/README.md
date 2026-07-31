## Problem
Given two sorted arrays, find the median of the merged array without actually merging them. The goal is to achieve this in an efficient manner, considering the overall time complexity. The input arrays are sorted in ascending order, and the total number of elements in both arrays is assumed to be even or odd.

## Examples
* Example 1:
  Input: `nums1 = [1, 3]`, `nums2 = [2]`
  Output: `2.0`
* Example 2:
  Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  Output: `(2 + 3) / 2 = 2.5`
* Example 3:
  Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  Output: `0.0`

## Approach
To solve this problem efficiently, we can utilize a binary search approach. The idea is to find the partition point for both arrays such that the elements on the left side of the partition point in both arrays are less than the elements on the right side. Since the arrays are sorted, we can achieve this by ensuring that the maximum element on the left side of the partition point in both arrays is less than or equal to the minimum element on the right side.

Here are the steps to achieve this:
1. Calculate the total length of both arrays.
2. Determine if the total length is even or odd to decide whether the median will be a single middle element or the average of two middle elements.
3. Initialize the binary search range to be from 0 to the length of the shorter array.
4. Perform binary search to find the partition point.
5. At each iteration of the binary search, calculate the partition points for both arrays based on the current search range.
6. Compare the maximum element on the left side of the partition point in both arrays with the minimum element on the right side.
7. Adjust the binary search range based on the comparison result.
8. Once the correct partition point is found, calculate the median.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the shorter array to simplify the logic
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1

    # Calculate the total length
    total_length = len(nums1) + len(nums2)

    # Initialize the binary search range
    left, right = 0, len(nums1)

    while left <= right:
        # Calculate the partition point for nums1
        partition_nums1 = (left + right) // 2
        
        # Calculate the partition point for nums2 based on the total length and the partition point of nums1
        partition_nums2 = (total_length + 1) // 2 - partition_nums1

        # Calculate the maximum element on the left side of the partition point in nums1
        max_left_nums1 = float('-inf') if partition_nums1 == 0 else nums1[partition_nums1 - 1]
        
        # Calculate the minimum element on the right side of the partition point in nums1
        min_right_nums1 = float('inf') if partition_nums1 == len(nums1) else nums1[partition_nums1]

        # Calculate the maximum element on the left side of the partition point in nums2
        max_left_nums2 = float('-inf') if partition_nums2 == 0 else nums2[partition_nums2 - 1]
        
        # Calculate the minimum element on the right side of the partition point in nums2
        min_right_nums2 = float('inf') if partition_nums2 == len(nums2) else nums2[partition_nums2]

        # Check if the partition is correct
        if max_left_nums1 <= min_right_nums2 and max_left_nums2 <= min_right_nums1:
            # If the total length is even, the median is the average of the two middle elements
            if total_length % 2 == 0:
                return (max(max_left_nums1, max_left_nums2) + min(min_right_nums1, min_right_nums2)) / 2
            # If the total length is odd, the median is the middle element
            else:
                return max(max_left_nums1, max_left_nums2)
        # If the partition is not correct, adjust the binary search range
        elif max_left_nums1 > min_right_nums2:
            right = partition_nums1 - 1
        else:
            left = partition_nums1 + 1
```

## Complexity
- Time: O(log(min(n, m))) — where n and m are the lengths of the input arrays. This is because we are using binary search to find the partition point, and the search range is limited to the length of the shorter array.
- Space: O(1) — the space complexity is constant because we are only using a fixed amount of space to store the partition points and the maximum and minimum elements on the left and right sides of the partition points.

## Key Insight
The core trick to solve this problem is to use binary search to find the partition point for both arrays such that the elements on the left side of the partition point in both arrays are less than the elements on the right side, allowing us to calculate the median without actually merging the arrays.