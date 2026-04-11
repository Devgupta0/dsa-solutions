## Problem
Given two strings `s` and `t`, find the minimum window in `s` that contains all characters of `t`. This minimum window is the shortest substring of `s` that includes all characters of `t`. If there are multiple such windows, return the one that appears first in `s`.

## Examples
- Input: `s = "ADOBECODEBANC", t = "ABC"` Output: `"BANC"`
- Input: `s = "a", t = "aa"` Output: `""` (since there's no substring in `s` that contains all characters of `t`)
- Input: `s = "aa", t = "aa"` Output: `"aa"`

## Approach
To solve this problem, we'll use the sliding window technique. The idea is to maintain a window in `s` that includes all characters of `t`. We'll start with an empty window and expand it to the right until we include all characters of `t`. Once we have a valid window, we'll try to minimize it by moving the left boundary of the window to the right while still keeping all characters of `t`.

Here's the step-by-step process:
1. Create a dictionary to store the frequency of characters in `t`.
2. Initialize two pointers, `left` and `right`, to the start of `s`. `left` will be the start of the window, and `right` will be the end.
3. Expand the window to the right by moving `right` until we include all characters of `t`.
4. Once we have a valid window, try to minimize it by moving `left` to the right while still keeping all characters of `t`.
5. Keep track of the minimum window found so far.

## Solution
```python
from collections import defaultdict

def minWindow(s: str, t: str) -> str:
    # Base case: if string `t` is longer than string `s`, return empty string
    if len(t) > len(s):
        return ""

    # Create a dictionary to store the frequency of characters in `t`
    t_count = defaultdict(int)
    for char in t:
        t_count[char] += 1

    # Initialize variables to keep track of the minimum window
    min_window = ""
    min_length = float('inf')

    # Initialize variables to keep track of the current window
    left = 0
    formed = 0

    # Create a dictionary to store the frequency of characters in the current window
    window_counts = defaultdict(int)

    # Expand the window to the right
    for right in range(len(s)):
        character = s[right]
        window_counts[character] += 1

        # If the frequency of the current character in the window is equal to the frequency in `t`,
        # increment the `formed` count
        if character in t_count and window_counts[character] == t_count[character]:
            formed += 1

        # While the window is valid and the left pointer is not at the start of the string,
        # try to minimize the window
        while left <= right and formed == len(t_count):
            character = s[left]

            # If the current window is smaller than the minimum window found so far, update the minimum window
            if right - left + 1 < min_length:
                min_length = right - left + 1
                min_window = s[left:right + 1]

            # Move the left pointer to the right
            window_counts[character] -= 1
            if character in t_count and window_counts[character] < t_count[character]:
                formed -= 1

            left += 1

    return min_window
```

## Complexity
- Time: O(|s| + |t|) — The time complexity is linear with respect to the lengths of the strings `s` and `t`. We make a single pass over `s` and a single pass over `t` to create the frequency dictionary.
- Space: O(|s| + |t|) — The space complexity is also linear with respect to the lengths of the strings `s` and `t`. We use dictionaries to store the frequency of characters in `t` and in the current window, which can contain at most `|s| + |t|` characters.

## Key Insight
The core trick to solving this problem is to use the sliding window technique and maintain a dictionary to keep track of the frequency of characters in the current window, allowing us to efficiently determine whether the window contains all characters of `t`.