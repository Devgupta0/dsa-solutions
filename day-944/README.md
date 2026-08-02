## Problem
Given a string and an integer k, find the length of the longest substring with exactly k distinct characters. This problem involves finding a substring within the given string that contains exactly k unique characters, and determining the maximum length of such a substring.

## Examples
* Input: s = "eceba", k = 2
  Output: 3
  Explanation: The longest substring with 2 distinct characters is "ece".
* Input: s = "aa", k = 1
  Output: 2
  Explanation: The longest substring with 1 distinct character is "aa".
* Input: s = "abc", k = 3
  Output: 3
  Explanation: The longest substring with 3 distinct characters is "abc".

## Approach
To solve this problem, we will use a sliding window approach combined with a hash map to keep track of the frequency of characters within the current window. In plain English, the algorithm works by maintaining a window of characters that moves through the string, expanding to the right when possible and contracting from the left when necessary, to always ensure that the window contains exactly k distinct characters. 
Here are the steps:
1. Initialize the left pointer of the window to 0, and an empty hash map to store character frequencies.
2. Expand the window to the right by moving the right pointer, updating the hash map with the frequency of each character.
3. When the number of distinct characters in the window exceeds k, contract the window from the left by moving the left pointer, updating the hash map accordingly.
4. Keep track of the maximum length of the window that contains exactly k distinct characters.

## Solution
```python
def longest_substring_with_k_distinct(s: str, k: int) -> int:
    """
    Find the length of the longest substring with exactly k distinct characters.

    Args:
    s (str): The input string.
    k (int): The number of distinct characters.

    Returns:
    int: The length of the longest substring with exactly k distinct characters.
    """
    if not s or k == 0:
        return 0
    
    char_freq = {}  # Hash map to store character frequencies
    left = 0  # Left pointer of the window
    max_length = 0  # Maximum length of the substring with k distinct characters
    
    for right in range(len(s)):  # Expand the window to the right
        char_freq[s[right]] = char_freq.get(s[right], 0) + 1
        
        # Contract the window from the left if the number of distinct characters exceeds k
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
- Time: O(n) — The algorithm iterates through the string once, where n is the length of the string. The operations within the loop (hash map updates and pointer movements) take constant time.
- Space: O(k) — The hash map stores the frequency of characters within the current window, which can contain at most k distinct characters.

## Key Insight
The core trick to solve this problem is to use a sliding window approach with a hash map to efficiently track the frequency of characters and maintain a window with exactly k distinct characters.