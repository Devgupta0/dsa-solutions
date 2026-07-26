## Problem
Given a string, find the length of the longest substring where every character appears at least twice, with no more than K distinct characters. This problem requires us to efficiently scan the string and identify the longest substring that meets these conditions.

## Examples
* Input: `s = "abcabc", K = 2`, Output: `4` (The longest substring "abca" has 4 characters and every character appears at least twice, with only 2 distinct characters 'a' and 'b' or 'c' and 'b' or 'a' and 'c')
* Input: `s = "aabbbcc", K = 2`, Output: `6` (The longest substring "aabbbcc" has 6 characters and every character appears at least twice, with only 2 distinct characters 'a' and 'b')
* Input: `s = "abcdef", K = 2`, Output: `0` (No substring meets the condition of having every character appear at least twice with no more than 2 distinct characters)

## Approach
To solve this problem, we'll use the sliding window technique along with character counting. We start by initializing a window and slowly expand it to the right, keeping track of the frequency of each character in the current window. If the number of distinct characters exceeds K or if any character in the window appears less than twice, we start shrinking the window from the left until the condition is met again.

Here are the steps:
1. Initialize two pointers, `left` and `right`, to represent the sliding window.
2. Expand the window to the right by moving the `right` pointer and update the frequency of characters in the window.
3. Check if the number of distinct characters exceeds K or if any character appears less than twice. If the condition is not met, shrink the window by moving the `left` pointer to the right.
4. Keep track of the maximum length of the substring that meets the condition.

## Solution
```python
def longest_substring(s, K):
    """
    Find the length of the longest substring where every character appears at least twice, 
    with no more than K distinct characters.

    Args:
        s (str): The input string.
        K (int): The maximum number of distinct characters.

    Returns:
        int: The length of the longest substring that meets the condition.
    """
    if not s or K < 1:
        return 0

    max_length = 0
    char_freq = {}
    distinct_chars = 0
    left = 0

    for right in range(len(s)):
        # Add the character at the right pointer to the frequency dictionary
        char_freq[s[right]] = char_freq.get(s[right], 0) + 1

        # If the character appears for the first time, increment the distinct characters count
        if char_freq[s[right]] == 1:
            distinct_chars += 1

        # Shrink the window if the number of distinct characters exceeds K
        while distinct_chars > K:
            char_freq[s[left]] -= 1
            if char_freq[s[left]] == 0:
                distinct_chars -= 1
            left += 1

        # Check if every character in the window appears at least twice
        appears_twice = all(freq >= 2 for freq in char_freq.values())

        # Update the maximum length if the current window meets the condition
        if appears_twice and right - left + 1 > max_length:
            max_length = right - left + 1

    return max_length
```

## Complexity
- Time: O(n) — where n is the length of the string, because we are potentially scanning the string once with the `right` pointer and at most once with the `left` pointer.
- Space: O(min(n, m)) — where m is the size of the character set, because we are storing the frequency of each character in the `char_freq` dictionary, and in the worst case, every character in the string is unique.

## Key Insight
The core trick to solving this problem is to efficiently manage the sliding window and track the frequency of characters, allowing us to quickly identify the longest substring that meets the given conditions.