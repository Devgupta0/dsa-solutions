## Problem  
Given an array **A** of **N** integers and an integer **K**, find the length of the longest contiguous sub‑array whose sum is divisible by **K**.  
If no such sub‑array exists, output `0`.

**Input format**  
```
N K
A1 A2 … AN
```
* `1 ≤ N ≤ 10^6`  
* `1 ≤ K ≤ 10^5`  
* `|Ai| ≤ 10^9`

**Output format**  
A single integer – the maximum length of a sub‑array whose sum % K == 0.

---

## Examples  

```
Input
7 5
2 7 6 1 4 5 3
Output
5
```
*Explanation*: Sub‑array `[7,6,1,4,5]` has sum `23`, and `23 % 5 = 0`. Its length `5` is maximal.

```
Input
5 3
1 2 3 4 5
Output
3
```
*Explanation*: Sub‑array `[1,2]` sum `3` (len 2) and `[3]` sum `3` (len 1); the longest is `[1,2,3]` with sum `6` (len 3).

```
Input
4 7
1 2 3 4
Output
0
```
*Explanation*: No sub‑array sum is divisible by `7`.

---

## Approach  
The sum of a sub‑array `A[l…r]` is divisible by `K` **iff** the two prefix sums up to `r` and `l‑1` have the same remainder when divided by `K`.

1. Compute prefix sums `pref[i] = (A1 + … + Ai) % K`.  
2. For each remainder `rem` we store the **first** index where it appears.  
3. While scanning the array, when we see a remainder that we have already seen at index `first[rem]`, the sub‑array `(first[rem] + 1 … i)` has sum % K == 0. Its length is `i - first[rem]`.  
4. Keep the maximum length seen.  
5. Edge case: remainder `0` at position `i` means the sub‑array `1…i` itself is valid; treat it as if the first occurrence of remainder `0` is at index `0`.

The algorithm runs in a single linear pass and uses a hash map (or a list of size `K`) to remember first occurrences.

---

## Solution
```python
#!/usr/bin/env python3
"""
Longest sub‑array with sum divisible by K
"""

import sys

def longest_divisible_subarray(arr, k):
    """
    Returns the maximum length of a contiguous sub‑array whose sum % k == 0.
    """
    # first_occurrence[rem] = earliest index where this remainder appeared
    # We use a dictionary because k can be up to 1e5 and we may not see all remainders.
    first_occurrence = {0: 0}          # remainder 0 before any element (prefix index 0)
    max_len = 0
    cur = 0                            # current prefix sum modulo k

    for i, val in enumerate(arr, 1):   # i is 1‑based prefix length
        cur = (cur + val) % k
        # Python's % can give negative results for negative numbers, fix it
        if cur < 0:
            cur += k

        if cur in first_occurrence:
            # sub‑array (first_occurrence[cur] + 1 … i) is divisible by k
            length = i - first_occurrence[cur]
            if length > max_len:
                max_len = length
        else:
            # remember the earliest index for this remainder
            first_occurrence[cur] = i

    return max_len


def main():
    data = sys.stdin.read().strip().split()
    if not data:
        return
    n = int(data[0])
    k = int(data[1])
    arr = list(map(int, data[2:2 + n]))
    print(longest_divisible_subarray(arr, k))


if __name__ == "__main__":
    main()
```

---

## Complexity  
- **Time:** `O(N)` – each element is processed once, all hash‑map operations are *O(1)* on average.  
- **Space:** `O(min(N, K))` – we store at most one index for each distinct remainder (there are at most `K` different remainders).

---

## Key Insight  
Two prefix sums with the same remainder modulo K define a sub‑array whose sum is divisible by K; tracking the earliest occurrence of each remainder yields the longest such sub‑array in linear time.