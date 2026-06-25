## Problem
Given two sorted arrays, find the median of the combined array without merging them, considering the overall length of the combined array could be odd or even. This problem requires an efficient solution that can handle large inputs and does not rely on merging the arrays, which would increase the space complexity.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`  
  Output: `2.0`  
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`  
  Output: `2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`  
  Output: `0.0`

## Approach
To solve this problem, we will use a binary search approach. The idea is to partition both arrays into two parts each, such that the elements on the left side of the partition in both arrays are less than the elements on the right side. We will then use binary search to find the correct partition that gives us the median.

Here's a step-by-step explanation:
1. Calculate the total length of the combined array.
2. Determine if the total length is odd or even to decide whether the median will be a single middle element or an average of two middle elements.
3. Initialize the binary search range based on the lengths of the input arrays.
4. Perform binary search to find the correct partition for both arrays that divides the combined array into two halves.
5. Compare elements at the partition boundaries to determine if the partition is correct or if we need to adjust the binary search range.
6. Once the correct partition is found, calculate the median based on whether the total length is odd or even.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array to simplify the logic
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1

    # Calculate the total length of the combined array
    total_length = len(nums1) + len(nums2)

    # Determine if the total length is odd or even
    is_odd = total_length % 2 == 1

    # Initialize the binary search range
    left, right = 0, len(nums1)

    while left <= right:
        # Partition for nums1
        partition_nums1 = (left + right) // 2

        # Partition for nums2 based on the total length and the partition for nums1
        partition_nums2 = (total_length + 1) // 2 - partition_nums1

        # Calculate the values at the partition boundaries
        max_left_nums1 = float('-inf') if partition_nums1 == 0 else nums1[partition_nums1 - 1]
        min_right_nums1 = float('inf') if partition_nums1 == len(nums1) else nums1[partition_nums1]

        max_left_nums2 = float('-inf') if partition_nums2 == 0 else nums2[partition_nums2 - 1]
        min_right_nums2 = float('inf') if partition_nums2 == len(nums2) else nums2[partition_nums2]

        # Check if the partition is correct
        if max_left_nums1 <= min_right_nums2 and max_left_nums2 <= min_right_nums1:
            # Calculate the median
            if is_odd:
                median = max(max_left_nums1, max_left_nums2)
            else:
                median = (max(max_left_nums1, max_left_nums2) + min(min_right_nums1, min_right_nums2)) / 2
            return median
        # Adjust the binary search range
        elif max_left_nums1 > min_right_nums2:
            right = partition_nums1 - 1
        else:
            left = partition_nums1 + 1

# Example usage
print(findMedianSortedArrays([1, 3], [2]))  # Output: 2.0
print(findMedianSortedArrays([1, 2], [3, 4]))  # Output: 2.5
print(findMedianSortedArrays([0, 0], [0, 0]))  # Output: 0.0
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is logarithmic because we perform a binary search on the smaller array. The `min(m, n)` part comes from ensuring that `nums1` is the smaller array to simplify the logic and reduce the search space.
- Space: O(1) — The space complexity is constant because we only use a fixed amount of space to store the partition boundaries and the median, regardless of the input size.

## Key Insight
The core trick to solving this problem efficiently is using binary search to find the correct partition of both arrays that gives us the median, allowing us to avoid merging the arrays and reducing the time complexity significantly.