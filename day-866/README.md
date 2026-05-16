## Problem
Given a string and an integer k, find the length of the longest substring with exactly k distinct characters. This problem involves finding a substring within the given string that contains exactly k unique characters and has the maximum length among all such substrings.

## Examples
* Input: s = "eceba", k = 2, Output: 3 (Longest substring is "ece" with 2 distinct characters)
* Input: s = "abc", k = 1, Output: 1 (Longest substring is "a" with 1 distinct character)
* Input: s = "aaaa", k = 1, Output: 4 (Longest substring is "aaaa" with 1 distinct character)

## Approach
The approach to solve this problem involves using a sliding window technique along with hashing. The idea is to maintain a window of characters and expand or shrink it based on the number of distinct characters within the window. We start by initializing two pointers, one at the beginning and one at the end of the window. We then expand the window to the right by moving the right pointer and keep track of the distinct characters within the window using a hash map. If the number of distinct characters exceeds k, we shrink the window from the left by moving the left pointer until the number of distinct characters is k. We keep track of the maximum length of the substring with k distinct characters during this process.

Here are the steps:
1. Initialize two pointers, left and right, to the beginning of the string.
2. Create a hash map to store the frequency of characters within the window.
3. Expand the window to the right by moving the right pointer and update the hash map.
4. If the number of distinct characters exceeds k, shrink the window from the left by moving the left pointer and update the hash map.
5. Keep track of the maximum length of the substring with k distinct characters.
6. Repeat steps 3-5 until the right pointer reaches the end of the string.

## Solution
```python
def longest_substring_with_k_distinct(s: str, k: int) -> int:
    """
    Given a string and an integer k, find the length of the longest substring with exactly k distinct characters.

    Args:
    s (str): The input string.
    k (int): The number of distinct characters.

    Returns:
    int: The length of the longest substring with exactly k distinct characters.
    """
    if not s or k == 0:  # edge case: empty string or k is 0
        return 0

    left = 0  # initialize left pointer
    max_length = 0  # initialize max length
    char_frequency = {}  # hash map to store character frequency

    for right in range(len(s)):  # expand window to the right
        char_frequency[s[right]] = char_frequency.get(s[right], 0) + 1  # update hash map

        # shrink window from the left if number of distinct characters exceeds k
        while len(char_frequency) > k:
            char_frequency[s[left]] -= 1  # update hash map
            if char_frequency[s[left]] == 0:
                del char_frequency[s[left]]  # remove character from hash map
            left += 1  # move left pointer

        # update max length if current window has k distinct characters
        if len(char_frequency) == k:
            max_length = max(max_length, right - left + 1)

    return max_length
```

## Complexity
- Time: O(n) — The time complexity is O(n) because each character in the string is visited at most twice, once by the right pointer and once by the left pointer.
- Space: O(min(n, m)) — The space complexity is O(min(n, m)) where n is the length of the string and m is the size of the character set. This is because in the worst case, the hash map will store all unique characters in the string.

## Key Insight
The core trick to solve this problem is to use a sliding window approach with a hash map to efficiently track the number of distinct characters within the window and adjust the window size accordingly.