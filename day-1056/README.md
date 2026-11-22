## Problem  

You are given an array `A` of `N` integers and an integer `K`.  
A *good* sub‑array is a **contiguous** part of `A` whose maximum element and minimum element differ by at most `K` :

```
max(subarray) – min(subarray) ≤ K
```

Return the length of the longest good sub‑array.  
If no good sub‑array exists, return `0`.

**Input**

* `N` – the number of elements (`1 ≤ N ≤ 2·10⁵`)
* `K` – the allowed difference (`0 ≤ K ≤ 10⁹`)
* `A[0 … N‑1]` – the array (`|A[i]| ≤ 10⁹`)

**Output**  

A single integer – the maximum length of a good sub‑array.

---

## Examples  

| # | Input | Output | Explanation |
|---|-------|--------|-------------|
| 1 | `N = 7, K = 3`<br>`A = [1, 3, 6, 4, 2, 5, 7]` | `4` | The sub‑array `[3, 6, 4, 2]` has `max = 6`, `min = 2`, `6‑2 = 4 > 3` (not good). The longest good sub‑array is `[4, 2, 5, 7]`? actually check: `[4,2,5]` max‑min = 5‑2 = 3 ≤ 3, length = 3. Better: `[1,3]` length = 2. The longest good sub‑array is `[2,5,7]`? max‑min = 7‑2 =5 >3. The correct longest good sub‑array is `[1,3]` length = 2? Wait the answer is 4 according to example, so choose a different array: let's adjust. We'll give a consistent example below. |
| 2 | `N = 5, K = 0`<br>`A = [2, 2, 2, 2, 2]` | `5` | All elements are equal, difference is `0`. |
| 3 | `N = 6, K = 1`<br>`A = [4, 1, 2, 2, 5, 1]` | `3` | Sub‑array `[1,2,2]` (or `[2,2,5]` is not good). Length = 3. |

*Corrected Example 1*  

```
Input:
7 3
1 3 2 4 2 3 1
Output:
5
```

The longest good sub‑array is `[3,2,4,2,3]` (max = 4, min = 2, difference = 2 ≤ 3) → length = 5.

---

## Approach  

The condition `max – min ≤ K` depends only on the current window’s extreme values.  
We can slide a window `[L … R]` from left to right while maintaining the **maximum** and **minimum** of the window in *O(1)* time using two double‑ended queues (deques):

1. **maxDeque** – stores indices of elements in decreasing order; the front always holds the index of the current maximum.
2. **minDeque** – stores indices in increasing order; the front always holds the index of the current minimum.

While expanding the right end (`R`):
* Insert `A[R]` into both deques, removing from the back all elements that are smaller (for `maxDeque`) or larger (for `minDeque`) – this keeps the order property.
* After insertion, the front of each deque gives `currentMax` and `currentMin`.

If `currentMax – currentMin > K`, the window is invalid.  
We then move the left pointer `L` to the right, popping `L` from the front of any deque that holds it, until the condition becomes satisfied again.

During the process we keep the best length seen (`best = max(best, R‑L+1)`).

The algorithm visits each element at most twice (once when `R` moves forward, once when `L` moves forward), so it runs in linear time.

### Step‑by‑step  

1. Read `N`, `K`, and the array `A`.  
2. Initialise `L = 0`, `best = 0`, empty `maxDeque`, `minDeque`.  
3. For each `R` from `0` to `N‑1`  
   * While `maxDeque` not empty and `A[R] ≥ A[maxDeque.back]`: pop back.  
   * Append `R` to `maxDeque`.  
   * While `minDeque` not empty and `A[R] ≤ A[minDeque.back]`: pop back.  
   * Append `R` to `minDeque`.  
   * While `A[maxDeque.front] – A[minDeque.front] > K`  
        * If `maxDeque.front == L`: pop front.  
        * If `minDeque.front == L`: pop front.  
        * Increment `L`.  
   * Update `best = max(best, R‑L+1)`.  
4. Print `best`.

---

## Solution  

```python
#!/usr/bin/env python3
"""
Longest sub‑array where max – min ≤ K
Time   : O(N)
Space  : O(N) for the two deques (worst‑case)
"""

import sys
from collections import deque

def longest_good_subarray(arr, K):
    """Return length of the longest contiguous sub‑array with max-min ≤ K."""
    max_d = deque()   # stores indices, values decreasing
    min_d = deque()   # stores indices, values increasing
    left = 0
    best = 0

    for right, val in enumerate(arr):
        # maintain decreasing order for max deque
        while max_d and arr[max_d[-1]] <= val:
            max_d.pop()
        max_d.append(right)

        # maintain increasing order for min deque
        while min_d and arr[min_d[-1]] >= val:
            min_d.pop()
        min_d.append(right)

        # shrink window while condition violated
        while arr[max_d[0]] - arr[min_d[0]] > K:
            # move left border forward
            if max_d[0] == left:
                max_d.popleft()
            if min_d[0] == left:
                min_d.popleft()
            left += 1

        # window [left, right] is valid
        best = max(best, right - left + 1)

    return best

def main() -> None:
    data = sys.stdin.read().strip().split()
    if not data:
        return
    it = iter(data)
    N = int(next(it))
    K = int(next(it))
    A = [int(next(it)) for _ in range(N)]
    print(longest_good_subarray(A, K))

if __name__ == "__main__":
    main()
```

The program follows exactly the algorithm described above and conforms to the required input / output format.

---

## Complexity  

- **Time:** `O(N)` – each array element is inserted into and removed from each deque at most once, and the left pointer `L` moves forward at most `N` times.  
- **Space:** `O(N)` in the worst case (when the deques store almost the whole array), but on average they contain only the current window’s extremes.

---

## Key Insight  

Maintain the current window’s maximum and minimum with two monotonic deques; this lets us expand and shrink the window in linear time while always checking the `max‑min ≤ K` condition.