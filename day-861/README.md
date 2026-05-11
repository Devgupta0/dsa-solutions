## Problem
Given a string and an integer k, find the length of the longest substring with exactly k distinct characters. This problem involves finding the maximum length of a substring in a given string where the number of unique characters is equal to k.

## Examples
- Input: s = "eceba", k = 2
  Output: 3
  Explanation: The longest substring with exactly 2 distinct characters is "ece".
- Input: s = "abcba", k = 3
  Output: 3
  Explanation: The longest substring with exactly 3 distinct characters is "abc" or "bca".
- Input: s = "aaaa", k = 1
  Output: 4
  Explanation: The longest substring with exactly 1 distinct character is the entire string "aaaa".

## Approach
To solve this problem, we can use a sliding window approach in combination with a hash map to keep track of the frequency of characters within the current window. The algorithm works by expanding the window to the right and when the number of distinct characters exceeds k, it starts shrinking the window from the left until the number of distinct characters is exactly k.

Step by step:
1. Initialize a hash map to store the frequency of each character in the current window.
2. Initialize two pointers, left and right, to represent the start and end of the window.
3. Initialize a variable to store the maximum length of the substring with exactly k distinct characters.
4. Expand the window to the right by moving the right pointer and update the hash map with the new character.
5. If the number of distinct characters in the hash map exceeds k, start shrinking the window from the left by moving the left pointer and update the hash map by removing the character going out of the window.
6. Keep track of the maximum length of the substring with exactly k distinct characters.
7. Repeat steps 4-6 until the right pointer reaches the end of the string.

## Solution
```python
def longest_substring_with_k_distinct(s: str, k: int) -> int:
    # Initialize a hash map to store character frequencies
    char_freq = {}
    # Initialize variables to store the maximum length and the window boundaries
    max_length = 0
    left = 0
    
    # Iterate over the string with the right pointer
    for right in range(len(s)):
        # Add the new character to the hash map
        char_freq[s[right]] = char_freq.get(s[right], 0) + 1
        
        # Shrink the window if the number of distinct characters exceeds k
        while len(char_freq) > k:
            char_freq[s[left]] -= 1
            if char_freq[s[left]] == 0:
                del char_freq[s[left]]
            left += 1
        
        # Update the maximum length if the current window has exactly k distinct characters
        if len(char_freq) == k:
            max_length = max(max_length, right - left + 1)
    
    return max_length
```

## Complexity
- Time: O(n) — The algorithm iterates over the string once, where n is the length of the string. The operations within the loop (hash map updates and pointer movements) take constant time.
- Space: O(min(n, m)) — The space complexity is determined by the size of the hash map, which stores the frequency of characters. In the worst case, if all characters in the string are unique, the hash map will store n characters. However, since the hash map is limited to storing k distinct characters in this problem, the space complexity is O(min(n, m)), where m is the size of the character set.

## Key Insight
The core trick to solving this problem lies in utilizing a sliding window approach with a hash map to efficiently track the frequency of characters and adjust the window size based on the number of distinct characters.