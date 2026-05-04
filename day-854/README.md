## Problem
Given two strings `s` and `t`, find the minimum length substring of `s` that contains all characters of `t`. This problem is a classic example of the sliding window technique, where we need to find the smallest window in `s` that contains all characters of `t`.

## Examples
* Input: `s = "ADOBECODEBANC", t = "ABC"` Output: `"BANC"`
* Input: `s = "a", t = "a"` Output: `"a"`
* Input: `s = "bba", t = "ab"` Output: `"ba"`

## Approach
The approach to solve this problem is to use the sliding window technique. We start by creating a dictionary to store the frequency of characters in string `t`. Then, we initialize two pointers, `left` and `right`, to the start of string `s`. We move the `right` pointer to the right and add the characters to the window until we have all characters of `t` in the window. Once we have all characters, we try to minimize the window by moving the `left` pointer to the right.

Here are the steps:
1. Create a dictionary to store the frequency of characters in string `t`.
2. Initialize two pointers, `left` and `right`, to the start of string `s`.
3. Move the `right` pointer to the right and add the characters to the window until we have all characters of `t` in the window.
4. Once we have all characters, try to minimize the window by moving the `left` pointer to the right.
5. Update the minimum length substring if the current window is smaller.

## Solution
```python
from collections import defaultdict

def min_window(s: str, t: str) -> str:
    # Create a dictionary to store the frequency of characters in string t
    t_count = defaultdict(int)
    for char in t:
        t_count[char] += 1
    
    # Initialize variables
    required_chars = len(t_count)
    left = 0
    min_length = float('inf')
    min_window = ""
    formed_chars = 0
    
    # Create a dictionary to store the frequency of characters in the window
    window_counts = defaultdict(int)
    
    # Move the right pointer to the right
    for right in range(len(s)):
        character = s[right]
        window_counts[character] += 1
        
        # If the character is in t and its frequency in the window is equal to its frequency in t,
        # increment the formed_chars count
        if character in t_count and window_counts[character] == t_count[character]:
            formed_chars += 1
        
        # While the window contains all characters of t and the left pointer is not at the start of the window,
        # try to minimize the window
        while left <= right and formed_chars == required_chars:
            character = s[left]
            
            # Update the minimum length substring if the current window is smaller
            if right - left + 1 < min_length:
                min_length = right - left + 1
                min_window = s[left:right + 1]
            
            # Move the left pointer to the right
            window_counts[character] -= 1
            if character in t_count and window_counts[character] < t_count[character]:
                formed_chars -= 1
            left += 1
    
    return min_window
```

## Complexity
- Time: O(|s| + |t|) — where |s| is the length of string `s` and |t| is the length of string `t`. This is because we are scanning both strings once.
- Space: O(|s| + |t|) — where |s| is the length of string `s` and |t| is the length of string `t`. This is because in the worst case, we might need to store all characters of both strings in the dictionaries.

## Key Insight
The core trick to solve this problem is to use the sliding window technique and maintain two dictionaries to store the frequency of characters in string `t` and the current window, allowing us to efficiently track the formation of the required substring.