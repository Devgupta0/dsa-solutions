## Problem
Given two sorted arrays, find the median of the combined array without merging them. The goal is to achieve this in a time complexity of O(log(min(m, n))), where m and n are the lengths of the two arrays.

## Examples
- Example 1: Input: `nums1 = [1, 3]`, `nums2 = [2]`, Output: `2.00000`
- Example 2: Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`, Output: `(2 + 3) / 2 = 2.5`

## Approach
To solve this problem, we will use a binary search approach. The idea is to find a partition point for both arrays such that elements on the left side of the partition point in both arrays are less than or equal to elements on the right side. This approach ensures that we can find the median without having to merge the two arrays.
Here are the steps:
1. Ensure that `nums1` is the smaller array to simplify the logic.
2. Calculate the total length of both arrays combined.
3. Determine the half length of the combined array, which will be used to find the median.
4. Perform a binary search on `nums1` to find the partition point that satisfies the condition for the median.
5. If the partition point is found, calculate the median based on whether the total length is odd or even.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of both arrays
    total_length = len(nums1) + len(nums2)
    
    # Determine the half length of the combined array
    half_length = total_length // 2
    
    # Initialize the binary search range
    left, right = 0, len(nums1) - 1
    
    while True:
        # Calculate the partition point for nums1
        i = (left + right) // 2
        
        # Calculate the corresponding partition point for nums2
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
        # Adjust the partition point
        elif nums1_left > nums2_right:
            right = i - 1
        else:
            left = i + 1
```

## Complexity
- Time: O(log(min(m, n))) — This is because we are performing a binary search on the smaller array, reducing the search space by half at each step.
- Space: O(1) — The space complexity is constant because we are only using a fixed amount of space to store the variables, regardless of the input size.

## Key Insight
The core trick to solving this problem is using binary search to find the partition point that divides the combined array into two halves, ensuring that elements on the left side are less than or equal to elements on the right side, allowing for the calculation of the median without merging the arrays.