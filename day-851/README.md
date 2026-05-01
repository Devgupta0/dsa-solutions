## Problem
Given two strings `s` and `t`, find the minimum window in `s` that contains all characters of `t`. The minimum window is the smallest substring of `s` that contains all characters of `t`. If there is no such window, return an empty string.

## Examples
* Input: `s = "ADOBECODEBANC", t = "ABC"` 
  Output: `"BANC"`
* Input: `s = "a", t = "a"` 
  Output: `"a"`
* Input: `s = "bba", t = "ab"` 
  Output: `"ba"`

## Approach
To solve this problem, we use a sliding window approach combined with a hashmap to keep track of the characters in the window. We start by creating a hashmap of the characters in `t` and their frequencies. Then, we initialize two pointers, `left` and `right`, to the start of `s`. We expand the window to the right by moving `right` and update the hashmap with the characters we encounter. When the window contains all characters of `t`, we try to minimize the window by moving `left` to the right. We keep track of the minimum window seen so far and return it at the end.

Here are the steps:
1. Create a hashmap `t_count` of the characters in `t` and their frequencies.
2. Initialize two pointers, `left` and `right`, to the start of `s`.
3. Initialize a hashmap `window_count` to store the characters in the current window and their frequencies.
4. Initialize a variable `required` to the number of unique characters in `t`.
5. Initialize a variable `formed` to 0, which stores the number of characters in the window that are also in `t`.
6. Move `right` to the right and update `window_count` and `formed` accordingly.
7. When `formed` equals `required`, try to minimize the window by moving `left` to the right.
8. Update the minimum window if the current window is smaller.

## Solution
```python
from collections import defaultdict

def min_window(s, t):
    """
    Find the minimum window in `s` that contains all characters of `t`.
    
    Args:
        s (str): The string to search for the minimum window.
        t (str): The string containing the characters to search for.
    
    Returns:
        str: The minimum window in `s` that contains all characters of `t`.
    """
    # Create a hashmap of the characters in `t` and their frequencies
    t_count = defaultdict(int)
    for char in t:
        t_count[char] += 1
    
    # Initialize variables to keep track of the minimum window
    required = len(t_count)
    formed = 0
    
    # Initialize variables to keep track of the window
    window_counts = defaultdict(int)
    left = 0
    
    # Initialize variables to keep track of the minimum window
    min_len = float("inf")
    min_window = ""
    
    # Move the right pointer to the right
    for right in range(len(s)):
        # Add the character at the right pointer to the window
        character = s[right]
        window_counts[character] += 1
        
        # If the character is in `t` and its frequency in the window is equal to its frequency in `t`,
        # increment the `formed` counter
        if character in t_count and window_counts[character] == t_count[character]:
            formed += 1
        
        # While the window contains all characters of `t` and the left pointer is not at the start of the window,
        # try to minimize the window by moving the left pointer to the right
        while left <= right and formed == required:
            # Update the minimum window if the current window is smaller
            if right - left + 1 < min_len:
                min_len = right - left + 1
                min_window = s[left:right + 1]
            
            # Remove the character at the left pointer from the window
            character = s[left]
            window_counts[character] -= 1
            
            # If the character is in `t` and its frequency in the window is less than its frequency in `t`,
            # decrement the `formed` counter
            if character in t_count and window_counts[character] < t_count[character]:
                formed -= 1
            
            # Move the left pointer to the right
            left += 1
    
    # Return the minimum window
    return min_window

# Example usage:
print(min_window("ADOBECODEBANC", "ABC"))  # Output: "BANC"
print(min_window("a", "a"))  # Output: "a"
print(min_window("bba", "ab"))  # Output: "ba"
```

## Complexity
- Time: O(|s| + |t|) — The time complexity is linear because we make a single pass over `s` and `t`.
- Space: O(|s| + |t|) — The space complexity is also linear because in the worst case, we might need to store all characters of `s` and `t` in the hashmaps.

## Key Insight
The core trick to solve this problem is to use a sliding window approach combined with hashmaps to efficiently keep track of the characters in the window and their frequencies.