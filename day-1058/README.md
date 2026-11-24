## Problem  

Given an array `A` of `N` integers and an integer `K` (`K > 0`), count how many **contiguous sub‑arrays** have a sum that is divisible by `K`.

Formally, a sub‑array `A[l … r]` ( `0 ≤ l ≤ r < N` ) is good if  

```
(A[l] + A[l+1] + … + A[r]) mod K = 0
```

Return the total number of good sub‑arrays.

**Input**  
- The first line contains two integers `N` and `K`.  
- The second line contains `N` space‑separated integers `A[i]`.

**Output**  
- A single integer – the number of sub‑arrays whose sum is divisible by `K`.

**Constraints** (typical for an algorithmic contest)  

```
1 ≤ N ≤ 200 000
1 ≤ K ≤ 10^5
|A[i]| ≤ 10^9
```

The answer can be up to `N·(N+1)/2`, which fits in 64‑bit signed integer.



## Examples  

| Input | Output | Explanation |
|-------|--------|-------------|
|`5 3`<br>`1 2 3 4 1`|`4`|The good sub‑arrays are `[1,2]`, `[3]`, `[1,2,3]` and `[4,1]`. |
|`4 5`<br>`5 5 5 5`|`10`|Every sub‑array sum is a multiple of 5. There are `4·5/2 = 10` sub‑arrays. |
|`3 2`<br>`1 3 5`|`2`|Good sub‑arrays are `[1,3]` (sum = 4) and `[3,5]` (sum = 8). |



## Approach  

The classic trick is to use **prefix sums modulo `K`**.

1. Let `pref[i]` be the sum of the first `i` elements (`pref[0] = 0`).  
2. For each `i` compute `mod_i = pref[i] mod K`.  
   Two indices `i < j` give a sub‑array `A[i … j‑1]` whose sum is divisible by `K` **iff** `mod_i == mod_j`.  
   (Because `pref[j] – pref[i]` is a multiple of `K` exactly when their remainders are equal.)
3. While scanning the array we keep a frequency map `cnt[rem]` – how many times each remainder has appeared so far.
4. When we encounter a new remainder `rem`, all previous occurrences of the same `rem` form good sub‑arrays ending at the current position.  
   So we add `cnt[rem]` to the answer, then increment `cnt[rem]`.

The algorithm proceeds in a single left‑to‑right pass, updating the running sum, its remainder, the frequency map, and the answer.

### Step‑by‑step  

```
answer = 0
cnt = array of size K filled with 0
cnt[0] = 1               # prefix sum before the first element has remainder 0
current = 0

for each value x in A:
    current = (current + x) % K
    if current < 0:               # Python can give negative modulo for negative sums
        current += K
    answer += cnt[current]        # all earlier prefixes with same remainder
    cnt[current] += 1
print(answer)
```

The modulo operation is O(1); the whole procedure is linear.



## Solution  

```python
#!/usr/bin/env python3
"""
Count sub‑arrays whose sum is divisible by K.

Algorithm:
    - Keep running prefix sum modulo K.
    - Frequency array cnt[rem] stores how many prefixes have remainder rem.
    - Each time we see remainder r, add cnt[r] to the answer (all previous
      prefixes with the same remainder form a valid sub‑array).
Complexity: O(N) time, O(K) additional space.
"""

import sys

def solve() -> None:
    data = sys.stdin.read().strip().split()
    if not data:
        return
    it = iter(data)
    n = int(next(it))
    k = int(next(it))
    arr = [int(next(it)) for _ in range(n)]

    # Frequency of each remainder 0 … k-1
    cnt = [0] * k
    cnt[0] = 1                 # empty prefix has remainder 0
    cur = 0
    ans = 0

    for x in arr:
        cur = (cur + x) % k
        # In Python, % already returns a non‑negative result for positive k,
        # but we keep the guard for completeness when k could be negative.
        if cur < 0:
            cur += k
        ans += cnt[cur]        # all earlier prefixes with same remainder
        cnt[cur] += 1

    print(ans)


if __name__ == "__main__":
    solve()
```

The code follows exactly the algorithm described above and includes comments for clarity.



## Complexity  

- **Time:** `O(N)` – each element is processed once, and all operations inside the loop are `O(1)`.  
- **Space:** `O(K)` – the frequency array holds `K` counters; the input array itself is `O(N)` but can be streamed, so the extra space beyond the input is `O(K)`.



## Key Insight  

Two prefix sums having the same remainder modulo `K` guarantee that the sub‑array between them sums to a multiple of `K`. Counting equal remainders while scanning the array yields the answer in linear time.