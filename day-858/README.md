## Problem
The problem requires finding the median of the combined array of two sorted arrays without merging them. The two input arrays are sorted in ascending order, and the goal is to calculate the median of the combined array. The solution must achieve a time complexity of O(log(min(n, m))), where n and m are the lengths of the two input arrays.

## Examples
* Example 1: Input - `nums1 = [1, 3]`, `nums2 = [2]`, Output - `2.0`
* Example 2: Input - `nums1 = [1, 2]`, `nums2 = [3, 4]`, Output - `(2 + 3) / 2 = 2.5`
* Example 3: Input - `nums1 = [0, 0]`, `nums2 = [0, 0]`, Output - `0.0`

## Approach
To solve this problem, we can use a binary search approach. The idea is to partition both arrays such that the elements on the left side of the partition in both arrays are less than or equal to the elements on the right side. We can then use this partition to calculate the median.

Here's a step-by-step explanation:
1. Calculate the total length of the combined array.
2. Determine if the total length is odd or even, as this will affect how we calculate the median.
3. Initialize two pointers, one for each array, to keep track of the current partition.
4. Use binary search to adjust the partition until we find the correct partition that satisfies the condition.
5. Once the correct partition is found, calculate the median based on the elements at the partition.

## Solution
```python
def findMedianSortedArrays(nums1, nums2):
    # Make sure that nums1 is the smaller array to simplify the logic
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1

    x, y = len(nums1), len(nums2)
    start = 0
    end = x

    while start <= end:
        # Partition for nums1
        partitionX = (start + end) // 2
        # Partition for nums2
        partitionY = (x + y + 1) // 2 - partitionX

        # Calculate the values at the partitions
        maxLeftX = float('-inf') if partitionX == 0 else nums1[partitionX - 1]
        minRightX = float('inf') if partitionX == x else nums1[partitionX]

        maxLeftY = float('-inf') if partitionY == 0 else nums2[partitionY - 1]
        minRightY = float('inf') if partitionY == y else nums2[partitionY]

        # Check if the partition is correct
        if maxLeftX <= minRightY and maxLeftY <= minRightX:
            # Calculate the median
            if (x + y) % 2 == 0:
                return (max(maxLeftX, maxLeftY) + min(minRightX, minRightY)) / 2
            else:
                return max(maxLeftX, maxLeftY)
        # Adjust the partition
        elif maxLeftX > minRightY:
            end = partitionX - 1
        else:
            start = partitionX + 1
```

## Complexity
- Time: O(log(min(n, m))) — The time complexity is logarithmic because we are using binary search to find the correct partition. The search space is reduced by half at each step, resulting in a logarithmic time complexity. The reason we use log(min(n, m)) is that we are ensuring that the smaller array is always the one we are partitioning, which reduces the search space.
- Space: O(1) — The space complexity is constant because we are only using a fixed amount of space to store the pointers and the values at the partitions, regardless of the input size.

## Key Insight
The core trick to solve this problem is to use binary search to find the correct partition of the two arrays, allowing us to calculate the median without merging the arrays.