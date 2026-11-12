# Custom Problem  

## Problem  

You are given an array `A` of `N` non‑negative integers and an integer `S`.  
Find the **maximum length** of a (contiguous) sub‑array whose sum does not exceed `S`.  

If no sub‑array satisfies the condition, output `0`.

**Input**

```
N S
A1 A2 … AN
```

* `1 ≤ N ≤ 2·10⁵`
* `0 ≤ Ai ≤ 10⁹`
* `0 ≤ S ≤ 10¹⁵`

**Output**

A single integer – the maximum possible length.

---

## Examples  

### Example 1  

```
5 7
2 1 5 1 3
```

**Output**

```
3
```

**Explanation**  
The sub‑array `[2, 1, 5]` has sum `8` (too large).  
The longest sub‑array with sum ≤ 7 is `[1, 5, 1]` (length 3, sum 7).

---

### Example 2  

```
4 0
0 0 0 0
```

**Output**

```
4
```

All elements are zero, so the whole array (length 4) satisfies the condition.

---

### Example 3  

```
6 10
4 2 3 7 1 2
```

**Output**

```
4
```

The sub‑array `[4, 2, 3]` has length 3, sum 9.  
A longer valid sub‑array is `[2, 3, 1, 2]` (length 4, sum 8).

---

## Approach  

Because all numbers are **non‑negative**, the sum of a window never decreases when we extend it to the right. This monotonicity lets us use the classic **two‑pointer (sliding window)** technique:

1. Keep two indices `left` and `right` that delimit the current window `[left, right)`.  
2. Move `right` forward, adding `A[right]` to the running sum.  
3. While the sum exceeds `S`, shrink the window from the left: subtract `A[left]` and increment `left`.  
4. After each adjustment the window sum ≤ S, so its length `right‑left+1` is a candidate for the answer.  
5. Track the maximum length seen.

The algorithm processes each element at most twice (once when `right` visits it, once when `left` removes it), yielding linear time.

---

## Solution  

```python
#!/usr/bin/env python3
"""
Solution for the "Custom Problem": longest sub‑array with sum ≤ S.

The algorithm uses a sliding window (two pointers) because all array
elements are non‑negative, which guarantees that extending the window
to the right never reduces its sum.
"""

import sys

def max_len_subarray_leq_sum(arr, S):
    """Return the maximum length of a sub‑array whose sum ≤ S."""
    left = 0            # left border of the window
    cur_sum = 0         # sum of elements in the current window
    best = 0            # best length found so far

    for right, val in enumerate(arr):
        cur_sum += val               # extend window to the right

        # shrink from the left while the sum is too large
        while cur_sum > S and left <= right:
            cur_sum -= arr[left]
            left += 1

        # now cur_sum ≤ S, update answer
        window_len = right - left + 1
        if window_len > best:
            best = window_len

    return best


def solve() -> None:
    data = sys.stdin.read().strip().split()
    if not data:
        return
    it = iter(data)
    N = int(next(it))
    S = int(next(it))
    A = [int(next(it)) for _ in range(N)]

    ans = max_len_subarray_leq_sum(A, S)
    print(ans)


if __name__ == "__main__":
    solve()
```

The program reads from standard input, computes the answer with the
function `max_len_subarray_leq_sum`, and prints the result.

---

## Complexity  

- **Time:** `O(N)` – each index moves at most once as `right` and once as `left`.  
- **Space:** `O(1)` – only a few integer variables are used besides the input array.

---

## Key Insight  

Because all numbers are non‑negative, a sliding‑window (two‑pointer) scan can maintain a valid sum while moving only forward, giving a linear‑time solution.