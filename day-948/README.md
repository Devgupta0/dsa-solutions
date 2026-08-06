## Problem
Given a string and an integer K, find the length of the longest substring that contains no more than K distinct characters. This problem requires an efficient algorithm to scan the string and identify the longest substring that meets the given condition.

## Examples
* Input: `s = "abcba", K = 2`, Output: `2` (Longest substring is "ab" or "bc" or "cb")
* Input: `s = "aaaaaa", K = 1`, Output: `6` (Longest substring is the entire string "aaaaaa")
* Input: `s = "abcdef", K = 3`, Output: `3` (Longest substring is "abc" or "bcd" or "cde" or "def")

## Approach
To solve this problem, we can use a sliding window approach combined with a hash map to keep track of the characters in the current substring. The algorithm works by expanding the window to the right and when the number of distinct characters exceeds K, we shrink the window from the left. 
Here are the steps:
1. Initialize two pointers, `left` and `right`, to the start of the string.
2. Create a hash map to store the frequency of each character in the current substring.
3. Move the `right` pointer to the right, expanding the window, and update the hash map with the new character.
4. If the number of distinct characters in the hash map exceeds K, move the `left` pointer to the right, shrinking the window, and update the hash map by removing the character that is no longer in the window.
5. Keep track of the maximum length of the substring that meets the condition.

## Solution
```python
def longest_substring_with_k_distinct(s: str, k: int) -> int:
    # Initialize the hash map and the maximum length
    char_freq = {}
    max_length = 0
    
    # Initialize the window boundaries
    left = 0
    
    # Iterate over the string
    for right in range(len(s)):
        # Add the new character to the hash map
        char_freq[s[right]] = char_freq.get(s[right], 0) + 1
        
        # Shrink the window if the number of distinct characters exceeds K
        while len(char_freq) > k:
            char_freq[s[left]] -= 1
            if char_freq[s[left]] == 0:
                del char_freq[s[left]]
            left += 1
        
        # Update the maximum length
        max_length = max(max_length, right - left + 1)
    
    return max_length
```

## Complexity
- Time: O(n) — where n is the length of the string, because we are scanning the string once and performing constant time operations for each character.
- Space: O(min(n, k)) — because in the worst case, we might store all characters in the hash map, but the number of distinct characters is limited by K, so the space complexity is at most K.

## Key Insight
The core trick to solve this problem is to use a sliding window approach with a hash map to efficiently track the number of distinct characters in the current substring and adjust the window boundaries accordingly.