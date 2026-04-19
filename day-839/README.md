## Problem
Given two sorted arrays, find the median of the merged array without actually merging them. The overall size of the merged array could be very large, making it inefficient to merge the arrays and then calculate the median. The goal is to find the median in a way that scales well for large input arrays.

## Examples
- Example 1: Input: `nums1 = [1, 3]`, `nums2 = [2]`, Output: `2.0`
- Example 2: Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`, Output: `2.5`
- Example 3: Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`, Output: `0.0`

## Approach
To solve this problem, we can use a binary search approach. The idea is to find the partition point for both arrays such that the elements on the left side of the partition in both arrays are less than the elements on the right side. This partition point can be used to calculate the median. 

Here are the steps:
1. Ensure that `nums1` is the smaller array to simplify the logic.
2. Calculate the total length of both arrays combined.
3. Determine the half length of the combined array, which will be used to find the partition point.
4. Perform a binary search on `nums1` to find the partition point.
5. For each potential partition point in `nums1`, calculate the corresponding partition point in `nums2`.
6. Check if the partition is correct by comparing the elements at the partition points.
7. If the partition is correct, calculate the median based on whether the total length is odd or even.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length and half length
    total_length = len(nums1) + len(nums2)
    half_length = total_length // 2
    
    # Initialize the binary search range
    left, right = 0, len(nums1) - 1
    
    while True:
        # Calculate the partition point for nums1
        i = (left + right) // 2
        
        # Calculate the partition point for nums2
        j = half_length - i - 2
        
        # Calculate the values at the partition points
        nums1_left = nums1[i] if i >= 0 else float("-infinity")
        nums1_right = nums1[i + 1] if (i + 1) < len(nums1) else float("infinity")
        nums2_left = nums2[j] if j >= 0 else float("-infinity")
        nums2_right = nums2[j + 1] if (j + 1) < len(nums2) else float("infinity")
        
        # Check if the partition is correct
        if nums1_left <= nums2_right and nums2_left <= nums1_right:
            # Calculate the median
            if total_length % 2:
                return min(nums1_right, nums2_right)
            return (max(nums1_left, nums2_left) + min(nums1_right, nums2_right)) / 2
        # Adjust the binary search range
        elif nums1_left > nums2_right:
            right = i - 1
        else:
            left = i + 1
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is logarithmic because we are performing a binary search on the smaller array. The search space is reduced by half at each step, resulting in a logarithmic time complexity.
- Space: O(1) — The space complexity is constant because we are not using any data structures that scale with the input size. We only use a constant amount of space to store the variables.

## Key Insight
The core trick to solving this problem is using binary search to find the partition point in the smaller array, allowing us to calculate the median of the merged array without actually merging it.