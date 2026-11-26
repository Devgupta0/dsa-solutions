## Problem  
Given an array `A` of `N` integers (positive, negative or zero), find the length of the longest **contiguous** sub‑array whose elements sum to `0`.  
If no such sub‑array exists, output `0`.

**Input**  
```
N                     # 1 ≤ N ≤ 10^5
A1 A2 … AN            # -10^9 ≤ Ai ≤ 10^9
```

**Output**  
A single integer – the maximum length of a zero‑sum sub‑array.

---

## Examples  

| Input | Output | Explanation |
|-------|--------|-------------|
|`7`<br>`1 2 -3 3 -1 2 -2`|`5`|The sub‑array `2 -3 3 -1 2` (indices 2‑6) sums to 0 and has length 5.|
|`5`<br>`4 1 2 3 5`|`0`|No contiguous segment sums to 0.|
|`6`<br>`0 0 0 0 0 0`|`6`|The whole array sums to 0.|

---

## Approach  
The classic way to detect a zero‑sum sub‑array is to use **prefix sums**.

1. **Prefix sum** `pref[i]` = sum of the first `i` elements (`pref[0] = 0`).  
2. The sum of sub‑array `A[l … r]` equals `pref[r+1] - pref[l]`.  
3. This sum is `0` **iff** `pref[r+1] == pref[l]`.  
   Therefore, whenever a prefix sum value appears twice, the segment between the two
   occurrences has sum 0.  
4. While scanning the array, store for each distinct prefix sum the **first**
   index where it occurs in a hash map (`first_index`).  
5. At position `i` (after processing `A[i-1]`) we have current prefix `cur`.  
   * If `cur` has been seen before at index `j`, the sub‑array `j … i-1` has sum 0;  
     its length is `i - j`. Update the answer with the maximum of such lengths.  
   * If `cur` is new, record `first_index[cur] = i`.  

The algorithm runs in a single pass, using O(N) time and O(N) extra space for the map.

---

## Solution  
```python
#!/usr/bin/env python3
"""
Zero‑sum sub‑array – longest length

Given an array A of N integers, returns the length of the longest contiguous
sub‑array whose sum equals 0.
"""

import sys

def longest_zero_sum_subarray(arr):
    """
    Returns the maximum length of a sub‑array with sum 0.
    """
    # Map: prefix_sum -> first index where it appears
    first_index = {0: 0}          # prefix sum 0 occurs before the array starts
    cur_sum = 0
    best = 0

    for i, val in enumerate(arr, start=1):   # i is the length of processed prefix
        cur_sum += val

        if cur_sum in first_index:
            # sub‑array (first_index[cur_sum] … i-1) sums to 0
            length = i - first_index[cur_sum]
            if length > best:
                best = length
        else:
            # store the earliest occurrence of this prefix sum
            first_index[cur_sum] = i

    return best


def main():
    data = sys.stdin.read().strip().split()
    if not data:
        return
    n = int(data[0])
    arr = list(map(int, data[1:1 + n]))
    print(longest_zero_sum_subarray(arr))


if __name__ == "__main__":
    main()
```

The program reads from standard input, computes the answer using the
`longest_zero_sum_subarray` function, and prints the result.

---

## Complexity  
- **Time:** `O(N)` – each element is processed once, and all hash‑map operations are average `O(1)`.  
- **Space:** `O(N)` – in the worst case each prefix sum is distinct, so the map stores up to `N+1` entries.

---

## Key Insight  
A sub‑array sums to zero exactly when its two bounding prefix sums are equal; tracking the first occurrence of each prefix sum lets us compute the longest such segment in linear time.