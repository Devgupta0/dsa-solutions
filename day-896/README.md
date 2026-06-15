## Problem
Given two sorted arrays, find the median of the merged array without actually merging them. This problem requires considering the overall time complexity to achieve an efficient solution.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`
  Output: `2.00000`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  Output: `(2 + 3) / 2 = 2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  Output: `0.0`

## Approach
To solve this problem, we can utilize a binary search approach. The idea is to find a partition point for both arrays such that the elements on the left side of the partition point in both arrays are less than the elements on the right side. This partition point will help us find the median of the merged array.

Here's a step-by-step explanation:
1. Calculate the total length of both arrays.
2. Determine if the total length is odd or even, as this will affect how we calculate the median.
3. Initialize the low and high pointers for the binary search.
4. Perform a binary search to find the partition point.
5. Once the partition point is found, calculate the median based on whether the total length is odd or even.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array to simplify the logic
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of both arrays
    total_length = len(nums1) + len(nums2)
    
    # Initialize the low and high pointers for the binary search
    low = 0
    high = len(nums1)
    
    while low <= high:
        # Calculate the partition point for nums1
        partition_nums1 = (low + high) // 2
        
        # Calculate the partition point for nums2
        partition_nums2 = (total_length + 1) // 2 - partition_nums1
        
        # Calculate the values at the partition points
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
- Time: O(log(min(n, m))) — The time complexity is logarithmic because we are performing a binary search on the smaller array. The search space is reduced by half at each step, resulting in a logarithmic time complexity. Here, n and m are the lengths of the two input arrays.
- Space: O(1) — The space complexity is constant because we are not using any data structures that scale with the input size. We are only using a constant amount of space to store the partition points and the values at those points.

## Key Insight
The core trick to solve this problem is to use a binary search approach to find the partition point that divides the merged array into two halves, allowing us to calculate the median without actually merging the arrays.