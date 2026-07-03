## Problem
Given a string and an integer k, find the length of the longest substring that contains exactly k distinct characters. This problem requires implementing a solution that efficiently scans the string to identify the longest substring with the specified number of distinct characters.

## Examples
* Input: `s = "eceba", k = 2`, Output: `3` (The longest substring with 2 distinct characters is "ece")
* Input: `s = "aa", k = 1`, Output: `2` (The longest substring with 1 distinct character is "aa")
* Input: `s = "abcba", k = 3`, Output: `3` (The longest substring with 3 distinct characters is "abc" or "bca")

## Approach
To solve this problem, we use the sliding window technique in combination with a hash map to track the frequency of characters within the current window. The algorithm works by maintaining a window of characters and expanding it to the right while keeping track of the number of distinct characters. When the number of distinct characters exceeds k, we start moving the left boundary of the window to the right until the number of distinct characters is exactly k again.

Here are the steps:
1. Initialize two pointers, `left` and `right`, to the start of the string.
2. Create a hash map to store the frequency of characters within the current window.
3. Expand the window to the right by moving the `right` pointer and update the hash map.
4. If the number of distinct characters in the hash map exceeds k, move the `left` pointer to the right and update the hash map accordingly.
5. Keep track of the maximum length of the substring with exactly k distinct characters.

## Solution
```python
def longest_substring_with_k_distinct(s: str, k: int) -> int:
    """
    Given a string and an integer k, find the length of the longest substring 
    that contains exactly k distinct characters.

    Args:
        s (str): The input string.
        k (int): The number of distinct characters.

    Returns:
        int: The length of the longest substring with k distinct characters.
    """
    # Initialize variables to store the maximum length and the frequency of characters
    max_length = 0
    char_freq = {}
    
    # Initialize the window boundaries
    left = 0
    
    # Iterate over the string
    for right in range(len(s)):
        # Add the current character to the frequency map
        char_freq[s[right]] = char_freq.get(s[right], 0) + 1
        
        # While there are more than k distinct characters, shrink the window
        while len(char_freq) > k:
            # Remove the leftmost character from the frequency map
            char_freq[s[left]] -= 1
            if char_freq[s[left]] == 0:
                del char_freq[s[left]]
            # Move the left boundary to the right
            left += 1
        
        # If the number of distinct characters is exactly k, update the max length
        if len(char_freq) == k:
            max_length = max(max_length, right - left + 1)
    
    return max_length
```

## Complexity
- Time: O(n) — where n is the length of the string, because each character is visited at most twice (once by the right pointer and once by the left pointer).
- Space: O(min(n, m)) — where m is the size of the character set, because in the worst case, the hash map will store the frequency of each unique character in the string, and the maximum number of unique characters is the minimum of the string length and the character set size.

## Key Insight
The core trick to solve this problem is using a sliding window in combination with a hash map to efficiently track the frequency of characters and maintain the constraint of having exactly k distinct characters in the current substring.