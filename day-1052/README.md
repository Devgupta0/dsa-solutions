## Problem  
Given an array **A** of **N** integers (which may be positive, negative or zero) and an integer **K**, count how many **contiguous sub‑arrays** of **A** have a sum exactly equal to **K**.

* A *sub‑array* is a sequence `A[l] , A[l+1] , … , A[r]` with `0 ≤ l ≤ r < N`.  
* The answer can be large; you should output it as a normal 64‑bit integer (no modulo is required).

**Input**  
```
N K
A1 A2 … AN
```
* `1 ≤ N ≤ 2·10⁵`  
* `|Ai| ≤ 10⁹`  
* `|K| ≤ 10⁹`

**Output**  
A single integer – the number of sub‑arrays whose sum equals **K**.

---

## Examples  

| Input | Output | Explanation |
|-------|--------|-------------|
| `5 5`<br>`1 2 3 2 1` | `2` | Sub‑arrays `[2,3]` (indices 1‑2) and `[3,2]` (indices 2‑3) sum to 5. |
| `4 0`<br>`0 0 0 0` | `10` | Every pair `(l,r)` with `l ≤ r` works; there are `N·(N+1)/2 = 10` of them. |
| `6 -2`<br>`-1 -1 2 -2 1 -1` | `3` | Sub‑arrays `[-1,-1]`, `[2,-2]`, `[1,-1]` sum to –2. |

---

## Approach  

The classic way to count sub‑arrays with a given sum uses **prefix sums** and a hash map.

1. **Prefix sum definition** – `pref[i]` is the sum of the first `i` elements (`pref[0] = 0`).  
   The sum of sub‑array `A[l…r]` equals `pref[r+1] – pref[l]`.

2. For a fixed right end `r`, we need the number of `l` such that  
   `pref[r+1] – pref[l] = K  ⇔  pref[l] = pref[r+1] – K`.  

   So while scanning the array from left to right we keep a frequency table
   `cnt` of all previously seen prefix sums.  
   For the current prefix `cur = pref[r+1]` the contribution to the answer is  
   `cnt[cur - K]`.

3. After counting, we insert the current prefix `cur` into `cnt` (so it can be
   used for later positions).

4. Initialise `cnt` with `{0:1}` because the empty prefix (sum = 0) occurs once
   before the first element.

The algorithm is a single pass over the array, each operation being O(1) on
average thanks to the hash map.

---

## Solution  
```python
# -------------------------------------------------------------
# Count sub‑arrays with sum equal to K
# -------------------------------------------------------------
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

    # cnt[p] = how many times a prefix sum equal to p has been seen
    cnt = defaultdict(int)
    cnt[0] = 1                     # empty prefix
    cur = 0                        # current prefix sum
    ans = 0

    for x in arr:
        cur += x                   # pref up to current position
        # Need previous prefix = cur - k
        ans += cnt[cur - k]
        # Store current prefix for future positions
        cnt[cur] += 1

    print(ans)


if __name__ == "__main__":
    solve()
```

*The code follows exactly the steps described in the Approach section and
contains inline comments for clarity.*

---

## Complexity  
- **Time:** `O(N)` – one pass over the array, each hash‑map operation is
  amortised O(1).  
- **Space:** `O(N)` in the worst case for the hash map (all prefix sums may be
  distinct).

---

## Key Insight  
A sub‑array sum condition can be transformed into a difference of two prefix
sums, allowing us to count matching pairs with a single hash‑map lookup per
element.