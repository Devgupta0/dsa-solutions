## Problem
The Minimum Window Substring problem requires finding the shortest substring of a given string that contains all characters of another string. The condition is that every character in the second string must be present in the window at least as many times as in the second string.

## Examples
* Input: `s = "ADOBECODEBANC", t = "ABC"` 
  Output: `"BANC"` 
* Input: `s = "a", t = "a"` 
  Output: `"a"`
* Input: `s = "aa", t = "aa"` 
  Output: `"aa"`

## Approach
To solve this problem, we will use the sliding window technique. The idea is to maintain a window of characters in the string `s` that contains all characters of the string `t`. We start with an empty window and expand it to the right, adding characters to the window until it contains all characters of `t`. Once we have a valid window, we try to shrink it from the left, removing characters from the window as long as it remains valid. We keep track of the minimum window found so far.

Here's a step-by-step breakdown:
1. Create a dictionary to store the frequency of characters in the string `t`.
2. Initialize two pointers, `left` and `right`, to the start of the string `s`.
3. Expand the window to the right by moving the `right` pointer, adding characters to the window and updating the frequency dictionary.
4. When the window contains all characters of `t`, try to shrink it from the left by moving the `left` pointer, removing characters from the window and updating the frequency dictionary.
5. Keep track of the minimum window found so far.

## Solution
```python
from collections import defaultdict

def min_window(s: str, t: str) -> str:
    # Create a dictionary to store the frequency of characters in t
    t_count = defaultdict(int)
    for char in t:
        t_count[char] += 1
    
    # Initialize variables
    required_chars = len(t_count)
    left = 0
    min_len = float('inf')
    min_window = ""
    
    # Initialize a dictionary to store the frequency of characters in the window
    window_counts = defaultdict(int)
    formed_chars = 0
    
    # Expand the window to the right
    for right in range(len(s)):
        # Add the character to the window
        character = s[right]
        window_counts[character] += 1
        
        # If the character is in t and its frequency in the window is equal to its frequency in t,
        # increment the formed_chars count
        if character in t_count and window_counts[character] == t_count[character]:
            formed_chars += 1
        
        # Try to shrink the window from the left
        while left <= right and formed_chars == required_chars:
            # Update the minimum window if the current window is smaller
            if right - left + 1 < min_len:
                min_len = right - left + 1
                min_window = s[left:right + 1]
            
            # Remove the character from the window
            character = s[left]
            window_counts[character] -= 1
            
            # If the character is in t and its frequency in the window is less than its frequency in t,
            # decrement the formed_chars count
            if character in t_count and window_counts[character] < t_count[character]:
                formed_chars -= 1
            
            # Move the left pointer to the right
            left += 1
    
    return min_window

# Example usage
s = "ADOBECODEBANC"
t = "ABC"
print(min_window(s, t))  # Output: "BANC"
```

## Complexity
- Time: O(|s| + |t|) — We iterate over the string `s` once and the string `t` once to create the frequency dictionary. The while loop inside the for loop does not change the overall time complexity because each character in `s` is visited at most twice (once by the right pointer and once by the left pointer).
- Space: O(|s| + |t|) — We use two dictionaries to store the frequency of characters in `t` and in the window. The maximum size of these dictionaries is equal to the number of unique characters in `s` and `t`.

## Key Insight
The core trick to solve this problem is to use the sliding window technique with two pointers and a frequency dictionary to efficiently find the minimum window that contains all characters of the given string.