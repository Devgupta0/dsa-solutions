## Problem
Given two sorted arrays, find the median of the combined array formed by merging the two arrays. The median is the middle value in the sorted array. If the total number of elements is even, the median is the average of the two middle values.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`
  Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  Output: `2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  Output: `0.0`

## Approach
To find the median of the combined array, we can use a binary search approach. The idea is to partition both arrays such that the elements on the left side of the partition in both arrays are less than or equal to the elements on the right side. We can then use this partition to find the median.

Here's a step-by-step explanation:
1. Calculate the total length of the combined array.
2. Determine if the total length is odd or even to decide how to calculate the median.
3. Initialize two pointers, one for each array, to the start of each array.
4. Use binary search to find the partition point for the first array.
5. Use the partition point to find the corresponding partition point in the second array.
6. Compare the elements at the partition points and adjust the partition points as needed.
7. Once the partition points are correct, calculate the median.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Make sure that nums1 is the smaller array to simplify the logic
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1

    # Calculate the total length of the combined array
    total_length = len(nums1) + len(nums2)

    # Initialize the low and high pointers for binary search
    low = 0
    high = len(nums1)

    while low <= high:
        # Calculate the partition point for nums1
        partition_nums1 = (low + high) // 2

        # Calculate the corresponding partition point for nums2
        partition_nums2 = (total_length + 1) // 2 - partition_nums1

        # Calculate the values at the partition points
        max_left_nums1 = float('-inf') if partition_nums1 == 0 else nums1[partition_nums1 - 1]
        min_right_nums1 = float('inf') if partition_nums1 == len(nums1) else nums1[partition_nums1]

        max_left_nums2 = float('-inf') if partition_nums2 == 0 else nums2[partition_nums2 - 1]
        min_right_nums2 = float('inf') if partition_nums2 == len(nums2) else nums2[partition_nums2]

        # Check if the partition points are correct
        if max_left_nums1 <= min_right_nums2 and max_left_nums2 <= min_right_nums1:
            # Calculate the median
            if total_length % 2 == 0:
                return (max(max_left_nums1, max_left_nums2) + min(min_right_nums1, min_right_nums2)) / 2
            else:
                return max(max_left_nums1, max_left_nums2)
        # Adjust the partition points
        elif max_left_nums1 > min_right_nums2:
            high = partition_nums1 - 1
        else:
            low = partition_nums1 + 1
```

## Complexity
- Time: O(log(min(m, n))) — This is because we are using binary search to find the partition point, where m and n are the lengths of the two arrays. The binary search is performed on the smaller array, so the time complexity is logarithmic in the length of the smaller array.
- Space: O(1) — This is because we only use a constant amount of space to store the partition points and the values at the partition points.

## Key Insight
The core trick to solving this problem is to use binary search to find the partition point in the smaller array, which allows us to find the median in logarithmic time.