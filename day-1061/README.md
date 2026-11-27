## Problem  

Given an array `A` of **`N`** integers (positive, negative or zero), determine the length of the longest **contiguous** sub‑array whose elements sum to `0`.  
If no such sub‑array exists, output `0`.

**Input**  
```
N
A1 A2 … AN
```
* `1 ≤ N ≤ 2·10⁵`  
* `|Ai| ≤ 10⁹`

**Output**  
A single integer – the maximum length of a sub‑array with sum `0`.

---

## Examples  

| Input | Output | Explanation |
|-------|--------|-------------|
| `7`<br>`1 2 -3 3 -1 2 -2` | `5` | The sub‑array `1 2 -3 3 -1` (indices 0‑4) sums to 0 and has length 5. |
| `5`<br>`4 -1 2 -3 1` | `4` | Sub‑array `-1 2 -3 1` (indices 1‑4) sums to 0. |
| `3`<br>`1 2 3` | `0` | No sub‑array sums to 0. |

---

## Approach  

The classic way to detect a zero‑sum sub‑array is to use **prefix sums**.  
Define `pref[i] = A[0] + A[1] + … + A[i]` (sum of the first `i+1` elements).  
For any two indices `l ≤ r` we have  

```
sum(A[l..r]) = pref[r] - pref[l-1]          (pref[-1] is defined as 0)
```

Hence the sub‑array `A[l..r]` sums to 0 **iff** `pref[r] == pref[l-1]`.  
So we only need to know, for each distinct prefix‑sum value, the earliest index where it appears.  
When we encounter the same prefix‑sum again at index `i`, the sub‑array from the *first* occurrence+1 to `i` has sum 0, and its length is `i - first_index`.  
The longest such length over the whole scan is the answer.

Steps  

1. Initialise a dictionary `first_occurrence` with `{0: -1}` – the prefix sum `0` is considered to appear before the array starts.  
2. Iterate `i` from `0` to `N‑1`, maintaining a running `curr_sum`.  
3. If `curr_sum` has been seen before, compute candidate length `i - first_occurrence[curr_sum]` and update the best answer.  
4. Otherwise store `first_occurrence[curr_sum] = i`.  
5. After the loop, output the best length (or `0` if never updated).

The algorithm is linear because each element is processed once and all dictionary operations are O(1) average.

---

## Solution  
```python
# -*- coding: utf-8 -*-
"""
Solution for the "Longest Zero‑Sum Sub‑array" problem.
Works in O(N) time and O(N) additional space.
"""

import sys

def longest_zero_sum_subarray(arr):
    """
    Returns the length of the longest contiguous sub‑array whose sum is 0.
    """
    first_occurrence = {0: -1}   # prefix sum 0 appears before the array
    best = 0
    cur_sum = 0

    for i, val in enumerate(arr):
        cur_sum += val

        # If we have seen this prefix sum before, a zero‑sum sub‑array exists
        if cur_sum in first_occurrence:
            length = i - first_occurrence[cur_sum]
            if length > best:
                best = length
        else:
            # store only the first occurrence to maximise length later
            first_occurrence[cur_sum] = i

    return best


def solve() -> None:
    data = sys.stdin.read().strip().split()
    if not data:
        return
    n = int(data[0])
    a = list(map(int, data[1:1 + n]))
    print(longest_zero_sum_subarray(a))


if __name__ == "__main__":
    solve()
```

---

## Complexity  

- **Time:** `O(N)` – a single pass over the array; dictionary look‑ups/inserts are O(1) amortised.  
- **Space:** `O(N)` – at most one entry per distinct prefix‑sum (worst case `N+1` entries).

---

## Key Insight  

A sub‑array sums to 0 exactly when the prefix sum at its right end equals the prefix sum just before its left end; tracking the earliest index of each prefix sum yields the longest such segment in linear time.