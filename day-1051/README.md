# Custom Problem – Longest Subarray with Sum Divisible by k  

**Tags:** `algorithms`, `arrays`

---

## Problem  
Given an integer array `nums` of length `n` and an integer `k (k > 0)`, find the length of the longest **contiguous** subarray whose sum is divisible by `k`.  
If no such subarray exists, return `0`.

**Input**  
- `n` – the number of elements (`1 ≤ n ≤ 10⁵`).  
- `k` – a positive integer (`1 ≤ k ≤ 10⁹`).  
- `nums` – an array of `n` integers (`|nums[i]| ≤ 10⁹`).

**Output**  
- A single integer – the maximum length of a subarray with sum `% k == 0`.  

---

## Examples  

| # | Input | Output | Explanation |
|---|-------|--------|-------------|
| 1 | `n = 6, k = 3`<br>`nums = [2, 7, 6, 1, 4, 5]` | `4` | Subarray `[7, 6, 1, 4]` has sum `18`, which is divisible by `3`. Its length `4` is maximal. |
| 2 | `n = 5, k = 5`<br>`nums = [5, 0, 5, 5, 5]` | `5` | The whole array sums to `20`, divisible by `5`. |
| 3 | `n = 4, k = 7`<br>`nums = [1, 2, 3, 4]` | `0` | No subarray sum is a multiple of `7`. |

---

## Approach  

The sum of a subarray `nums[l … r]` is `prefix[r] - prefix[l‑1]`.  
We need `(prefix[r] - prefix[l‑1]) % k == 0`, i.e. `prefix[r] % k == prefix[l‑1] % k`.  

Therefore, for each remainder `rem = prefix[i] % k` we only need to know the **earliest** index where this remainder appeared.  
When we encounter the same remainder later at index `j`, the subarray `(earliest_index + 1 … j)` has a sum divisible by `k`. Its length is `j - earliest_index`. Keeping the maximum of those lengths gives the answer.

Steps  

1. Initialise a dictionary `first` mapping each remainder to its first occurrence index.  
   - Store `0 → -1` because a prefix sum of `0` (empty prefix) has remainder `0`.  
2. Iterate through the array, maintaining a running prefix sum `pref`.  
3. Compute `rem = pref % k`. In Python, adjust negative remainders with `rem = (pref % k + k) % k`.  
4. If `rem` has been seen before, compute candidate length `i - first[rem]` and update the answer.  
5. If `rem` is new, store `first[rem] = i`.  
6. After the loop, output the maximal length (or `0` if never updated).

The algorithm processes the array once and uses a hash map of at most `k` (or `n`) entries.

---

## Solution  
```python
"""
Longest subarray whose sum is divisible by k.
Time   : O(n)
Memory : O(min(k, n))  (hash map of remainders)
"""

import sys

def longest_divisible_subarray(nums, k):
    """
    Returns the length of the longest contiguous subarray with sum % k == 0.
    """
    # remainder -> earliest index where this remainder appeared
    first_occurrence = {0: -1}          # prefix sum 0 before the array starts
    pref = 0
    best = 0

    for i, val in enumerate(nums):
        pref += val
        # Python's % already yields non‑negative for positive k,
        # but we guard against negative k (not needed here) for clarity.
        rem = pref % k
        # If we have seen this remainder before, a subarray with sum % k == 0 exists
        if rem in first_occurrence:
            length = i - first_occurrence[rem]
            if length > best:
                best = length
        else:
            # store only the first occurrence to maximize length later
            first_occurrence[rem] = i

    return best


def main() -> None:
    """
    Expected input format:
    n k
    a1 a2 ... an
    """
    data = sys.stdin.read().strip().split()
    if not data:
        return
    it = iter(data)
    n = int(next(it))
    k = int(next(it))
    nums = [int(next(it)) for _ in range(n)]

    ans = longest_divisible_subarray(nums, k)
    print(ans)


if __name__ == "__main__":
    main()
```

The program reads from standard input, computes the answer using the described algorithm, and prints the result.

---

## Complexity  

- **Time:** `O(n)` – each element is processed once, and dictionary operations are average `O(1)`.  
- **Space:** `O(min(k, n))` – at most one entry per distinct remainder; there are at most `k` possible remainders, but never more than `n+1` entries.

---

## Key Insight  
A subarray sum is divisible by `k` **iff** the prefix sums at its boundaries share the same remainder modulo `k`; tracking the earliest occurrence of each remainder yields the longest such subarray in linear time.