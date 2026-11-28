## Problem  
Given an array `A` of `N` integers (both positive and negative), find the maximum possible sum of a **non‑empty** contiguous subarray of `A`.  
A subarray is a sequence of consecutive elements of the original array.

*Input*  
- The first line contains an integer `N` ( 1 ≤ `N` ≤ 10⁶ ).  
- The second line contains `N` space‑separated integers `A[i]` ( |`A[i]`| ≤ 10⁹ ).

*Output*  
- A single integer: the maximum subarray sum.

The algorithm must run in `O(N)` time and use `O(1)` additional memory.

---

## Examples  

| Input | Output | Explanation |
|-------|--------|-------------|
| `5`<br>`-2 1 -3 4 -1` | `4` | The best subarray is `[4]` with sum 4. |
| `7`<br>`1 2 -1 2 -3 2 2` | `6` | Subarray `[1,2,-1,2]` (or `[2,2]` after the last `-3`) gives sum 6. |
| `3`<br>`-5 -2 -3` | `-2` | All numbers are negative, so we pick the largest one, `-2`. |

---

## Approach  
The problem is the classic **Maximum Subarray Sum** (Kadane’s algorithm).  
The idea is to scan the array once while maintaining two values:

1. `current` – the maximum sum of a subarray that **ends** at the current position.  
   - Either we extend the previous subarray (`current + A[i]`) or we start a new subarray at `i` (`A[i]`).  
2. `best` – the best (largest) sum seen so far among all `current` values.

While iterating:
- Update `current = max(A[i], current + A[i])`.
- Update `best = max(best, current)`.

At the end, `best` holds the answer.  
Because we only keep two integers, the memory usage stays constant.

**Step‑by‑step algorithm**
1. Read `N` and the array `A`.  
2. Initialise `current = best = A[0]`.  
3. For each index `i` from 1 to `N‑1`:
   - `current = max(A[i], current + A[i])`
   - `best = max(best, current)`
4. Output `best`.

The algorithm works for all‑negative arrays because `current` is allowed to start a new subarray at each element, guaranteeing that the largest single element is considered.

---

## Solution
```python
# ------------------------------------------------------------
# Maximum Subarray Sum (Kadane's algorithm)
# ------------------------------------------------------------
import sys

def max_subarray_sum(arr):
    """
    Returns the maximum sum of any non‑empty contiguous subarray.
    """
    # Initialise with the first element (handles all‑negative case)
    current = best = arr[0]

    # Process remaining elements
    for x in arr[1:]:
        # Either extend the previous subarray or start fresh at x
        current = max(x, current + x)
        # Keep the best value seen so far
        best = max(best, current)

    return best

def solve() -> None:
    data = sys.stdin.read().strip().split()
    if not data:
        return
    n = int(data[0])
    # Convert the next n tokens to integers
    a = list(map(int, data[1:1 + n]))

    # Compute and print the answer
    print(max_subarray_sum(a))

if __name__ == "__main__":
    solve()
```

---

## Complexity  
- **Time:** `O(N)` – each element is visited exactly once.  
- **Space:** `O(1)` – only a few integer variables are used regardless of `N`.

---

## Key Insight  
Kadane’s algorithm keeps the best subarray sum ending at each position, allowing a linear‑time solution.