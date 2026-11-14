## Problem  
Given an integer array `A` of length `n`, find the length of the longest **contiguous** sub‑array whose elements sum to `0`.  
If no such sub‑array exists, output `0`.

**Input**  
- The first line contains a single integer `n` ( 1 ≤ `n` ≤ 10⁵ ), the size of the array.  
- The second line contains `n` space‑separated integers `A[i]` ( |`A[i]`| ≤ 10⁹ ).

**Output**  
- A single integer – the maximum length of a sub‑array with sum `0`.  

The algorithm must run in `O(n)` time and use `O(n)` additional memory (or better).

---

## Examples  

| Input | Output | Explanation |
|-------|--------|-------------|
| `7`<br>`1 2 -3 3 -1 2 -2` | `5` | Sub‑array `[2, -3, 3, -1, 2]` (indices 2‑6) sums to 0 and has length 5, which is maximal. |
| `5`<br>`1 2 3 4 5` | `0` | No contiguous segment adds up to 0. |
| `6`<br>`0 0 0 0 0 0` | `6` | The whole array sums to 0. |

---

## Approach  

1. **Prefix sums** – while scanning the array keep the running sum `pref`.  
2. If two positions `i` and `j` (`i < j`) have the same prefix sum, the sub‑array `A[i+1 … j]` sums to 0 because the contributions before `i` cancel out.  
3. Store, in a hash map, the **first** index where each prefix sum appears.  
4. For each new `pref` value:  
   * If it has never been seen, record the current index.  
   * If it has been seen before at index `first`, the length of the zero‑sum sub‑array ending at the current position is `cur_index - first`. Update the answer with the maximum of such lengths.  
5. Additionally, treat the empty prefix (sum 0 before any element) as occurring at index `-1`. This handles sub‑arrays that start at index 0.  

The algorithm processes the array once, using O(1) work per element.

---

## Solution  
```python
#!/usr/bin/env python3
"""
Maximum length of a contiguous sub‑array whose sum is zero.

Runs in O(n) time and O(n) extra space.
"""

import sys

def longest_zero_sum_subarray(arr):
    """
    Returns the length of the longest sub‑array with sum 0.
    """
    # prefix_sum -> first index where it appears
    first_occurrence = {0: -1}      # sum 0 before the array starts
    prefix = 0
    best = 0

    for idx, val in enumerate(arr):
        prefix += val

        if prefix in first_occurrence:
            # sub‑array (first_occurrence[prefix] + 1 … idx) sums to 0
            length = idx - first_occurrence[prefix]
            if length > best:
                best = length
        else:
            # remember the first time we see this sum
            first_occurrence[prefix] = idx

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

The code follows the approach described above, includes comments for clarity, and reads input from standard input exactly as required by typical competitive‑programming judges.

---

## Complexity  
- **Time:** `O(n)` – one pass over the array, each hash‑map operation is amortised `O(1)`.  
- **Space:** `O(n)` – at most one entry per distinct prefix sum (≤ `n+1` entries).

---

## Key Insight  
If two prefix sums are equal, the elements between their positions form a zero‑sum sub‑array; tracking the earliest occurrence of each sum lets us compute the longest such segment in linear time.