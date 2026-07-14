## Problem
The problem requires finding the length of the longest substring with exactly k distinct characters in a given string. This involves scanning the string and keeping track of the unique characters within a certain window, ensuring that the window always contains exactly k distinct characters.

## Examples
- Input: s = "eceba", k = 2, Output: 3 (Longest substring "ece" or "eba" has 2 distinct characters)
- Input: s = "abcba", k = 2, Output: 3 (Longest substring "abc" or "bca" or "cba" has 2 distinct characters)
- Input: s = "aaaa", k = 1, Output: 4 (Longest substring "aaaa" has 1 distinct character)

## Approach
To solve this problem, we will use the sliding window technique along with a hash map to keep track of the frequency of characters within the window. The algorithm works by expanding the window to the right and adding characters to the hash map. When the number of distinct characters in the window exceeds k, we start shrinking the window from the left and remove characters from the hash map until the number of distinct characters is exactly k. The maximum length of the window during this process will give us the length of the longest substring with exactly k distinct characters.

Here's the step-by-step approach:
1. Initialize two pointers, left and right, to the start of the string, representing the window.
2. Initialize a hash map to store the frequency of characters within the window.
3. Expand the window to the right by moving the right pointer and adding the characters to the hash map.
4. If the number of distinct characters in the window exceeds k, start shrinking the window from the left by moving the left pointer and removing characters from the hash map.
5. Keep track of the maximum length of the window when the number of distinct characters is exactly k.

## Solution
```python
def longest_substring_with_k_distinct(s: str, k: int) -> int:
    # Initialize the maximum length and the hash map
    max_length = 0
    char_freq = {}

    # Initialize the window pointers
    left = 0

    # Expand the window to the right
    for right in range(len(s)):
        # Add the character to the hash map
        char_freq[s[right]] = char_freq.get(s[right], 0) + 1

        # Shrink the window from the left if the number of distinct characters exceeds k
        while len(char_freq) > k:
            char_freq[s[left]] -= 1
            if char_freq[s[left]] == 0:
                del char_freq[s[left]]
            left += 1

        # Update the maximum length if the number of distinct characters is exactly k
        if len(char_freq) == k:
            max_length = max(max_length, right - left + 1)

    return max_length
```

## Complexity
- Time: O(n) — The time complexity is linear because we are scanning the string once and performing constant time operations (hash map lookups and updates) within the loop.
- Space: O(min(n, m)) — The space complexity is linear with respect to the size of the string (n) or the size of the character set (m), as in the worst case, we might need to store all unique characters in the hash map.

## Key Insight
The core trick to solving this problem is to use a sliding window approach with a hash map to efficiently track the frequency of characters and maintain exactly k distinct characters within the window.