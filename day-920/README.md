## Problem
Given two strings `s` and `t`, find the minimum window in `s` that contains all characters of `t`. This problem is a classic example of a string matching problem and requires an efficient solution to handle large inputs.

## Examples
* Input: `s = "ADOBECODEBANC", t = "ABC"` Output: `"BANC"`
* Input: `s = "a", t = "a"` Output: `"a"`
* Input: `s = "aa", t = "aa"` Output: `"aa"`

## Approach
To solve this problem, we can use a sliding window approach. The idea is to maintain a window of characters in `s` and keep expanding it to the right until we find all characters of `t`. We then try to minimize the window by moving the left end to the right. The key here is to keep track of the characters in `t` that we have seen so far in the window.

Here are the steps to solve the problem:
1. Create a dictionary to store the frequency of characters in `t`.
2. Initialize two pointers, `left` and `right`, to the start of `s`.
3. Expand the window to the right by moving `right` and update the frequency of characters in the window.
4. When we have seen all characters of `t`, try to minimize the window by moving `left` to the right.
5. Keep track of the minimum window seen so far.

## Solution
```python
def min_window(s: str, t: str) -> str:
    # Create a dictionary to store the frequency of characters in t
    t_freq = {}
    for char in t:
        if char in t_freq:
            t_freq[char] += 1
        else:
            t_freq[char] = 1

    # Initialize variables to keep track of the minimum window
    min_length = float('inf')
    min_window = ""
    left = 0
    formed = 0

    # Create a dictionary to store the frequency of characters in the window
    window_freq = {}

    # Expand the window to the right
    for right in range(len(s)):
        # Add the character at the right end to the window
        character = s[right]
        if character in t_freq:
            if character in window_freq:
                window_freq[character] += 1
            else:
                window_freq[character] = 1

            # If the frequency of the character in the window is equal to the frequency in t,
            # increment the formed count
            if window_freq[character] == t_freq[character]:
                formed += 1

        # Try to minimize the window
        while left <= right and formed == len(t_freq):
            # Update the minimum window if the current window is smaller
            if right - left + 1 < min_length:
                min_length = right - left + 1
                min_window = s[left:right + 1]

            # Remove the character at the left end from the window
            character = s[left]
            if character in t_freq:
                # Decrement the frequency of the character in the window
                window_freq[character] -= 1

                # If the frequency of the character in the window is less than the frequency in t,
                # decrement the formed count
                if window_freq[character] < t_freq[character]:
                    formed -= 1

            # Move the left end to the right
            left += 1

    return min_window
```

## Complexity
- Time: O(|s| + |t|) — The time complexity is linear because we are making a single pass over both strings.
- Space: O(|s| + |t|) — The space complexity is also linear because we are storing the frequency of characters in both strings.

## Key Insight
The core trick to solving this problem is to maintain a sliding window and keep track of the characters in `t` that we have seen so far in the window, allowing us to efficiently find the minimum window that contains all characters of `t`.