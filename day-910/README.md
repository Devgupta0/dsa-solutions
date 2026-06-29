## Problem
Given a string and a set of repeating characters, find the longest substring that contains all the repeating characters at least the number of times they repeat. The problem requires finding the longest contiguous substring that satisfies the condition of having all repeating characters with their specified frequencies.

## Examples
- Input: `s = "abcabcbb", repeating_chars = {"a": 2, "b": 2}` 
  Output: `"abcabc"`
- Input: `s = "aaaaaa", repeating_chars = {"a": 6}` 
  Output: `"aaaaaa"`
- Input: `s = "abcdabcd", repeating_chars = {"a": 1, "b": 1, "c": 1, "d": 1}` 
  Output: `"abcdabcd"`

## Approach
The approach to solve this problem involves using a sliding window technique to find the longest substring that contains all repeating characters with their specified frequencies. In plain English, the algorithm works by maintaining a window of characters in the string and expanding or shrinking this window based on whether all repeating characters are present in the current window with their required frequencies. Here are the steps:
1. Initialize a dictionary to store the frequency of each character in the current window.
2. Initialize variables to store the maximum length of the substring and the substring itself.
3. Iterate over the string using a sliding window approach.
4. For each character in the window, update its frequency in the dictionary.
5. Check if all repeating characters are present in the window with their required frequencies.
6. If they are, update the maximum length and the substring if the current window is larger.
7. If not, shrink the window from the left until all repeating characters are present with their required frequencies.
8. Repeat steps 3-7 until the entire string has been processed.

## Solution
```python
def longest_substring(s, repeating_chars):
    # Initialize variables to store the maximum length and the substring
    max_length = 0
    max_substring = ""
    
    # Initialize a dictionary to store the frequency of each character in the current window
    char_freq = {}
    
    # Initialize variables for the sliding window
    left = 0
    
    # Iterate over the string
    for right in range(len(s)):
        # Update the frequency of the current character in the window
        char_freq[s[right]] = char_freq.get(s[right], 0) + 1
        
        # While the window is not valid (i.e., not all repeating characters are present with their required frequencies)
        while not is_valid_window(char_freq, repeating_chars):
            # Shrink the window from the left
            char_freq[s[left]] -= 1
            if char_freq[s[left]] == 0:
                del char_freq[s[left]]
            left += 1
        
        # If the current window is larger than the maximum substring found so far, update the maximum substring
        if right - left + 1 > max_length:
            max_length = right - left + 1
            max_substring = s[left:right + 1]
    
    return max_substring

def is_valid_window(char_freq, repeating_chars):
    # Check if all repeating characters are present in the window with their required frequencies
    for char, freq in repeating_chars.items():
        if char not in char_freq or char_freq[char] < freq:
            return False
    return True
```

## Complexity
- Time: O(n) — The algorithm iterates over the string once, where n is the length of the string. The while loop inside the for loop does not increase the overall time complexity because each character is visited at most twice (once by the for loop and once by the while loop).
- Space: O(n) — The space complexity is due to the dictionary used to store the frequency of each character in the current window. In the worst case, the dictionary can contain n characters.

## Key Insight
The core trick to solve this problem is to use a sliding window approach with a dictionary to efficiently track the frequency of each character in the current window, allowing for fast validation of whether the window contains all repeating characters with their required frequencies.