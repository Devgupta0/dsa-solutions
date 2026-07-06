## Problem
The Minimum Window Substring problem is a classic problem in computer science where you're given two strings, `s` and `t`. The goal is to find the minimum length substring of `s` that contains all characters of `t`, with the condition that every character in `t` must appear at least once in the substring. This problem requires an efficient algorithm to scan the string `s` and find the smallest substring that satisfies the condition.

## Examples
* Input: `s = "ADOBECODEBANC", t = "ABC"` 
  Output: `"BANC"` 
* Input: `s = "a", t = "a"` 
  Output: `"a"` 
* Input: `s = "aa", t = "aa"` 
  Output: `"aa"`

## Approach
To solve this problem, we use the sliding window technique, which is a common approach for string matching and substring search problems. The idea is to maintain a window of characters in `s` that could potentially contain all characters of `t`. We start with an empty window and expand it to the right by adding characters from `s`. As we add characters, we keep track of the frequency of each character in `t` that appears in the window. When the window contains all characters of `t`, we try to shrink the window from the left by removing characters, as long as the window still contains all characters of `t`. The smallest window that contains all characters of `t` is the minimum length substring we're looking for.

Here's a step-by-step breakdown:
1. Create a dictionary to store the frequency of each character in `t`.
2. Initialize two pointers, `left` and `right`, to represent the sliding window.
3. Initialize a variable to store the minimum length substring.
4. Expand the window to the right by moving the `right` pointer and updating the frequency dictionary.
5. When the window contains all characters of `t`, try to shrink the window from the left by moving the `left` pointer.
6. Update the minimum length substring if a smaller window is found.

## Solution
```python
from collections import defaultdict

def min_window(s: str, t: str) -> str:
    # Create a dictionary to store the frequency of each character in t
    t_freq = defaultdict(int)
    for char in t:
        t_freq[char] += 1
    
    # Initialize variables to store the minimum length substring
    min_len = float('inf')
    min_str = ""
    
    # Initialize the left and right pointers
    left = 0
    formed = 0
    
    # Create a dictionary to store the frequency of each character in the window
    window_freq = defaultdict(int)
    
    # Expand the window to the right
    for right in range(len(s)):
        char = s[right]
        window_freq[char] += 1
        
        # If the character is in t and its frequency in the window is equal to its frequency in t,
        # increment the formed variable
        if char in t_freq and window_freq[char] == t_freq[char]:
            formed += 1
        
        # While the window contains all characters of t, try to shrink the window from the left
        while formed == len(t_freq):
            # Update the minimum length substring if a smaller window is found
            if right - left + 1 < min_len:
                min_len = right - left + 1
                min_str = s[left:right + 1]
            
            # Remove the character at the left pointer from the window
            char = s[left]
            window_freq[char] -= 1
            
            # If the character is in t and its frequency in the window is less than its frequency in t,
            # decrement the formed variable
            if char in t_freq and window_freq[char] < t_freq[char]:
                formed -= 1
            
            # Move the left pointer to the right
            left += 1
    
    return min_str

# Test the function
print(min_window("ADOBECODEBANC", "ABC"))  # Output: "BANC"
print(min_window("a", "a"))  # Output: "a"
print(min_window("aa", "aa"))  # Output: "aa"
```

## Complexity
- Time: O(|s| + |t|) — The time complexity is linear with respect to the lengths of the strings `s` and `t`, because we make a single pass through `s` and a single pass through `t` to create the frequency dictionary.
- Space: O(|s| + |t|) — The space complexity is also linear with respect to the lengths of the strings `s` and `t`, because in the worst case, we might need to store all characters of `s` and `t` in the frequency dictionaries.

## Key Insight
The core trick to solving this problem is to use the sliding window technique to efficiently scan the string `s` and find the smallest substring that contains all characters of `t`, by maintaining a window of characters and tracking the frequency of each character in `t` that appears in the window.