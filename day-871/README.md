## Problem
The Minimum Window Substring problem is a classic string matching problem that involves finding the smallest substring of a given string that contains all unique characters of the string at least once. The goal is to identify the minimum-length substring that satisfies this condition, making it a challenging problem that requires an efficient algorithm to solve.

## Examples
* Input: `s = "ADOBECODEBANC", t = "ABC"` 
  Output: `"BANC"` 
* Input: `s = "a", t = "a"` 
  Output: `"a"` 
* Input: `s = "aa", t = "aa"` 
  Output: `"aa"`

## Approach
To solve this problem, we can use a sliding window approach, which is a common technique for string matching and substring search problems. The basic idea is to maintain a window of characters that we are currently considering, and then slide this window over the string to find the minimum substring that meets the condition. 

Here's a step-by-step breakdown of the algorithm:
1. Initialize two pointers, `left` and `right`, to represent the start and end of the sliding window.
2. Create a dictionary to store the frequency of each character in the string `t`.
3. Iterate over the string `s` using the `right` pointer, expanding the window to the right.
4. For each character in the window, update the frequency dictionary to reflect the current characters in the window.
5. When the window contains all characters of `t`, try to shrink the window by moving the `left` pointer to the right.
6. Keep track of the minimum-length substring that satisfies the condition.

## Solution
```python
from collections import defaultdict

def min_window(s, t):
    # Create a dictionary to store the frequency of each character in t
    t_count = defaultdict(int)
    for char in t:
        t_count[char] += 1
    
    # Initialize the required character count
    required = len(t_count)
    
    # Initialize the left and right pointers
    left = 0
    min_length = float('inf')
    min_window = ""
    
    # Initialize the formed character count
    formed = 0
    
    # Create a dictionary to store the frequency of each character in the window
    window_counts = defaultdict(int)
    
    # Iterate over the string s
    for right in range(len(s)):
        # Add the character at the right pointer to the window
        character = s[right]
        window_counts[character] += 1
        
        # If the character is in t and its frequency in the window is equal to its frequency in t,
        # increment the formed count
        if character in t_count and window_counts[character] == t_count[character]:
            formed += 1
        
        # While the window contains all characters of t and the left pointer is less than the right pointer,
        # try to shrink the window
        while left <= right and formed == required:
            # Update the minimum window if the current window is smaller
            if right - left + 1 < min_length:
                min_length = right - left + 1
                min_window = s[left:right + 1]
            
            # Remove the character at the left pointer from the window
            character = s[left]
            window_counts[character] -= 1
            
            # If the character is in t and its frequency in the window is less than its frequency in t,
            # decrement the formed count
            if character in t_count and window_counts[character] < t_count[character]:
                formed -= 1
            
            # Move the left pointer to the right
            left += 1
    
    # Return the minimum window
    return min_window
```

## Complexity
- Time: O(|s| + |t|) — The algorithm iterates over the string `s` once and the string `t` once to create the frequency dictionary. The while loop inside the for loop does not change the overall time complexity because each character in `s` is visited at most twice (once by the `right` pointer and once by the `left` pointer).
- Space: O(|s| + |t|) — The algorithm uses two dictionaries to store the frequency of characters in `t` and the window, which requires O(|t|) and O(|s|) space, respectively.

## Key Insight
The core trick to solving this problem is to use a sliding window approach with two pointers, `left` and `right`, to efficiently find the minimum-length substring that contains all unique characters of the given string at least once.