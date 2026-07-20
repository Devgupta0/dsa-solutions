## Problem
Given two sorted arrays, find the median of the combined array. The input arrays are too large to fit into memory, so we need to find a solution that can handle this constraint. The goal is to calculate the median without having to load the entire combined array into memory.

## Examples
* Input: `nums1 = [1, 3]`, `nums2 = [2]`
  Output: `2.0`
* Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  Output: `2.5`
* Input: `nums1 = [0, 0]`, `nums2 = [0, 0]`
  Output: `0.0`

## Approach
To solve this problem, we can use a modified binary search algorithm. The idea is to partition both arrays into two halves each, such that the elements in the left half of both arrays are less than or equal to the elements in the right half. We can then use the partition indices to calculate the median.

Here are the steps:
1. Calculate the total length of the combined array.
2. Determine the partition index for the combined array, which is the index at which the median lies.
3. Use binary search to find the partition indices for both arrays, such that the elements in the left half of both arrays are less than or equal to the elements in the right half.
4. Once the partition indices are found, calculate the median using the elements at the partition indices.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure that nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length of the combined array
    total_length = len(nums1) + len(nums2)
    
    # Determine the partition index for the combined array
    partition_index = total_length // 2
    
    # Initialize the low and high pointers for binary search
    low = 0
    high = len(nums1) - 1
    
    while True:
        # Calculate the partition index for nums1
        i = (low + high) // 2
        
        # Calculate the partition index for nums2
        j = partition_index - i - 2
        
        # Calculate the values at the partition indices
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
        # Adjust the partition index
        elif nums1_left > nums2_right:
            high = i - 1
        else:
            low = i + 1
```

## Complexity
- Time: O(log(min(m, n))) — The time complexity is logarithmic because we are using binary search to find the partition indices. The search space is reduced by half at each step, resulting in a logarithmic time complexity. The search space is at most the size of the smaller array, hence the log(min(m, n)) term.
- Space: O(1) — The space complexity is constant because we are only using a fixed amount of space to store the partition indices and the values at the partition indices.

## Key Insight
The core trick to solving this problem is to use a modified binary search algorithm to find the partition indices for both arrays, allowing us to calculate the median without having to load the entire combined array into memory.