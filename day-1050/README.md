## Problem  
Given an array `A` of `N` integers, count the number of **important reverse pairs**  
\[
(i, j)\; \text{such that}\; 0 \le i < j < N \text{ and } A[i] > 2 \times A[j].
\]

**Input**  
```
N
A[0] A[1] … A[N‑1]
```
- `1 ≤ N ≤ 2·10⁵`
- `|A[i]| ≤ 10⁹`

**Output**  
A single integer – the total number of important reverse pairs in the array.

The answer may be larger than 32‑bit, so use 64‑bit integers.

---

## Examples  

| Input | Output | Explanation |
|-------|--------|-------------|
| `5`<br>`1 3 2 3 1` | `2` | Pairs are (3, 4) → `3 > 2·1` and (1, 4) → `3 > 2·1`. |
| `5`<br>`2 4 3 5 1` | `3` | Pairs are (0,4), (1,4), (2,4). |
| `4`<br>`-5 -2 -1 -10` | `4` | All pairs satisfy the condition because of negative numbers. |

---

## Approach  
The condition `A[i] > 2·A[j]` is reminiscent of the classic “reverse pairs” problem.  
A naïve O(N²) scan is far too slow for N up to 2·10⁵.  

The key is to **combine counting with a divide‑and‑conquer merge sort**:

1. **Divide** the array recursively until each sub‑array has size 1 (trivially sorted).  
2. **Count** important pairs where the left element lies in the left half and the right element lies in the right half:  
   *Both halves are already sorted.*  
   *For each element `x` in the left half, advance a pointer in the right half until `x ≤ 2·right[p]`.  
   *All elements before `p` form a valid pair with `x`. Add their count.*  
3. **Merge** the two sorted halves into a single sorted array (standard merge step).  

Because each level processes the whole array once, the total work is `O(N log N)`.  
The merge step also uses only `O(N)` auxiliary space.

---

## Solution  
```python
"""
Important Reverse Pairs
----------------------
Counts pairs (i, j) with i < j and A[i] > 2 * A[j] in O(N log N) time
using a merge‑sort based divide‑and‑conquer algorithm.
"""

import sys
sys.setrecursionlimit(1 << 25)

def count_reverse_pairs(arr):
    """Returns the number of important reverse pairs in arr."""
    n = len(arr)
    temp = [0] * n          # auxiliary array for merging

    def sort_and_count(l, r):
        """Sort arr[l:r] (half‑open) and return the number of important pairs."""
        if r - l <= 1:          # length 0 or 1 → already sorted
            return 0

        m = (l + r) // 2
        cnt = sort_and_count(l, m) + sort_and_count(m, r)

        # ---- count cross pairs ----
        j = m
        for i in range(l, m):
            # Move j while left[i] > 2 * right[j]
            while j < r and arr[i] > 2 * arr[j]:
                j += 1
            cnt += j - m   # all elements arr[m .. j-1] satisfy the condition

        # ---- merge step ----
        i, j = l, m
        idx = l
        while i < m and j < r:
            if arr[i] <= arr[j]:
                temp[idx] = arr[i]
                i += 1
            else:
                temp[idx] = arr[j]
                j += 1
            idx += 1
        # copy remaining elements
        while i < m:
            temp[idx] = arr[i]
            i += 1
            idx += 1
        while j < r:
            temp[idx] = arr[j]
            j += 1
            idx += 1

        # write merged segment back to original array
        arr[l:r] = temp[l:r]
        return cnt

    return sort_and_count(0, n)


def main():
    data = sys.stdin.read().strip().split()
    if not data:
        return
    n = int(data[0])
    a = list(map(int, data[1:1 + n]))
    print(count_reverse_pairs(a))


if __name__ == "__main__":
    main()
```

---

## Complexity  
- **Time:** `O(N log N)` – each level of the recursion scans the whole current segment once (counting + merging).  
- **Space:** `O(N)` – the auxiliary `temp` array of size `N` plus the recursion stack (`O(log N)`).

---

## Key Insight  
While performing merge sort, the two halves are already sorted, allowing us to count all cross‑half pairs in linear time by sliding a pointer over the right half. This turns an O(N²) problem into O(N log N).