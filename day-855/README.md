## Problem
Given two strings, `s` and `t`, find the minimum length substring of `s` that contains all characters of `t`, with the condition that each character in `t` must appear at least once in the substring. This is known as the Minimum Window Substring problem.

## Examples
* Input: `s = "ADOBECODEBANC", t = "ABC"` Output: `"BANC"`
* Input: `s = "a", t = "a"` Output: `"a"`
* Input: `s = "aa", t = "aa"` Output: `"aa"`

## Approach
To solve this problem, we can use the sliding window technique. The algorithm works by maintaining a window of characters in `s` that contains all characters of `t`. We start with an empty window and expand it to the right until we find all characters of `t`. Then, we try to shrink the window from the left while still keeping all characters of `t` in the window. The minimum length window that contains all characters of `t` is our answer.

Here are the steps:
1. Create a dictionary to store the frequency of characters in `t`.
2. Initialize two pointers, `left` and `right`, to represent the sliding window.
3. Expand the window to the right by moving `right` pointer until we find all characters of `t`.
4. Once we have all characters of `t` in the window, try to shrink the window from the left by moving `left` pointer.
5. Update the minimum length window if the current window is smaller.
6. Repeat steps 3-5 until we have checked all possible windows.

## Solution
```python
from collections import defaultdict

def min_window(s: str, t: str) -> str:
    # Create a dictionary to store the frequency of characters in t
    t_count = defaultdict(int)
    for char in t:
        t_count[char] += 1

    # Initialize variables
    left = 0
    min_len = float('inf')
    min_window = ""
    formed = 0

    # Create a dictionary to store the frequency of characters in the window
    window_counts = defaultdict(int)

    # Expand the window to the right
    for right in range(len(s)):
        character = s[right]
        window_counts[character] += 1

        # If the character is in t and its frequency in the window is equal to its frequency in t,
        # increment the formed variable
        if character in t_count and window_counts[character] == t_count[character]:
            formed += 1

        # While the window contains all characters of t, try to shrink the window
        while left <= right and formed == len(t_count):
            character = s[left]

            # Update the minimum length window if the current window is smaller
            if right - left + 1 < min_len:
                min_len = right - left + 1
                min_window = s[left:right + 1]

            # Shrink the window from the left
            window_counts[character] -= 1
            if character in t_count and window_counts[character] < t_count[character]:
                formed -= 1

            left += 1

    return min_window
```

## Complexity
- Time: O(|s| + |t|) — The time complexity is linear because we are scanning the string `s` and `t` once. The while loop inside the for loop might seem like it would increase the time complexity, but since each character in `s` is visited at most twice (once by `right` pointer and once by `left` pointer), the total time complexity remains linear.
- Space: O(|s| + |t|) — The space complexity is linear because in the worst case, we might need to store all characters of `s` and `t` in the dictionaries.

## Key Insight
The core trick to solve this problem is to use the sliding window technique and maintain two dictionaries to store the frequency of characters in `t` and the current window, which allows us to efficiently check if the window contains all characters of `t`.