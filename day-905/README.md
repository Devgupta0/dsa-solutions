## Problem
Given a string and an integer k, find the length of the longest substring that contains exactly k distinct characters. This problem requires us to efficiently scan the string and keep track of the unique characters in each substring.

## Examples
- Input: `s = "eceba", k = 2`, Output: `3` (The longest substring with 2 distinct characters is "ece" or "eba")
- Input: `s = "aa", k = 1`, Output: `2` (The longest substring with 1 distinct character is "aa")

## Approach
To solve this problem, we will use the sliding window technique along with a hash map to keep track of the frequency of characters in the current substring. The algorithm can be explained as follows:
1. Initialize two pointers, `left` and `right`, to represent the sliding window.
2. Use a hash map to store the frequency of characters in the current window.
3. Expand the window to the right and update the hash map.
4. When the number of distinct characters exceeds `k`, move the `left` pointer to the right and update the hash map.
5. Keep track of the maximum length of the substring with exactly `k` distinct characters.

## Solution
```python
def longest_substring_with_k_distinct(s: str, k: int) -> int:
    # Initialize variables
    max_length = 0
    char_frequency = {}
    left = 0
    
    # Iterate over the string
    for right in range(len(s)):
        # Add the current character to the hash map
        char_frequency[s[right]] = char_frequency.get(s[right], 0) + 1
        
        # Shrink the window if the number of distinct characters exceeds k
        while len(char_frequency) > k:
            char_frequency[s[left]] -= 1
            if char_frequency[s[left]] == 0:
                del char_frequency[s[left]]
            left += 1
        
        # Update the maximum length if the number of distinct characters is k
        if len(char_frequency) == k:
            max_length = max(max_length, right - left + 1)
    
    return max_length
```

## Complexity
- Time: O(n) — because each character in the string is visited at most twice (once by the `right` pointer and once by the `left` pointer).
- Space: O(k) — because the hash map stores at most `k` distinct characters.

## Key Insight
The core trick to solve this problem is to use a sliding window approach with a hash map to efficiently track the frequency of characters and maintain a window with exactly k distinct characters.