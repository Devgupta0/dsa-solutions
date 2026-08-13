## Problem
Given a string and an integer k, find the length of the longest substring that contains at most k distinct characters. This means we need to find a substring within the given string where the number of unique characters does not exceed k, and this substring should be as long as possible.

## Examples
* Input: `s = "eceba", k = 2`, Output: `3` (because "ece" is the longest substring with at most 2 distinct characters).
* Input: `s = "abcba", k = 3`, Output: `5` (because the entire string "abcba" has exactly 3 distinct characters).
* Input: `s = "aaabbbccc", k = 1`, Output: `3` (because the longest substrings with at most 1 distinct character are "aaa", "bbb", or "ccc", each of length 3).

## Approach
To solve this problem, we use a sliding window approach combined with a hash map to track the frequency of characters within the current window. The idea is to expand the window to the right as long as the number of distinct characters does not exceed k, and then start shrinking the window from the left when the number of distinct characters exceeds k, until we are back within the limit of k distinct characters.

Here's a step-by-step breakdown:
1. Initialize two pointers, `left` and `right`, to the start of the string, representing the sliding window.
2. Use a hash map to store the frequency of characters within the current window.
3. As we move the `right` pointer to the right, add the new character to the hash map and increment its count.
4. If the number of keys in the hash map exceeds k, move the `left` pointer to the right, removing characters from the hash map and decrementing their counts until the number of keys is less than or equal to k.
5. Keep track of the maximum length of the substring seen so far that has at most k distinct characters.

## Solution
```python
def longest_substring_with_k_distinct(s: str, k: int) -> int:
    # Initialize variables to keep track of the longest substring length and the current window boundaries
    max_length = 0
    left = 0
    char_freq = {}  # Hash map to store character frequencies within the current window
    
    # Iterate over the string with the right pointer of the sliding window
    for right in range(len(s)):
        # Add the character at the right pointer to the hash map
        char_freq[s[right]] = char_freq.get(s[right], 0) + 1
        
        # Shrink the window from the left if the number of distinct characters exceeds k
        while len(char_freq) > k:
            char_freq[s[left]] -= 1
            if char_freq[s[left]] == 0:
                del char_freq[s[left]]
            left += 1
        
        # Update the maximum length if the current window is longer
        max_length = max(max_length, right - left + 1)
    
    return max_length
```

## Complexity
- Time: O(n) — where n is the length of the string, because each character is visited at most twice (once by the `right` pointer and once by the `left` pointer).
- Space: O(min(n, k)) — because in the worst case, the hash map will store at most min(n, k) characters. However, in practice, it will typically store at most k characters since the number of distinct characters in the window is limited to k.

## Key Insight
The core trick to solving this problem efficiently is using a sliding window approach with a hash map to dynamically track and limit the number of distinct characters within the window.