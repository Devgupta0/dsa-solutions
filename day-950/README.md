## Problem
Given two sorted arrays of different lengths, merge and find the median of the resulting array without sorting the entire merged array. The goal is to achieve this efficiently, considering the arrays are already sorted, which provides an opportunity to simplify the merging and median calculation process.

## Examples
- Example 1: 
  - Input: `nums1 = [1, 3]`, `nums2 = [2]`
  - Output: `2.0`
  - Explanation: Merged array is `[1, 2, 3]`, and the median is `2`.
- Example 2: 
  - Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  - Output: `(2 + 3) / 2 = 2.5`
  - Explanation: Merged array is `[1, 2, 3, 4]`, and the median is the average of `2` and `3`, which is `2.5`.
- Example 3: 
  - Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  - Output: `0.0`
  - Explanation: Merged array is `[0, 0, 0, 0]`, and the median is `0`.

## Approach
The key to solving this problem efficiently is to use a binary search approach. Since the input arrays are sorted, we can find the median without fully merging the arrays. The idea is to partition both arrays such that the elements on the left side of the partition in both arrays are less than the elements on the right side. This partitioning is done in a way that the total number of elements on the left side of the partition in both arrays is equal to the half of the total length of both arrays. If the total length of both arrays is odd, the median will be the maximum element on the left side of the partition. If the total length is even, the median will be the average of the maximum element on the left side and the minimum element on the right side.

Step by step:
1. Calculate the total length of both arrays.
2. Determine if the total length is odd or even to decide how to calculate the median.
3. Initialize the low and high pointers for the binary search.
4. Perform the binary search to find the correct partition.
5. Calculate the median based on the partition.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array to simplify the logic
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length
    total_length = len(nums1) + len(nums2)
    
    # If the total length is odd, the median is the max of the left half
    # If the total length is even, the median is the average of the max of the left half and the min of the right half
    half_length = total_length // 2
    
    # Initialize the binary search range
    left, right = 0, len(nums1) - 1
    
    while True:
        # Calculate the partition point for nums1
        i = (left + right) // 2
        
        # Calculate the partition point for nums2 based on the partition point for nums1
        j = half_length - i - 2
        
        # Calculate the values at the partition points
        nums1_left = nums1[i] if i >= 0 else float("-infinity")
        nums1_right = nums1[i + 1] if (i + 1) < len(nums1) else float("infinity")
        nums2_left = nums2[j] if j >= 0 else float("-infinity")
        nums2_right = nums2[j + 1] if (j + 1) < len(nums2) else float("infinity")
        
        # Check if the partition is correct
        if nums1_left <= nums2_right and nums2_left <= nums1_right:
            # If the total length is odd, return the max of the left half
            if total_length % 2:
                return max(nums1_left, nums2_left)
            # If the total length is even, return the average of the max of the left half and the min of the right half
            else:
                return (max(nums1_left, nums2_left) + min(nums1_right, nums2_right)) / 2
        # If the partition is not correct, adjust the binary search range
        elif nums1_left > nums2_right:
            right = i - 1
        else:
            left = i + 1
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is logarithmic because we are performing a binary search on the smaller of the two input arrays. The number of steps in the binary search is proportional to the logarithm of the size of the smaller array.
- Space: O(1) — The space complexity is constant because we only use a fixed amount of space to store the variables used in the algorithm, and we do not use any data structures that grow with the size of the input.

## Key Insight
The core trick to solving this problem is to use a binary search approach to find the correct partition of the two arrays, allowing us to calculate the median without fully merging the arrays.