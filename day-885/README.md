## Problem
The Minimum Window Substring problem is a classic problem in computer science where you are given two strings, `s` and `t`, and you need to find the minimum window in `s` that contains all characters of `t`. The characters in the window do not need to be in the same order as `t`. If there is no such window, you should return an empty string.

## Examples
- Input: `s = "ADOBECODEBANC", t = "ABC"` Output: `"BANC"`
- Input: `s = "a", t = "a"` Output: `"a"`
- Input: `s = "a", t = "aa"` Output: `""`

## Approach
To solve this problem, we can use the sliding window technique. The idea is to maintain a window of characters in `s` and keep track of the characters in `t` that we have seen so far. We can use a dictionary to store the frequency of characters in `t` and another dictionary to store the frequency of characters in the current window.

Here are the steps:
1. Create a dictionary `t_count` to store the frequency of characters in `t`.
2. Create a dictionary `window_count` to store the frequency of characters in the current window.
3. Initialize two pointers, `left` and `right`, to the start of `s`.
4. Expand the window to the right by moving the `right` pointer and update `window_count`.
5. If the window contains all characters of `t`, try to shrink the window by moving the `left` pointer to the right.
6. Keep track of the minimum window that contains all characters of `t`.

## Solution
```python
from collections import defaultdict

def min_window(s: str, t: str) -> str:
    # Create a dictionary to store the frequency of characters in t
    t_count = defaultdict(int)
    for char in t:
        t_count[char] += 1

    # Create a dictionary to store the frequency of characters in the current window
    window_count = defaultdict(int)

    # Initialize the minimum window
    min_window = ""
    min_window_len = float('inf')

    # Initialize the left and right pointers
    left = 0

    # Initialize the number of characters in t that we have seen so far
    required_chars = len(t_count)

    # Initialize the number of characters in the window that match t
    formed_chars = 0

    # Expand the window to the right
    for right in range(len(s)):
        # Add the character at the right pointer to the window
        char = s[right]
        window_count[char] += 1

        # If the character is in t and its frequency in the window is equal to its frequency in t,
        # increment the number of formed characters
        if char in t_count and window_count[char] == t_count[char]:
            formed_chars += 1

        # While the window contains all characters of t and the left pointer is less than the right pointer,
        # try to shrink the window
        while left <= right and formed_chars == required_chars:
            # Update the minimum window if the current window is smaller
            if right - left + 1 < min_window_len:
                min_window = s[left:right + 1]
                min_window_len = right - left + 1

            # Remove the character at the left pointer from the window
            char = s[left]
            window_count[char] -= 1

            # If the character is in t and its frequency in the window is less than its frequency in t,
            # decrement the number of formed characters
            if char in t_count and window_count[char] < t_count[char]:
                formed_chars -= 1

            # Move the left pointer to the right
            left += 1

    return min_window
```

## Complexity
- Time: O(|s| + |t|) — The time complexity is linear because we are scanning `s` and `t` once. The while loop inside the for loop does not change the overall time complexity because each character in `s` is visited at most twice.
- Space: O(|s| + |t|) — The space complexity is linear because in the worst case, we might need to store all characters of `s` and `t` in the dictionaries.

## Key Insight
The core trick to solve this problem is to use the sliding window technique and maintain two dictionaries to store the frequency of characters in `t` and the current window, allowing us to efficiently expand and shrink the window to find the minimum window that contains all characters of `t`.