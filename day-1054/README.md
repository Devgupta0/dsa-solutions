## Problem  

Given an array `A` of `N` integers (the numbers may be positive, negative or zero) and an integer `K`, find the **maximum length** of a contiguous sub‑array whose elements sum exactly to `K`.  
If no such sub‑array exists, output `0`.

*Input*  

```
N K
A1 A2 … AN
```

*Output*  

A single integer – the length of the longest sub‑array with sum `K` (or `0` if none).

*Constraints*  

- `1 ≤ N ≤ 2·10⁵`  
- `-10⁹ ≤ Ai ≤ 10⁹`  
- `-10⁹ ≤ K ≤ 10⁹`  

The solution must run in `O(N)` time and use `O(N)` additional memory (or less).

---

## Examples  

| Input | Output | Explanation |
|-------|--------|-------------|
| `7 5`<br>`1 2 3 2 5 1 -1` | `5` | The sub‑array `[2,3,2,5,1]` (indices 2‑6) sums to `5` and its length `5` is maximal. |
| `8 6`<br>`5 1 2 3 1 1 1 5` | `4` | Sub‑array `[3,1,1,1]` (indices 4‑7) has sum `6` and length `4`. |
| `5 10`<br>`1 2 3 4 5` | `0` | No contiguous segment adds up to `10`. |

---

## Approach  

The classic way to handle “sum of a sub‑array equals a given value” in linear time is to use **prefix sums**.

1. **Prefix sum definition** – `pref[i]` = sum of the first `i` elements (`A[0] … A[i‑1]`).  
   The sum of a sub‑array `A[l … r]` equals `pref[r+1] - pref[l]`.

2. We need `pref[r+1] - pref[l] = K` ⇒ `pref[l] = pref[r+1] - K`.  
   While scanning the array from left to right we keep a hash map `first_idx` that stores, for each prefix sum value, the **earliest** index where it appeared.

3. For the current position `i` (meaning we have computed `pref = pref[i]`):  
   *If* `pref - K` has been seen before at position `j`, then the sub‑array `A[j … i‑1]` sums to `K`. Its length is `i - j`.  
   Update the answer with the maximum of all such lengths.

4. Also store the current prefix sum in `first_idx` **only if it is not already present**, because we want the earliest occurrence to maximise the length later.

5. Initialise the map with `{0: 0}` (empty prefix) so that a sub‑array starting at index 0 can be considered.

The algorithm makes a single pass over the array, performing only constant‑time hashmap operations per element.

---

## Solution  

```python
#!/usr/bin/env python3
"""
Longest sub‑array with sum exactly K.

Time   : O(N)
Memory : O(N) for the hash map of prefix sums.
"""

import sys

def longest_subarray_with_sum_k(arr, k):
    """
    Returns the maximum length of a contiguous sub‑array whose sum is k.
    """
    first_idx = {0: 0}          # prefix sum -> earliest index (1‑based for simplicity)
    pref = 0                    # current prefix sum
    best = 0                    # best length found

    for i, val in enumerate(arr, 1):   # i is 1‑based prefix length
        pref += val

        # If we have seen pref - k before, a sub‑array ending at i‑1 sums to k
        need = pref - k
        if need in first_idx:
            length = i - first_idx[need]
            if length > best:
                best = length

        # Store earliest occurrence of this prefix sum
        if pref not in first_idx:
            first_idx[pref] = i

    return best


def solve() -> None:
    data = sys.stdin.read().strip().split()
    if not data:
        return
    n = int(data[0])
    k = int(data[1])
    arr = list(map(int, data[2:2 + n]))
    ans = longest_subarray_with_sum_k(arr, k)
    print(ans)


if __name__ == "__main__":
    solve()
```

The program reads from standard input, computes the answer using the function above, and prints the result.

---

## Complexity  

- **Time:** `O(N)` – each array element is processed once; all hashmap operations (`in`, `get`, `set`) are amortised `O(1)`.  
- **Space:** `O(N)` – in the worst case every prefix sum is distinct, so the dictionary stores `N+1` entries.

---

## Key Insight  

A sub‑array sum equals `K` exactly when two prefix sums differ by `K`; storing the earliest index of each prefix sum lets us compute the longest such distance in a single linear scan.