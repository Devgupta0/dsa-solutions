## Problem
The problem requires finding the length of the longest substring with exactly k distinct characters in a given string. This means we need to identify the longest contiguous sequence of characters that contains exactly k unique characters.

## Examples
- Input: `s = "abcba", k = 2`, Output: `2` (Longest substring with 2 distinct characters is "ab" or "ba")
- Input: `s = "eceba", k = 3`, Output: `4` (Longest substring with 3 distinct characters is "eceb")
- Input: `s = "aaaa", k = 1`, Output: `4` (Longest substring with 1 distinct character is the entire string "aaaa")

## Approach
To solve this problem, we can use a sliding window approach combined with a hash map to keep track of the distinct characters in the current window. The algorithm works by expanding the window to the right and adding characters to the hash map until we have more than k distinct characters. Then, we start shrinking the window from the left, removing characters from the hash map, until we have exactly k distinct characters again. We keep track of the maximum length of the window that has exactly k distinct characters.

Here are the steps:
1. Initialize a hash map to store the frequency of each character in the current window.
2. Initialize two pointers, `left` and `right`, to represent the boundaries of the sliding window.
3. Initialize a variable, `max_length`, to store the maximum length of the substring with exactly k distinct characters.
4. Move the `right` pointer to the right, adding characters to the hash map and updating the frequency of each character.
5. If the number of distinct characters in the hash map is greater than k, move the `left` pointer to the right, removing characters from the hash map and updating the frequency of each character.
6. If the number of distinct characters in the hash map is equal to k, update `max_length` with the current length of the window.
7. Repeat steps 4-6 until the `right` pointer reaches the end of the string.

## Solution
```python
def longest_substring_with_k_distinct(s: str, k: int) -> int:
    """
    Returns the length of the longest substring with exactly k distinct characters.

    :param s: The input string.
    :param k: The number of distinct characters.
    :return: The length of the longest substring with k distinct characters.
    """
    # Initialize a hash map to store the frequency of each character
    char_frequency = {}
    
    # Initialize variables to store the maximum length and the window boundaries
    max_length = 0
    left = 0
    
    # Iterate over the string with the right pointer
    for right in range(len(s)):
        # Add the current character to the hash map
        char_frequency[s[right]] = char_frequency.get(s[right], 0) + 1
        
        # If the number of distinct characters is greater than k, shrink the window
        while len(char_frequency) > k:
            # Remove the character at the left pointer from the hash map
            char_frequency[s[left]] -= 1
            # If the frequency of the character is 0, remove it from the hash map
            if char_frequency[s[left]] == 0:
                del char_frequency[s[left]]
            # Move the left pointer to the right
            left += 1
        
        # If the number of distinct characters is equal to k, update the max length
        if len(char_frequency) == k:
            max_length = max(max_length, right - left + 1)
    
    return max_length
```

## Complexity
- Time: O(n) — The time complexity is linear because we are iterating over the string once with the right pointer, and the operations inside the loop (adding and removing characters from the hash map) take constant time.
- Space: O(min(n, m)) — The space complexity is linear because in the worst case, we might need to store all characters in the hash map. However, the number of characters is limited by the size of the character set (m), so the space complexity is O(min(n, m)), where n is the length of the string and m is the size of the character set.

## Key Insight
The core trick to solving this problem is using a sliding window approach with a hash map to efficiently keep track of the distinct characters in the current window.