## Problem
The Minimum Window Substring problem requires finding the smallest substring of a given string that contains all characters of another string at least once. This problem involves using a sliding window approach to efficiently scan the string and identify the minimum window that satisfies the condition.

## Examples
* Input: `s = "ADOBECODEBANC", t = "ABC"` Output: `"BANC"` (the smallest substring that contains all characters of `"ABC"`)
* Input: `s = "a", t = "a"` Output: `"a"` (the smallest substring that contains all characters of `"a"`)
* Input: `s = "ab", t = "b"` Output: `"b"` (the smallest substring that contains all characters of `"b"`)

## Approach
The approach to solve this problem involves using a sliding window technique to scan the string `s` and find the smallest substring that contains all characters of string `t`. The algorithm works as follows:
1. Create a dictionary to store the frequency of characters in string `t`.
2. Initialize two pointers, `left` and `right`, to represent the sliding window.
3. Expand the window to the right by moving the `right` pointer and update the frequency of characters in the window.
4. When the window contains all characters of `t`, try to minimize the window by moving the `left` pointer to the right.
5. Keep track of the minimum window size and the corresponding substring.

## Solution
```python
from collections import defaultdict

def minWindow(s: str, t: str) -> str:
    # Create a dictionary to store the frequency of characters in string t
    t_count = defaultdict(int)
    for char in t:
        t_count[char] += 1
    
    # Initialize variables to store the minimum window
    min_window = ""
    min_window_size = float('inf')
    
    # Initialize variables for the sliding window
    left = 0
    formed = 0
    
    # Create a dictionary to store the frequency of characters in the window
    window_counts = defaultdict(int)
    
    # Iterate over the string s
    for right in range(len(s)):
        # Add the character at the right pointer to the window
        character = s[right]
        window_counts[character] += 1
        
        # If the character is in t and its frequency in the window is equal to its frequency in t,
        # increment the formed variable
        if character in t_count and window_counts[character] == t_count[character]:
            formed += 1
        
        # While the window contains all characters of t and the left pointer is not at the beginning of the window
        while left <= right and formed == len(t_count):
            # Update the minimum window if the current window is smaller
            if right - left + 1 < min_window_size:
                min_window = s[left:right + 1]
                min_window_size = right - left + 1
            
            # Remove the character at the left pointer from the window
            character = s[left]
            window_counts[character] -= 1
            
            # If the character is in t and its frequency in the window is less than its frequency in t,
            # decrement the formed variable
            if character in t_count and window_counts[character] < t_count[character]:
                formed -= 1
            
            # Move the left pointer to the right
            left += 1
    
    return min_window
```

## Complexity
- Time: O(|s| + |t|) — The time complexity is linear because we iterate over both strings `s` and `t` once. The operations inside the loops (dictionary updates and comparisons) take constant time.
- Space: O(|s| + |t|) — The space complexity is linear because in the worst case, we might need to store all characters of both strings `s` and `t` in the dictionaries.

## Key Insight
The core trick to solve this problem is to use a sliding window approach with two pointers, `left` and `right`, to efficiently scan the string `s` and find the smallest substring that contains all characters of string `t`.