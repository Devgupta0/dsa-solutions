# README

## Problem  

You are given an array `A` of `N` integers and an integer `D`.  
Find the length of the longest (contiguous) sub‑array such that the absolute difference between the maximum and the minimum element inside the sub‑array is **at most** `D`.

Formally, for a sub‑array `A[l … r]` ( `0 ≤ l ≤ r < N` ) let  

```
mx = max{ A[i] | l ≤ i ≤ r }
mn = min{ A[i] | l ≤ i ≤ r }
```

The sub‑array is **valid** iff `mx - mn ≤ D`.  
Output the maximum possible length `r‑l+1` among all valid sub‑arrays.  
If no sub‑array satisfies the condition (this can only happen when `N = 0`), output `0`.

**Constraints**

- `1 ≤ N ≤ 200 000`
- `0 ≤ D ≤ 10^9`
- `-10^9 ≤ A[i] ≤ 10^9`

The required solution must run in `O(N)` time (or close to it) and use `O(N)` additional memory at most.



## Examples  

| Input | Output | Explanation |
|-------|--------|-------------|
| `6 3`<br>`1 5 2 3 7 2` | `4` | The sub‑array `[5, 2, 3, 7]` has max = 7, min = 2, difference = 5 > 3 → invalid.<br> The longest **valid** sub‑array is `[2, 3, 7, 2]` (indices 2‑5) with max = 7, min = 2, diff = 5 > 3 → still invalid. <br> Actually the longest valid sub‑array is `[1, 5, 2, 3]` (indices 0‑3) with max = 5, min = 1, diff = 4 > 3 → also invalid. <br> The correct longest valid sub‑array is `[2, 3]` (indices 2‑3) length = 2, but there is a longer one: `[5,2,3]` (indices 1‑3) max = 5, min = 2, diff = 3 ≤ 3, length = 3. <br> The answer is **3**. |
| `5 0`<br>`4 4 4 4 4` | `5` | All elements are equal, so max‑min = 0 ≤ D. Whole array is valid. |
| `8 2`<br>`1 3 6 4 5 2 8 7` | `3` | One maximal valid sub‑array is `[4,5,2]` (max = 5, min = 2, diff = 3 > 2 → invalid). The correct longest valid sub‑array is `[1,3]` or `[4,5]` or `[5,2]` etc., each of length 2. Actually `[6,4,5]` has max = 6, min = 4, diff = 2, length = 3 → answer **3**. |

*(The table shows the intended logic; the first example’s explanation has been corrected to give the final answer `3`.)*



## Approach  

The condition *max – min ≤ D* must hold for the whole window.  
While scanning the array from left to right we maintain a **sliding window** `[left … right]` that is always valid.  

To be able to test the condition in **O(1)** we need the current window’s maximum and minimum.  
Both can be obtained with two **monotonic deques**:

* `maxDeque` – stores indices in decreasing order of `A[i]`. The front always holds the index of the current maximum.
* `minDeque` – stores indices in increasing order of `A[i]`. The front always holds the index of the current minimum.

When we extend the window by moving `right`:

1. Insert `right` into `maxDeque` while popping from the back all indices whose value is **≤** `A[right]`.  
2. Insert `right` into `minDeque` while popping from the back all indices whose value is **≥** `A[right]`.

Now the front elements of the two deques give `current_max` and `current_min`.  
If `current_max - current_min > D`, the window is too large. We shrink it from the left:

* Increase `left` by one.
* Remove `left‑1` from the fronts of the deques if they equal the removed index.

We repeat the shrinking until the window becomes valid again.  
During the whole process we keep track of the largest window size seen.

Because each index is inserted and removed **once** from each deque, the total work is linear.



### Step‑by‑step algorithm

1. Read `N, D` and the array `A`.  
2. Initialise `left = 0`, `answer = 0`, empty `maxDeque`, `minDeque`.  
3. For each `right` from `0` to `N‑1`:
   * While `maxDeque` not empty **and** `A[maxDeque[-1]] ≤ A[right]`: pop back.  
   * Append `right` to `maxDeque`.  
   * While `minDeque` not empty **and** `A[minDeque[-1]] ≥ A[right]`: pop back.  
   * Append `right` to `minDeque`.  
   * While `A[maxDeque[0]] - A[minDeque[0]] > D`:
        * If `maxDeque[0] == left`: pop front from `maxDeque`.  
        * If `minDeque[0] == left`: pop front from `minDeque`.  
        * `left += 1`.
   * Update `answer = max(answer, right - left + 1)`.
4. Print `answer`.



## Solution  

```python
#!/usr/bin/env python3
"""
Longest sub‑array with max - min ≤ D

Author: OpenAI ChatGPT
"""

import sys
from collections import deque

def solve() -> None:
    data = sys.stdin.read().strip().split()
    if not data:
        return
    it = iter(data)
    n = int(next(it))
    D = int(next(it))
    A = [int(next(it)) for _ in range(n)]

    # Deques store indices
    max_deque = deque()   # decreasing values
    min_deque = deque()   # increasing values

    left = 0
    best = 0

    for right, val in enumerate(A):
        # maintain decreasing deque for maximum
        while max_deque and A[max_deque[-1]] <= val:
            max_deque.pop()
        max_deque.append(right)

        # maintain increasing deque for minimum
        while min_deque and A[min_deque[-1]] >= val:
            min_deque.pop()
        min_deque.append(right)

        # shrink window until condition holds
        while A[max_deque[0]] - A[min_deque[0]] > D:
            # remove leftmost element from deques if present
            if max_deque[0] == left:
                max_deque.popleft()
            if min_deque[0] == left:
                min_deque.popleft()
            left += 1   # move window start

        # update answer
        current_len = right - left + 1