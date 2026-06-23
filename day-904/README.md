## Problem
The Minimum Window Substring problem is a classic string matching problem where we need to find the minimum window (substring) in a given string that contains all unique characters of another string. The window can be any substring of the main string and must contain all unique characters of the given string.

## Examples
* Input: `s = "ADOBECODEBANC", t = "ABC"` Output: `"BANC"`
* Input: `s = "a", t = "a"` Output: `"a"`
* Input: `s = "aa", t = "aa"` Output: `"aa"`

## Approach
The algorithm used to solve this problem is based on the sliding window technique. In plain English, we maintain a window of characters in the main string and keep expanding it to the right until we have all the unique characters of the given string. Then, we start contracting the window from the left until we no longer have all the unique characters. We keep track of the minimum window size and update it whenever we find a smaller window that contains all the unique characters.

Here are the steps:
1. Create a dictionary to store the frequency of each character in the given string.
2. Initialize two pointers, `left` and `right`, to the start of the main string.
3. Initialize a dictionary to store the frequency of each character in the current window.
4. Initialize a variable to store the minimum window size and the corresponding window.
5. Expand the window to the right by moving the `right` pointer until we have all the unique characters of the given string.
6. Contract the window from the left by moving the `left` pointer until we no longer have all the unique characters.
7. Update the minimum window size and the corresponding window whenever we find a smaller window.

## Solution
```python
from collections import defaultdict

def min_window(s: str, t: str) -> str:
    # Base case
    if not s or not t:
        return ""

    # Create a dictionary to store the frequency of each character in the given string
    t_count = defaultdict(int)
    for char in t:
        t_count[char] += 1

    # Initialize variables
    required_chars = len(t_count)
    left = 0
    min_len = float('inf')
    min_window = ""

    # Initialize a dictionary to store the frequency of each character in the current window
    window_counts = defaultdict(int)
    formed_chars = 0

    # Expand the window to the right
    for right in range(len(s)):
        character = s[right]
        window_counts[character] += 1

        # If the character is in the given string and its frequency in the window is equal to its frequency in the given string
        if character in t_count and window_counts[character] == t_count[character]:
            formed_chars += 1

        # While we have all the unique characters and the window is not empty
        while left <= right and formed_chars == required_chars:
            character = s[left]

            # Update the minimum window size and the corresponding window
            if right - left + 1 < min_len:
                min_len = right - left + 1
                min_window = s[left:right + 1]

            # Contract the window from the left
            window_counts[character] -= 1
            if character in t_count and window_counts[character] < t_count[character]:
                formed_chars -= 1

            # Move the left pointer to the right
            left += 1

    return min_window
```

## Complexity
- Time: O(|s| + |t|) — The time complexity is linear with respect to the size of the main string `s` and the given string `t`. This is because we are iterating over both strings once.
- Space: O(|s| + |t|) — The space complexity is also linear with respect to the size of the main string `s` and the given string `t`. This is because we are storing the frequency of each character in the given string and the current window.

## Key Insight
The core trick to solve this problem is to use the sliding window technique and maintain a dictionary to store the frequency of each character in the current window, allowing us to efficiently check if we have all the unique characters of the given string.