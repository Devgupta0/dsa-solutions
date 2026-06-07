## Problem
Given a string and an integer k, find the length of the longest substring that contains exactly k distinct characters. This means we need to identify the longest contiguous segment of the string where the number of unique characters is equal to k.

## Examples
* Input: `s = "eceba", k = 2`, Output: `3` (The longest substring with 2 distinct characters is "ece" or "eba").
* Input: `s = "aa", k = 1`, Output: `2` (The longest substring with 1 distinct character is "aa").

## Approach
To solve this problem, we'll use the sliding window technique in combination with a hash map to track the frequency of characters within the current window. The algorithm works by expanding the window to the right when the number of distinct characters is less than or equal to k and contracting the window from the left when the number of distinct characters exceeds k. 
1. Initialize two pointers, `left` and `right`, to the start of the string, representing the sliding window.
2. Use a hash map to store the frequency of characters within the current window.
3. Expand the window to the right by moving the `right` pointer and update the hash map accordingly.
4. When the number of distinct characters exceeds k, contract the window from the left by moving the `left` pointer and update the hash map.
5. Keep track of the maximum length of the substring that contains exactly k distinct characters.

## Solution
```python
def length_of_longest_substring_k_distinct(s: str, k: int) -> int:
    """
    Find the length of the longest substring that contains exactly k distinct characters.

    Args:
    s (str): The input string.
    k (int): The number of distinct characters.

    Returns:
    int: The length of the longest substring with k distinct characters.
    """
    if not s or k == 0:
        return 0
    
    # Initialize the hash map and variables
    char_freq = {}
    left = 0  # Left pointer of the sliding window
    max_length = 0  # Maximum length of the substring with k distinct characters
    
    # Expand the window to the right
    for right in range(len(s)):
        # Add the character at the right pointer to the hash map
        char_freq[s[right]] = char_freq.get(s[right], 0) + 1
        
        # Contract the window from the left if the number of distinct characters exceeds k
        while len(char_freq) > k:
            char_freq[s[left]] -= 1
            if char_freq[s[left]] == 0:
                del char_freq[s[left]]
            left += 1
        
        # Update the maximum length if the number of distinct characters is equal to k
        if len(char_freq) == k:
            max_length = max(max_length, right - left + 1)
    
    return max_length
```

## Complexity
- Time: O(n) — where n is the length of the string, because each character is visited at most twice (once by the right pointer and once by the left pointer).
- Space: O(min(n, m)) — where m is the size of the character set, because in the worst case, the hash map will store all unique characters in the string, which is bounded by the minimum of the string length and the character set size.

## Key Insight
The core trick to solving this problem lies in utilizing the sliding window technique to efficiently track the number of distinct characters within the current window, allowing for a linear time complexity solution.