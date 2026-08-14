## Problem
Given two strings `s` and `t`, find the minimum length substring of `s` that contains all characters from `t`. If no such substring exists, return an empty string. The characters in the substring do not need to be in the same order as they appear in `t`, but each character in `t` must appear at least once in the substring.

## Examples
- Input: `s = "ADOBECODEBANC", t = "ABC"` Output: `"BANC"`
- Input: `s = "a", t = "a"` Output: `"a"`
- Input: `s = "a", t = "aa"` Output: `""`

## Approach
To solve this problem, we will use a sliding window approach. The idea is to maintain a window of characters in `s` that contains all characters from `t`. We start by creating a dictionary to store the frequency of characters in `t`. Then we initialize two pointers, `left` and `right`, to represent the sliding window. We expand the window to the right until we have all characters from `t` in the window, and then we try to shrink the window from the left until we no longer have all characters from `t`. We keep track of the minimum length substring that contains all characters from `t`.

Here are the steps:
1. Create a dictionary to store the frequency of characters in `t`.
2. Initialize two pointers, `left` and `right`, to represent the sliding window.
3. Expand the window to the right until we have all characters from `t` in the window.
4. Try to shrink the window from the left until we no longer have all characters from `t`.
5. Keep track of the minimum length substring that contains all characters from `t`.

## Solution
```python
def minWindow(s: str, t: str) -> str:
    # Create a dictionary to store the frequency of characters in t
    t_count = {}
    for char in t:
        if char in t_count:
            t_count[char] += 1
        else:
            t_count[char] = 1

    # Initialize variables to keep track of the minimum window
    left = 0
    min_len = float('inf')
    min_window = ""

    # Initialize variables to keep track of the characters in the current window
    window_count = {}
    required_chars = len(t_count)
    formed_chars = 0

    # Iterate over the string s
    for right in range(len(s)):
        # Add the current character to the window
        character = s[right]
        if character in t_count:
            window_count[character] = window_count.get(character, 0) + 1
            if window_count[character] == t_count[character]:
                formed_chars += 1

        # Try to shrink the window from the left
        while left <= right and formed_chars == required_chars:
            character = s[left]

            # Update the minimum window if the current window is smaller
            if right - left + 1 < min_len:
                min_len = right - left + 1
                min_window = s[left:right + 1]

            # Remove the leftmost character from the window
            if character in t_count:
                window_count[character] -= 1
                if window_count[character] < t_count[character]:
                    formed_chars -= 1
            left += 1

    return min_window
```

## Complexity
- Time: O(|s| + |t|) — The time complexity is linear because we are scanning the string `s` once and the string `t` once to create the frequency dictionary.
- Space: O(|s| + |t|) — The space complexity is also linear because in the worst case, the size of the window can be equal to the size of `s`, and we need to store the frequency dictionary for `t`.

## Key Insight
The core trick to solve this problem is to use a sliding window approach and maintain a dictionary to keep track of the frequency of characters in the current window.