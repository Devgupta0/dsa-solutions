## Problem  

Given an array `A` of `N` integers and an integer `K` ( `K > 0` ), find the length of the **longest contiguous sub‑array** whose sum is divisible by `K`.  
If no such sub‑array exists, output `0`.

**Input**  
```
N K
A1 A2 … AN
```
- `1 ≤ N ≤ 10^5`  
- `1 ≤ K ≤ 10^9`  
- `|Ai| ≤ 10^9`

**Output**  
A single integer – the maximum length of a sub‑array whose sum % K == 0.

---

## Examples  

| Input | Output | Explanation |
|-------|--------|-------------|
| `6 3`<br>`2 7 6 1 4 5` | `4` | Sub‑array `[7,6,1,4]` has sum `18`, which is divisible by `3`. Its length `4` is maximal. |
| `5 5`<br>`1 2 3 4 5` | `5` | The whole array sums to `15`, divisible by `5`. |
| `4 7`<br>`1 2 3 4` | `0` | No sub‑array sum is a multiple of `7`. |

---

## Approach  

The problem reduces to finding two indices `i < j` such that  

```
(prefix_sum[j] - prefix_sum[i]) % K == 0
```

which is equivalent to  

```
prefix_sum[j] % K == prefix_sum[i] % K
```

So we only need the earliest occurrence of each remainder of the prefix sum modulo `K`.  

**Step‑by‑step algorithm**

1. Initialise a dictionary `first_occurrence` with remainder `0` mapped to index `-1` (this handles sub‑arrays starting at position 0).  
2. Iterate over the array, maintaining `curr_sum` (the running prefix sum).  
3. Compute `rem = ((curr_sum % K) + K) % K` to keep the remainder non‑negative.  
4. If `rem` has been seen before, a sub‑array ending at the current index has sum divisible by `K`. Its length is `idx - first_occurrence[rem]`. Update the answer if this length is larger.  
5. If `rem` is **not** in the dictionary, store `first_occurrence[rem] = idx` (the earliest index for this remainder).  
6. After processing all elements, the stored answer is the required maximum length.

The algorithm is linear because each element is processed once and all dictionary operations are O(1) on average.

---

## Solution  
```python
# full working Python solution with comments
import sys

def longest_divisible_subarray(arr, k):
    """
    Returns the length of the longest contiguous sub‑array whose sum is divisible by k.
    """
    first_occurrence = {0: -1}          # remainder -> earliest index
    cur_sum = 0
    best = 0

    for idx, val in enumerate(arr):
        cur_sum += val
        # Python's % can be negative for negative sums, fix it
        rem = ((cur_sum % k) + k) % k

        if rem in first_occurrence:
            length = idx - first_occurrence[rem]
            if length > best:
                best = length
        else:
            # store only the first time we see this remainder
            first_occurrence[rem] = idx

    return best

def solve() -> None:
    data = sys.stdin.read().strip().split()
    if not data:
        return
    it = iter(data)
    n = int(next(it))
    k = int(next(it))
    arr = [int(next(it)) for _ in range(n)]

    ans = longest_divisible_subarray(arr, k)
    print(ans)

if __name__ == "__main__":
    solve()
```

---

## Complexity  

- **Time:** `O(N)` — each element is visited once and dictionary look‑ups/inserts are O(1) amortised.  
- **Space:** `O(min(N, K))` — at most one entry per distinct remainder (0 … K‑1). In the worst case we store `K` entries, but never more than `N`.

---

## Key Insight  

Two prefix sums with the same remainder modulo `K` define a sub‑array whose sum is a multiple of `K`; storing the earliest index for each remainder lets us compute the longest such sub‑array in linear time.