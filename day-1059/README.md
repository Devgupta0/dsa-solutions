## Problem  

You are given an array `A` of `N` integers and an integer `K`.  
A **sub‑array** is a contiguous part of `A`.  
Your task is to count how many sub‑arrays have a sum that is **divisible by `K`**.  

Formally, find the number of pairs `(l, r)` (`0 ≤ l ≤ r < N`) such that  

```
(A[l] + A[l+1] + … + A[r]) % K == 0
```

**Input**  

```
N K
A1 A2 … AN
```

* `1 ≤ N ≤ 2·10^5`
* `1 ≤ K ≤ 10^5`
* `|Ai| ≤ 10^9`

**Output**  

A single integer – the number of sub‑arrays whose sum is divisible by `K`.  

The answer can be large, but it fits into a 64‑bit signed integer.

---

## Examples  

| Input | Output | Explanation |
|-------|--------|-------------|
| `5 3`<br>`1 2 3 4 1` | `4` | The qualifying sub‑arrays are `[1,2]`, `[3]`, `[1,2,3]`, `[2,3,4]`. |
| `4 5`<br>`5 5 5 5` | `10` | Every sub‑array sum is a multiple of 5. There are `4·5/2 = 10` sub‑arrays. |
| `3 2`<br>`1 3 5` | `2` | Sub‑arrays `[1,3]` (sum = 4) and `[5]` (sum = 5) are divisible by 2. |

---

## Approach  

1. **Prefix sums** – Let `pref[i]` be the sum of the first `i` elements (`pref[0] = 0`).  
   The sum of a sub‑array `[l, r]` equals `pref[r+1] – pref[l]`.

2. **Modulo trick** –  
   `(pref[r+1] – pref[l]) % K == 0`  ⇔  `pref[r+1] % K == pref[l] % K`.  
   Therefore a sub‑array is good exactly when the two prefix sums that bound it have the same remainder modulo `K`.

3. **Counting equal remainders** –  
   While scanning the array we keep a frequency map `cnt[rem]` that stores how many times a particular remainder `rem` (in `[0, K‑1]`) has already appeared among the processed prefix sums.  
   For the current prefix remainder `cur`, every previous occurrence of the same remainder creates a new good sub‑array ending at the current position. So we add `cnt[cur]` to the answer and then increment `cnt[cur]`.

4. **Initial condition** –  
   The empty prefix (`pref[0] = 0`) has remainder `0`. We start with `cnt[0] = 1` so that sub‑arrays starting at index 0 are counted correctly.

5. **Implementation details** –  
   * Use a dictionary (or a list of size `K` when `K` is modest) for `cnt`.  
   * Keep the running sum in a 64‑bit variable and apply the modulo operation after each addition, adjusting negative values to the range `[0, K‑1]`.  
   * The algorithm processes the array once – **O(N)** time.

---

## Solution  
```python
#!/usr/bin/env python3
"""
Count sub‑arrays whose sum is divisible by K.

Algorithm:
    • Keep running prefix sum modulo K.
    • For each remainder, the number of previous identical remainders
      tells how many good sub‑arrays end at the current index.
    • Complexity: O(N) time, O(K) (or O(N) in worst case) space.
"""

import sys
from collections import defaultdict

def solve() -> None:
    data = sys.stdin.read().strip().split()
    if not data:
        return
    it = iter(data)
    n = int(next(it))
    k = int(next(it))

    # frequency of each remainder seen so far
    cnt = defaultdict(int)
    cnt[0] = 1                     # empty prefix
    cur = 0                         # current prefix sum modulo k
    ans = 0

    for _ in range(n):
        x = int(next(it))
        cur = (cur + x) % k
        # Python's % already yields non‑negative remainder, but keep it safe
        if cur < 0:
            cur += k
        ans += cnt[cur]            # all previous same remainders form good sub‑arrays
        cnt[cur] += 1

    print(ans)


if __name__ == "__main__":
    solve()
```

---

## Complexity  

- **Time:** `O(N)` – each element is processed once, all dictionary operations are average `O(1)`.  
- **Space:** `O(K)` in the typical case (the map stores at most `K` different remainders).  
  In the worst case where `K` can be larger than `N`, the map holds at most `N+1` entries, i.e. `O(N)`.

---

## Key Insight  

Two prefix sums having the same remainder modulo `K` guarantee that the sub‑array between them sums to a multiple of `K`. Counting equal remainders while scanning the array yields the answer in linear time.