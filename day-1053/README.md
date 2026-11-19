## Problem  
Given an array `A` of `N` integers (positive, negative or zero), determine the length of the longest **contiguous** sub‑array whose elements sum to zero.  
If no such sub‑array exists, output `0`.

**Input**  
- The first line contains an integer `N` ( 1 ≤ `N` ≤ 10⁶ ).  
- The second line contains `N` space‑separated integers `A[i]` ( |`A[i]`| ≤ 10⁹ ).

**Output**  
- A single integer: the maximum length of a zero‑sum sub‑array.

---

## Examples  

| Input | Output | Explanation |
|-------|--------|-------------|
| `6`<br>`1 2 -3 3 -1 2` | `4` | Sub‑array `[1, 2, -3, 3]` (indices 0‑3) sums to 0 and has length 4. |
| `5`<br>`4 -2 -2 4 0` | `5` | The whole array sums to 0, so the answer is 5. |
| `4`<br>`1 2 3 4` | `0` | No contiguous sub‑array sums to 0. |

---

## Approach  
The sum of a sub‑array `A[l … r]` equals `prefix[r] - prefix[l‑1]`, where `prefix[i]` is the sum of the first `i+1` elements.  
If two prefix sums are equal, the elements between those positions form a zero‑sum sub‑array.

**Plain English**  
1. Scan the array while maintaining a running prefix sum.  
2. Store the earliest index where each prefix sum value appears in a hash map.  
3. When the current prefix sum has been seen before, the sub‑array between the previous index + 1 and the current index has sum 0.  
4. Keep the maximum length among all such sub‑arrays.  
5. Also treat prefix sum `0` at index `i` as a zero‑sum sub‑array starting from the first element.

**Step‑by‑step**  
1. Initialise `max_len = 0`, `prefix = 0`, and a dictionary `first_occurrence = {0: -1}` (the sum `0` occurs before the array starts).  
2. For each index `i` from `0` to `N‑1`  
   - Update `prefix += A[i]`.  
   - If `prefix` is already in `first_occurrence` → a zero‑sum sub‑array exists:  
     `length = i - first_occurrence[prefix]`; update `max_len` if larger.  
   - Otherwise, record `first_occurrence[prefix] = i` (store the earliest appearance).  
3. After the loop, output `max_len`.

The algorithm uses a single linear pass and constant‑time dictionary operations.

---

## Solution
```python
#!/usr/bin/env python3
"""
Longest Zero‑Sum Sub‑Array
-------------------------

Given an integer array, returns the length of the longest contiguous
sub‑array whose sum equals zero.

Complexities:
    Time  : O(N)
    Space : O(N)  (hash map of prefix sums)
"""

import sys

def longest_zero_sum_subarray(arr):
    """
    Returns the maximum length of a sub‑array with sum zero.
    """
    # prefix sum -> earliest index where this sum appears
    first_occurrence = {0: -1}          # sum 0 before the array starts
    prefix = 0
    max_len = 0

    for i, val in enumerate(arr):
        prefix += val

        if prefix in first_occurrence:
            # sub‑array (first_occurrence[prefix] + 1) .. i sums to 0
            cur_len = i - first_occurrence[prefix]
            if cur_len > max_len:
                max_len = cur_len
        else:
            # store the first time we see this sum
            first_occurrence[prefix] = i

    return max_len

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

---

## Complexity  
- **Time:** `O(N)` – each element is processed once; dictionary look‑ups/inserts are amortised `O(1)`.  
- **Space:** `O(N)` – in the worst case all prefix sums are distinct, so the hash map stores `N+1` entries.

---

## Key Insight  
A zero‑sum sub‑array exists exactly when a prefix sum repeats; tracking the earliest occurrence of each prefix sum lets us compute the longest such sub‑array in linear time.