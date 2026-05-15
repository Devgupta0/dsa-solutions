## Problem
The Minimum Window Substring problem requires finding the minimum length substring that contains all characters of a given string. This substring can have character repetition and non-contiguous characters, meaning the characters do not have to appear next to each other in the order they appear in the given string.

## Examples
* Input: `s = "ADOBECODEBANC", t = "ABC"` 
  Output: `"BANC"` (minimum length substring containing all characters of `"ABC"`)
* Input: `s = "a", t = "aa"` 
  Output: `""` (there is no substring containing all characters of `"aa"`)
* Input: `s = "aa", t = "aa"` 
  Output: `"aa"` (minimum length substring containing all characters of `"aa"`)

## Approach
To solve this problem, we will use the sliding window technique, a common approach for substring problems. The idea is to maintain a window (a substring of `s`) that contains all characters of `t`. We start by creating a dictionary to store the frequency of characters in `t`. Then, we expand the window to the right and shrink it from the left when all characters of `t` are found in the window. The step-by-step process involves:
1. Creating a dictionary to store the frequency of characters in `t`.
2. Initializing two pointers, `left` and `right`, to represent the sliding window.
3. Expanding the window to the right by moving the `right` pointer and updating the frequency of characters in the window.
4. When all characters of `t` are found in the window, updating the minimum length substring and shrinking the window from the left by moving the `left` pointer.
5. Repeating steps 3 and 4 until the `right` pointer reaches the end of `s`.

## Solution
```python
from collections import defaultdict

def min_window(s: str, t: str) -> str:
    # Create a dictionary to store the frequency of characters in t
    t_count = defaultdict(int)
    for char in t:
        t_count[char] += 1
    
    # Initialize the required character count
    required = len(t_count)
    
    # Initialize the minimum length substring
    min_len = float('inf')
    min_str = ""
    
    # Initialize the left and right pointers
    left = 0
    formed = 0
    
    # Create a dictionary to store the frequency of characters in the window
    window_counts = defaultdict(int)
    
    # Expand the window to the right
    for right in range(len(s)):
        character = s[right]
        window_counts[character] += 1
        
        # If the character is in t and its frequency in the window is equal to its frequency in t,
        # increment the formed count
        if character in t_count and window_counts[character] == t_count[character]:
            formed += 1
        
        # While the window contains all characters of t and the left pointer is not at the beginning of the window
        while left <= right and formed == required:
            character = s[left]
            
            # Update the minimum length substring
            if right - left + 1 < min_len:
                min_len = right - left + 1
                min_str = s[left:right + 1]
            
            # Shrink the window from the left
            window_counts[character] -= 1
            if character in t_count and window_counts[character] < t_count[character]:
                formed -= 1
            
            # Move the left pointer to the right
            left += 1
    
    # Return the minimum length substring
    return min_str

# Example usage:
print(min_window("ADOBECODEBANC", "ABC"))  # Output: "BANC"
```

## Complexity
- Time: O(|s| + |t|) — The time complexity is linear with respect to the lengths of `s` and `t` because we iterate over `s` and `t` once. The dictionary operations (insertion and lookup) take constant time.
- Space: O(|s| + |t|) — The space complexity is also linear with respect to the lengths of `s` and `t` because in the worst case, we might need to store all characters of `s` and `t` in the dictionaries.

## Key Insight
The core trick to solve this problem is to use the sliding window technique with two dictionaries to efficiently track the frequency of characters in the window and in the target string `t`.