# 📄 README – Custom Problem  

## Problem  
You are given an array `A` of `N` integers and an integer `limit`.  
Find the length of the longest **contiguous** sub‑array such that the difference between the maximum and minimum elements in that sub‑array is **≤ limit**.

Formally, for a sub‑array `A[l … r]` (0‑based indices) let  

```
mx = max{ A[i] | l ≤ i ≤ r }
mn = min{ A[i] | l ≤ i ≤ r }
```

The sub‑array is **valid** if `mx - mn ≤ limit`.  
Return the maximum possible length `r‑l+1` among all valid sub‑arrays.  
If no such sub‑array exists (i.e., `N = 0`), return `0`.

**Input constraints (typical for an online judge)**  

* `1 ≤ N ≤ 10⁵`  
* `-10⁹ ≤ A[i] ≤ 10⁹`  
* `0 ≤ limit ≤ 2·10⁹`  

The algorithm must run in **O(N)** time.

---

## Examples  

| # | Input                              | Output | Explanation |
|---|------------------------------------|--------|-------------|
| 1 | `A = [8, 2, 4, 7]`, `limit = 4`    | `2`    | The longest valid sub‑arrays are `[2,4]` and `[4,7]` (both length 2). |
| 2 | `A = [10, 1, 2, 4, 7, 2]`, `limit = 5` | `4`    | Sub‑array `[2,4,7,2]` has max = 7, min = 2, diff = 5. |
| 3 | `A = [4, 2, 2, 2, 4, 4, 2, 2]`, `limit = 0` | `3` | All elements must be equal; the longest run of equal numbers is three (`[2,2,2]`). |

---

## Approach  

The condition `max - min ≤ limit` depends only on the current window’s extreme values.  
We can maintain a **sliding window** `[left, right]` and expand `right` one step at a time.  

To know the current maximum and minimum in **O(1)** we keep two **monotonic deques**:

* `maxDeque` – stores indices in decreasing order of `A[i]`. The front always holds the index of the current maximum.
* `minDeque` – stores indices in increasing order of `A[i]`. The front always holds the index of the current minimum.

When we add a new element `A[right]`:

1. Pop from the back of `maxDeque` while `A[right]` is larger – the smaller values can never become maximum again.
2. Push `right` to `maxDeque`.  
   Do the symmetric operations for `minDeque`.

Now the window `[left, right]` is valid **iff**  

```
A[maxDeque[0]] - A[minDeque[0]] ≤ limit
```

If it becomes invalid, we need to shrink the window from the left:

* Increment `left`.
* Remove `left‑1` from the fronts of the deques if they point to that index.

While shrinking, keep checking the validity condition.  
Whenever the window is valid we update the answer with its length `right - left + 1`.

Because each index is inserted and removed from each deque at most once, the total work is linear.

### Step‑by‑step algorithm  

1. Initialise `left = 0`, `answer = 0`, empty `maxDeque`, `minDeque`.
2. Loop `right` from `0` to `N‑1`  
   a. Insert `right` into both deques as described.  
   b. While the window is invalid (`A[maxDeque[0]] - A[minDeque[0]] > limit`):  
      * If `maxDeque[0] == left` → pop it from the front.  
      * If `minDeque[0] == left` → pop it from the front.  
      * `left += 1`.  
   c. After the while‑loop the window is valid; update `answer = max(answer, right - left + 1)`.
3. Output `answer`.

---

## Solution  

```python
#!/usr/bin/env python3
"""
Longest sub‑array with limited max‑min difference.

The implementation follows the monotonic‑deque sliding‑window approach
described in the README.
"""

from collections import deque
import sys

def longest_subarray(A, limit):
    """
    Returns the maximum length of a contiguous sub‑array where
    max(A[l..r]) - min(A[l..r]) <= limit.
    """
    max_d = deque()   # indices, decreasing values
    min_d = deque()   # indices, increasing values
    left = 0
    best = 0

    for right, val in enumerate(A):
        # maintain max deque (decreasing)
        while max_d and A[max_d[-1]] < val:
            max_d.pop()
        max_d.append(right)

        # maintain min deque (increasing)
        while min_d and A[min_d[-1]] > val:
            min_d.pop()
        min_d.append(right)

        # shrink window until condition holds
        while A[max_d[0]] - A[min_d[0]] > limit:
            # move left forward
            if max_d[0] == left:
                max_d.popleft()
            if min_d[0] == left:
                min_d.popleft()
            left += 1

        # update answer
        best = max(best, right - left + 1)

    return best

def solve() -> None:
    """
    Reads from standard input.
    Input format (typical for contests):
        N limit
        A1 A2 ... AN
    Prints the answer.
    """
    data = sys.stdin.read().strip().split()
    if not data:
        return
    it = iter(data)
    N = int(next(it))
    limit = int(next(it))
    A = [int(next(it)) for _ in range(N)]
    print(longest_subarray(A, limit))

if __name__ == "__main__":
    solve()
```

**Explanation of the code**  

* `max_d` and `min_d` are the two monotonic deques.  
* The `while` loops inside the main `for` maintain the monotonic property.  
* The inner `while` shrinks the window only when the current extremes violate the `limit`.  
* `best` holds the maximal length seen so far.

---

## Complexity  

* **Time:** `O(N)` – each array element is inserted into and removed from each deque at most once, giving linear work.  
* **Space:** `O(N)` in the worst case for the deques, but because each index appears only once, the extra space is `O(N)` (actually `O(window size) ≤ O(N)`).

---

## Key Insight  

Maintain the current window’s maximum and minimum with two monotonic deques; this lets us slide the window in linear time while always knowing whether the `max‑min ≤ limit` condition holds.