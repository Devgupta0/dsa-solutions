## Problem  

Given an array `A` of `N` integers (`1 ≤ N ≤ 2·10⁵`) and an integer `K` (`1 ≤ K ≤ 10⁹`), count the number of **contiguous sub‑arrays** whose sum is divisible by `K`.  

Formally, a sub‑array `A[l … r]` (`0 ≤ l ≤ r < N`) is good if  

```
(A[l] + A[l+1] + … + A[r]) mod K = 0
```  

Output the total number of good sub‑arrays. The answer can be up to `N·(N+1)/2`, so use 64‑bit integer arithmetic.

---

## Examples  

| Input | Output | Explanation |
|-------|--------|-------------|
| `5 3`<br>`1 2 3 4 1` | `4` | Sub‑arrays with sum divisible by 3 are: `[1,2]`, `[3]`, `[1,2,3]`, `[2,3,4]`. |
| `4 5`<br>`5 5 5 5` | `10` | Every sub‑array sum is a multiple of 5. There are `4·5/2 = 10` sub‑arrays. |
| `6 7`<br>`3 1 4 1 5 9` | `2` | Good sub‑arrays are `[3,1,4,1,5,9]` (sum = 23 ≡ 2 mod 7 → not good) – actually the good ones are `[3,1,4,1,5]` (sum = 14) and `[9]` (sum = 9 ≡ 2 mod 7? wait) – correct good sub‑arrays are `[3,1,4,1,5]` (14) and `[5,9]` (14). |

*(The third example illustrates that only two sub‑arrays sum to a multiple of 7.)*

---

## Approach  

The condition “sub‑array sum % K == 0” can be rewritten with **prefix sums**.

1. Define `pref[i]` = sum of the first `i` elements (`pref[0] = 0`).  
2. The sum of sub‑array `A[l … r]` equals `pref[r+1] - pref[l]`.  
3. `pref[r+1] - pref[l]` is divisible by `K` **iff** `pref[r+1] % K == pref[l] % K`.

Therefore, for each remainder `r = pref[i] % K` we only need to know how many earlier prefix sums had the same remainder.  

Algorithm steps  

* Initialise a hash map `cnt` that stores how many times each remainder has appeared.  
* Start with `cnt[0] = 1` because the empty prefix (`pref[0]`) has remainder 0.  
* Iterate through the array, maintaining a running prefix sum `cur`.  
  * Compute `rem = cur % K`. In Python we need a non‑negative remainder: `rem = ((cur % K) + K) % K`.  
  * The number of good sub‑arrays ending at the current position equals `cnt[rem]` (all earlier prefixes with the same remainder).  
  * Add `cnt[rem]` to the answer.  
  * Increment `cnt[rem]`.  

The map gives O(1) expected time per element, leading to an overall linear solution.

---

## Solution  
```python
# -*- coding: utf-8 -*-
"""
Solution for the "Sub‑array Sum Divisible by K" problem.

We count sub‑arrays whose sum is a multiple of K using prefix sums
and a frequency map of remainders.
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
    arr = [int(next(it)) for _ in range(n)]

    # freq[r] = how many prefix sums have remainder r (mod k)
    freq = defaultdict(int)
    freq[0] = 1                     # empty prefix

    cur = 0                         # running prefix sum
    ans = 0

    for x in arr:
        cur += x
        # Python's % can be negative for negative cur, fix it
        rem = ((cur % k) + k) % k
        ans += freq[rem]            # all previous prefixes with same remainder
        freq[rem] += 1

    print(ans)


if __name__ == "__main__":
    solve()
```

The program reads from standard input, follows the algorithm described above, and prints the required count.

---

## Complexity  

- **Time:** `O(N)` – one pass over the array, each operation (hash‑map access, arithmetic) is `O(1)` amortised.  
- **Space:** `O(min(N, K))` – at most one entry per distinct remainder; in the worst case we store up to `K` counters, but `K` can be as large as `10⁹` while only `N` remainders actually appear, so the map size never exceeds `N+1`.

---

## Key Insight  

A sub‑array sum is divisible by `K` exactly when the two bordering prefix sums have the same remainder modulo `K`; counting equal remainders gives the answer in linear time.