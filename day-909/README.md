## Problem
Given two strings `s` and `t`, find the minimum length substring of `s` that contains all characters of `t`. The characters in the substring can appear any number of times in any order. If no such substring exists, return an empty string.

## Examples
* Input: `s = "ADOBECODEBANC", t = "ABC"` Output: `"BANC"`
* Input: `s = "a", t = "a"` Output: `"a"`
* Input: `s = "a", t = "aa"` Output: `""`

## Approach
To solve this problem, we use the sliding window technique. The idea is to maintain a window of characters in `s` that contains all characters of `t`. We start by creating a dictionary to store the frequency of characters in `t`. Then, we initialize two pointers, `left` and `right`, to represent the start and end of the window. We move the `right` pointer to the right and add characters to the window until we have all characters of `t`. Once we have all characters, we try to minimize the window by moving the `left` pointer to the right.

Here's a step-by-step breakdown:
1. Create a dictionary to store the frequency of characters in `t`.
2. Initialize two pointers, `left` and `right`, to 0.
3. Initialize a dictionary to store the frequency of characters in the current window.
4. Initialize a variable to store the minimum length of the substring.
5. Move the `right` pointer to the right and add characters to the window until we have all characters of `t`.
6. Once we have all characters, try to minimize the window by moving the `left` pointer to the right.
7. Update the minimum length of the substring if a smaller window is found.

## Solution
```python
from collections import defaultdict

def min_window(s, t):
    # Create a dictionary to store the frequency of characters in t
    t_count = defaultdict(int)
    for char in t:
        t_count[char] += 1
    
    # Initialize variables
    left = 0
    min_len = float('inf')
    min_window = ""
    formed = 0
    
    # Create a dictionary to store the frequency of characters in the current window
    window_counts = defaultdict(int)
    
    # Move the right pointer to the right and add characters to the window
    for right in range(len(s)):
        character = s[right]
        window_counts[character] += 1
        
        # If the character is in t and its frequency in the window is equal to its frequency in t,
        # increment the formed variable
        if character in t_count and window_counts[character] == t_count[character]:
            formed += 1
        
        # While the window contains all characters of t and the left pointer is not at the start of the window,
        # try to minimize the window by moving the left pointer to the right
        while left <= right and formed == len(t_count):
            character = s[left]
            
            # If the current window is smaller than the minimum window found so far, update the minimum window
            if right - left + 1 < min_len:
                min_len = right - left + 1
                min_window = s[left:right + 1]
            
            # Move the left pointer to the right
            window_counts[character] -= 1
            if character in t_count and window_counts[character] < t_count[character]:
                formed -= 1
            left += 1
    
    return min_window
```

## Complexity
- Time: O(|s| + |t|) — The time complexity is O(|s| + |t|) because we are scanning the string `s` once and the string `t` once to create the frequency dictionary.
- Space: O(|s| + |t|) — The space complexity is O(|s| + |t|) because in the worst case, the size of the window can be equal to the size of the string `s`, and we are storing the frequency of characters in `t` in a dictionary.

## Key Insight
The core trick to solve this problem is to use the sliding window technique and maintain a dictionary to store the frequency of characters in the current window, allowing us to efficiently check if the window contains all characters of the target string.