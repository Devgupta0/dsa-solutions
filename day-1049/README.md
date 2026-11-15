## Problem  
Given an array `A` of `N` integers (the integers may be positive, negative or zero), find the **length of the longest contiguous sub‑array whose sum is exactly zero**.  
If no such sub‑array exists, output `0`.

* `1 ≤ N ≤ 2·10⁵`  
* `-10⁹ ≤ A[i] ≤ 10⁹`  

The algorithm must run in O(N) time and use O(N) additional memory (or better).

---

## Examples  

| Input | Output | Explanation |
|-------|--------|-------------|
| `7` <br> `1 -2 3 0 -1 2 -3` | `5` | The sub‑array `[-2, 3, 0, -1, 0]` (indices 2‑6) sums to `0` and has length 5, which is maximal. |
| `5` <br> `4 2 -3 1 -4` | `4` | Sub‑array `[2, -3, 1, 0]` (indices 2‑5) sums to `0`. |
| `4` <br> `1 2 3 4` | `0` | No contiguous segment sums to zero. |

*(The first line of each test case is `N`, the second line contains the `N` space‑separated integers.)*

---

## Approach  

The sum of a sub‑array `A[l … r]` can be expressed with prefix sums:

```
prefix[i] = A[0] + A[1] + … + A[i]          (0‑based)
sum(l, r) = prefix[r] – prefix[l‑1]        (if l > 0)
```

Therefore `sum(l, r) = 0`  ⇔  `prefix[r] = prefix[l‑1]`.  
So we only need two equal prefix sums; the distance between their positions gives the length of a zero‑sum sub‑array.

**Algorithm**

1. Initialise a dictionary `first_pos` that stores, for each prefix sum value, the **first** index where it appears.  
   Insert the pair `{0: -1}` because a prefix sum of `0` before the array starts corresponds to a sub‑array that begins at index 0.
2. Iterate through the array while maintaining the running prefix sum `cur`.  
   * If `cur` has been seen before, a zero‑sum sub‑array exists from `first_pos[cur] + 1` to the current index `i`.  
     Its length is `i - first_pos[cur]`; update the answer if this length is larger.  
   * If `cur` is new, store `first_pos[cur] = i` (the earliest occurrence is what matters for maximal length).
3. After processing all elements, the stored answer is the required maximum length.

The dictionary lookup/insertion is O(1) on average, giving an overall linear solution.

---

## Solution  
```python
# --------------------------------------------------------------
#  Longest Zero‑Sum Sub‑array
#  --------------------------------------------------------------
#  The solution follows the approach described above:
#  * keep a running prefix sum,
#  * remember the first index where each sum occurs,
#  * update the answer whenever we meet a previously seen sum.
# --------------------------------------------------------------

import sys

def longest_zero_sum_subarray(arr):
    """
    Returns the length of the longest contiguous sub‑array whose sum is 0.
    """
    first_pos = {0: -1}          # prefix sum 0 appears before the array starts
    cur_sum = 0
    best_len = 0

    for i, val in enumerate(arr):
        cur_sum += val

        if cur_sum in first_pos:
            # sub‑array (first_pos[cur_sum] + 1 … i) sums to 0
            length = i - first_pos[cur_sum]
            if length > best_len:
                best_len = length
        else:
            # store the earliest occurrence of this prefix sum
            first_pos[cur_sum] = i

    return best_len


def main():
    data = sys.stdin.read().strip().split()
    if not data:
        return
    n = int(data[0])
    a = list(map(int, data[1:1 + n]))
    print(longest_zero_sum_subarray(a))


if __name__ == "__main__":
    main()
```

The program reads from standard input, computes the answer using the `longest_zero_sum_subarray` function, and prints the result.

---

## Complexity  
- **Time:** `O(N)` – each element is processed once, and all dictionary operations are average O(1).  
- **Space:** `O(N)` – in the worst case every prefix sum is distinct, so the dictionary stores `N + 1` entries.

---

## Key Insight  
A sub‑array sums to zero **iff** its two bounding prefix sums are equal; storing the earliest occurrence of each prefix sum lets us compute the maximal distance (length) in a single linear pass.