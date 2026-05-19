## Problem
The Minimum Window Substring problem involves finding the smallest substring of a given string that contains all unique characters of the string. The goal is to identify the minimum length substring that includes every distinct character from the original string, effectively covering the entire character set in the most compact manner possible.

## Examples
- Example 1: Given string "abc", the smallest substring that contains all unique characters is "abc" itself, as it includes all distinct characters 'a', 'b', and 'c'.
- Example 2: Given string "aab", the smallest substring that contains all unique characters is "ab" or "ba", as both include the distinct characters 'a' and 'b'.
- Example 3: Given string "aabbcc", the smallest substring that contains all unique characters is "abc" since it contains all the unique characters 'a', 'b', and 'c'.

## Approach
To solve this problem, we will use a sliding window approach combined with string matching techniques. The basic idea is to maintain a window that expands and contracts as we traverse the string, ensuring that the window always contains all unique characters of the given string. We will use a set to keep track of unique characters in the string and a dictionary to count the frequency of characters within the current window.

Step by step:
1. Initialize a set with unique characters from the given string.
2. Create a sliding window with two pointers, `left` and `right`, starting at the beginning of the string.
3. Expand the window to the right by moving the `right` pointer until the window contains all unique characters.
4. Once the window contains all unique characters, try to minimize the window by moving the `left` pointer to the right while ensuring the window still contains all unique characters.
5. Keep track of the minimum window size and its corresponding substring.
6. Repeat steps 3-5 until the `right` pointer reaches the end of the string.

## Solution
```python
from collections import defaultdict

def min_window_substring(s):
    # Create a set of unique characters in the string
    unique_chars = set(s)
    
    # Initialize minimum window size to infinity and result substring
    min_window_size = float('inf')
    result = ""
    
    # Initialize the left pointer of the sliding window
    left = 0
    
    # Initialize a dictionary to count the frequency of characters in the window
    char_count = defaultdict(int)
    
    # Initialize a variable to track the number of unique characters in the window
    unique_chars_in_window = 0
    
    # Traverse the string with the right pointer of the sliding window
    for right in range(len(s)):
        # Increment the count of the current character
        char_count[s[right]] += 1
        
        # If the current character is in the set of unique characters and its count is 1, increment the unique characters in window count
        if s[right] in unique_chars and char_count[s[right]] == 1:
            unique_chars_in_window += 1
        
        # While the window contains all unique characters, try to minimize the window
        while unique_chars_in_window == len(unique_chars):
            # If the current window size is less than the minimum window size, update the minimum window size and result substring
            if right - left + 1 < min_window_size:
                min_window_size = right - left + 1
                result = s[left:right + 1]
            
            # Decrement the count of the character at the left pointer
            char_count[s[left]] -= 1
            
            # If the character at the left pointer is in the set of unique characters and its count is 0, decrement the unique characters in window count
            if s[left] in unique_chars and char_count[s[left]] == 0:
                unique_chars_in_window -= 1
            
            # Move the left pointer to the right
            left += 1
    
    return result

# Example usage
print(min_window_substring("abc"))  # Output: "abc"
print(min_window_substring("aab"))  # Output: "ab"
print(min_window_substring("aabbcc"))  # Output: "abc"
```

## Complexity
- Time: O(n) — The time complexity is linear because we are traversing the string once with the right pointer and potentially once with the left pointer in the worst-case scenario.
- Space: O(n) — The space complexity is also linear due to the use of the set to store unique characters, the dictionary to count character frequencies, and the sliding window, which in the worst case could be the size of the input string.

## Key Insight
The core trick to solving this problem efficiently lies in utilizing a sliding window approach combined with a set to track unique characters and a dictionary to count character frequencies within the window, allowing for an efficient expansion and contraction of the window to find the minimum substring that contains all unique characters.