## Problem
Given two strings `s` and `t`, find the minimum length substring of `s` that contains all characters of `t`. Each character in the substring can be used only as many times as it appears in `t`. This is a classic problem of finding the minimum window substring that satisfies the given conditions.

## Examples
* Input: `s = "ADOBECODEBANC", t = "ABC"` Output: `"BANC"`
* Input: `s = "a", t = "a"` Output: `"a"`
* Input: `s = "aa", t = "aa"` Output: `"aa"`

## Approach
The algorithm used to solve this problem is based on the sliding window technique. The idea is to maintain a window of characters in `s` that contains all characters of `t`. We start by creating a frequency dictionary of characters in `t`. Then, we initialize two pointers, `left` and `right`, to the beginning of `s`. We move the `right` pointer to the right and add characters to the window until we have all characters of `t`. Once we have all characters, we try to minimize the window by moving the `left` pointer to the right.

Here are the steps:
1. Create a frequency dictionary of characters in `t`.
2. Initialize two pointers, `left` and `right`, to the beginning of `s`.
3. Move the `right` pointer to the right and add characters to the window until we have all characters of `t`.
4. Once we have all characters, try to minimize the window by moving the `left` pointer to the right.
5. Keep track of the minimum length substring that contains all characters of `t`.

## Solution
```python
from collections import defaultdict

def min_window(s: str, t: str) -> str:
    # Create a frequency dictionary of characters in t
    t_count = defaultdict(int)
    for char in t:
        t_count[char] += 1

    # Initialize variables
    left = 0
    min_len = float('inf')
    min_str = ""
    formed = 0

    # Create a frequency dictionary of characters in the window
    window_counts = defaultdict(int)

    # Traverse the string s
    for right in range(len(s)):
        character = s[right]
        window_counts[character] += 1

        # If the character is in t and its frequency in the window is equal to its frequency in t,
        # increment the formed variable
        if character in t_count and window_counts[character] == t_count[character]:
            formed += 1

        # While the window contains all characters of t and the left pointer is not at the beginning of the window,
        # try to minimize the window
        while left <= right and formed == len(t_count):
            character = s[left]

            # Update the minimum length substring if the current window is smaller
            if right - left + 1 < min_len:
                min_len = right - left + 1
                min_str = s[left:right + 1]

            # Remove the character at the left pointer from the window
            window_counts[character] -= 1

            # If the character is in t and its frequency in the window is less than its frequency in t,
            # decrement the formed variable
            if character in t_count and window_counts[character] < t_count[character]:
                formed -= 1

            # Move the left pointer to the right
            left += 1

    # Return the minimum length substring
    return min_str if min_len != float('inf') else ""
```

## Complexity
- Time: O(|s| + |t|) — The time complexity is linear because we are traversing the strings `s` and `t` once. The while loop inside the for loop does not change the overall time complexity because each character in `s` is visited at most twice (once by the `right` pointer and once by the `left` pointer).
- Space: O(|s| + |t|) — The space complexity is also linear because we are storing the frequency dictionaries of characters in `s` and `t`.

## Key Insight
The core trick to solve this problem is to use the sliding window technique to maintain a window of characters in `s` that contains all characters of `t`, and to minimize the window by moving the `left` pointer to the right when all characters of `t` are found in the window.