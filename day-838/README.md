## Problem
Given two sorted arrays, find the median of the merged array without actually merging them, with the constraint that the total number of elements is odd. The goal is to calculate the middle value of the combined array, knowing that the total length of both arrays is odd, ensuring a single median value exists.

## Examples
- Example 1: 
  - Input: `nums1 = [1, 3]`, `nums2 = [2]`
  - Output: `2.0`
- Example 2: 
  - Input: `nums1 = [1, 2]`, `nums2 = [3, 4]`
  - Output: `2.5` (Note: The problem statement specifies the total number of elements is odd, but this example is provided for a general understanding. For the given problem, we consider only scenarios where total elements are odd, such as in Example 1 or when one array has an odd length and the other is empty or has an even length but together they sum to an odd total length.)

## Approach
The approach to solve this problem involves using binary search to find the median without merging the two arrays. 
Here are the steps:
1. Ensure that `nums1` is the smaller array to simplify the logic.
2. Calculate the total length of both arrays and find the middle index, considering the total length is odd.
3. Perform a binary search on `nums1` to find the partition point for `nums1` and `nums2` such that elements on the left side of the partition in both arrays are less than the elements on the right side.
4. Adjust the partition as necessary to ensure that the elements on the left side of the partition in both arrays are less than the elements on the right side.
5. Once the correct partition is found, the median is the maximum element on the left side of the partition.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure nums1 is the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    # Calculate the total length
    total_length = len(nums1) + len(nums2)
    
    # Since the total length is odd, the median is the middle element
    # We use binary search to find the partition point for nums1
    left, right = 0, len(nums1)
    
    while left <= right:
        # Partition point for nums1
        i = (left + right) // 2
        
        # Partition point for nums2, ensuring the elements on the left side are less than the elements on the right side
        j = (total_length + 1) // 2 - i
        
        # Calculate the values at the partition points
        max_left_x = float('-inf') if i == 0 else nums1[i - 1]
        min_right_x = float('inf') if i == len(nums1) else nums1[i]
        
        max_left_y = float('-inf') if j == 0 else nums2[j - 1]
        min_right_y = float('inf') if j == len(nums2) else nums2[j]
        
        # Check if the partition is correct
        if max_left_x <= min_right_y and max_left_y <= min_right_x:
            # The median is the maximum of the left side
            return max(max_left_x, max_left_y)
        elif max_left_x > min_right_y:
            # Move the partition to the left
            right = i - 1
        else:
            # Move the partition to the right
            left = i + 1
```

## Complexity
- Time: O(log(min(n, m))) — The time complexity is logarithmic because we are using binary search on the smaller array (`nums1`). The search space is reduced by half at each step, resulting in a logarithmic time complexity relative to the size of the smaller array.
- Space: O(1) — The space complexity is constant because we are not using any additional data structures that scale with the input size.

## Key Insight
The core trick to solving this problem is using binary search to find the partition point that divides the two sorted arrays into two halves, ensuring that elements on the left side of the partition are less than the elements on the right side, without actually merging the arrays.