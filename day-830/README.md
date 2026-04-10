## Problem
Given two strings, `s` and `t`, find the minimum length substring of `s` that contains all characters of `t`. If no such substring exists, return an empty string. This problem is a classic example of a string matching problem that can be solved using the sliding window technique.

## Examples
* Input: `s = "ADOBECODEBANC", t = "ABC"` Output: `"BANC"`
* Input: `s = "a", t = "a"` Output: `"a"`
* Input: `s = "a", t = "b"` Output: `""`

## Approach
To solve this problem, we can use the sliding window technique. The idea is to maintain a window of characters in `s` that contains all characters of `t`. We start by creating a dictionary to store the frequency of characters in `t`. Then, we initialize two pointers, `left` and `right`, to the start of `s`. We expand the window to the right by moving the `right` pointer and update the frequency of characters in the window. When the window contains all characters of `t`, we try to minimize the window by moving the `left` pointer to the right. We keep track of the minimum length substring that contains all characters of `t`.

Here are the steps:
1. Create a dictionary to store the frequency of characters in `t`.
2. Initialize two pointers, `left` and `right`, to the start of `s`.
3. Expand the window to the right by moving the `right` pointer and update the frequency of characters in the window.
4. When the window contains all characters of `t`, try to minimize the window by moving the `left` pointer to the right.
5. Keep track of the minimum length substring that contains all characters of `t`.

## Solution
```python
from collections import defaultdict

def min_window(s: str, t: str) -> str:
    # Create a dictionary to store the frequency of characters in t
    t_count = defaultdict(int)
    for char in t:
        t_count[char] += 1
    
    # Initialize variables to keep track of the minimum window
    min_length = float('inf')
    min_window = ""
    left = 0
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
        
        # While the window contains all characters of t and the left pointer is not at the start of the window,
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
- Time: O(|s| + |t|) — The time complexity is linear with respect to the lengths of the strings `s` and `t` because we make a single pass over each string.
- Space: O(|s| + |t|) — The space complexity is also linear with respect to the lengths of the strings `s` and `t` because in the worst case, we might need to store all characters of `s` and `t` in the `window_counts` and `t_count` dictionaries.

## Key Insight
The core trick to solve this problem is to use the sliding window technique to maintain a window of characters in `s` that contains all characters of `t`, and then try to minimize the window by moving the left pointer to the right.