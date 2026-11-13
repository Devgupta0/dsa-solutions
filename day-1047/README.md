## Problem  

You are given an array `A` of `N` integers (`1 ≤ N ≤ 10^5`).  
Select a subset of elements such that **no two chosen elements are adjacent** in the original array and the sum of the chosen elements is as large as possible.  

Return that maximum possible sum.  
If all numbers are negative you may choose an empty subset, whose sum is `0`.

*Input*  
- The first line contains a single integer `N`.  
- The second line contains `N` space‑separated integers `A1, A2, …, AN` (each `|Ai| ≤ 10^9`).  

*Output*  
- A single integer – the maximum sum obtainable under the non‑adjacent constraint.

---

## Examples  

| Input | Output | Explanation |
|-------|--------|-------------|
|`5`<br>`3 2 5 10 7`|`15`|Choose `3 + 5 + 7 = 15` (indices 1, 3, 5).|
|`4`<br>`-1 -2 -3 -4`|`0`|All numbers are negative, the optimal choice is the empty set.|
|`6`<br>`4 1 1 4 2 1`|`9`|Choose `4 + 1 + 4 = 9` (indices 1, 4, 5).|

---

## Approach  

The restriction “no two chosen elements are adjacent” suggests a **dynamic‑programming** (DP) recurrence that looks at the prefix of the array and decides whether to take the current element or not.

Let `dp[i]` be the maximum sum we can obtain from the first `i` elements (`A[0] … A[i‑1]`).  
For each position `i` (1‑based in the recurrence) we have two choices:

1. **Skip `A[i‑1]`** – the best sum stays `dp[i‑1]`.  
2. **Take `A[i‑1]`** – then we cannot take `A[i‑2]`, so we add `A[i‑1]` to `dp[i‑2]`.

Hence  
```
dp[i] = max(dp[i‑1], dp[i‑2] + A[i‑1])
```
with base cases  
`dp[0] = 0` (empty prefix) and `dp[1] = max(0, A[0])` (we may also ignore a negative first element).

The answer is `dp[N]`.

Because each step uses only the two previous values, we can keep just two variables instead of an entire array, achieving **O(1) extra space**.

---

## Solution
```python
# Maximum sum of non‑adjacent elements
# Works for N up to 1e5 and |Ai| up to 1e9

import sys

def max_non_adjacent_sum(arr):
    """
    Returns the maximum sum of a subset of `arr` with no two
    elements adjacent. An empty subset (sum = 0) is allowed.
    """
    # prev2 = dp[i-2], prev1 = dp[i-1]
    prev2 = 0                         # dp[0]
    prev1 = max(0, arr[0]) if arr else 0   # dp[1]

    for i in range(1, len(arr)):
        # Option 1: skip current element
        skip = prev1
        # Option 2: take current element
        take = prev2 + arr[i]
        cur = max(skip, take)

        # shift the window forward
        prev2, prev1 = prev1, cur

    return prev1

def solve() -> None:
    data = sys.stdin.read().strip().split()
    if not data:
        return
    n = int(data[0])
    a = list(map(int, data[1:1 + n]))
    # Edge case: empty array (should not happen per constraints)
    if n == 0:
        print(0)
        return
    print(max_non_adjacent_sum(a))

if __name__ == "__main__":
    solve()
```

---

## Complexity  

- **Time:** `O(N)` – each element is processed once in the loop.  
- **Space:** `O(1)` – only a few integer variables are kept regardless of `N`.

---

## Key Insight  

The optimal answer for the first `i` elements depends only on the best results for the first `i‑1` and `i‑2` elements, enabling a linear DP with constant extra memory.