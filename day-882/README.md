## Problem
Given two sorted arrays, find the median of the merged array without actually merging them, with the constraint that the total number of elements in both arrays is odd. This means that the middle value of the combined array will be the median, and since the total number of elements is odd, this median will be a single number.

## Examples
- Example 1: Given `nums1 = [1, 3]` and `nums2 = [2]`, the median of the merged array `[1, 2, 3]` is `2`.
- Example 2: Given `nums1 = [1, 2]` and `nums2 = [3, 4]`, the median of the merged array `[1, 2, 3, 4]` is `2.5`, but since the problem specifies the total number of elements is odd, this example doesn't fit the problem's constraint directly. However, for educational purposes: if we had `nums1 = [1]` and `nums2 = [2, 3]`, the median of the merged array `[1, 2, 3]` would indeed be `2`, which is an odd total number of elements scenario.

## Approach
To find the median of two sorted arrays without merging them, we can use a binary search approach. The idea is to partition both arrays such that the elements on the left side of the partition in both arrays are less than the elements on the right side. Since the total number of elements is odd, the median will be the max of the left side of the partition. 

Here's the step-by-step approach:
1. Ensure that `nums1` is the smaller array. If not, swap `nums1` and `nums2`.
2. Calculate the total length of both arrays.
3. Determine the partition point for `nums1`. Since the total number of elements is odd, we are looking for a partition that makes the left side of `nums1` and `nums2` have one more element than the right side when combined.
4. Perform a binary search on `nums1` to find the correct partition point.
5. For each potential partition point in `nums1`, calculate the corresponding partition point in `nums2` based on the total length and the goal of having the left side elements be less than the right side elements.
6. Compare the elements at the partition points and adjust the binary search range accordingly.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate total length
    total_length = len(nums1) + len(nums2)
    
    # Since total length is odd, the median will be the max of the left side
    half_length = total_length // 2
    
    # Initialize binary search range
    left, right = 0, len(nums1) - 1
    
    while True:
        # Calculate partition point for nums1
        i = (left + right) // 2
        
        # Calculate partition point for nums2
        j = half_length - i - 2
        
        # Calculate elements at partition points
        nums1_left = nums1[i] if i >= 0 else float("-infinity")
        nums1_right = nums1[i + 1] if (i + 1) < len(nums1) else float("infinity")
        nums2_left = nums2[j] if j >= 0 else float("-infinity")
        nums2_right = nums2[j + 1] if (j + 1) < len(nums2) else float("infinity")
        
        # Check if partition is correct
        if nums1_left <= nums2_right and nums2_left <= nums1_right:
            # Return the max of the left side
            return max(nums1_left, nums2_left)
        elif nums1_left > nums2_right:
            # Move partition to the left
            right = i - 1
        else:
            # Move partition to the right
            left = i + 1
```

## Complexity
- Time: O(log(min(m, n))) — This is because we are performing a binary search on the smaller array. The time complexity depends on the size of the smaller array (`m` or `n`), where `m` and `n` are the lengths of `nums1` and `nums2`, respectively.
- Space: O(1) — The space complexity is constant because we are only using a fixed amount of space to store the partition points and the elements at those points, regardless of the input size.

## Key Insight
The core trick to solving this problem is using binary search to find the correct partition point in the smaller array that ensures the elements on the left side of the partition in both arrays are less than the elements on the right side, thus allowing us to find the median without actually merging the arrays.