## Problem
Given a string and an integer k, find the length of the longest substring with exactly k distinct characters. This problem involves finding the maximum length of a substring that contains exactly k unique characters, which can be achieved by using a sliding window approach.

## Examples
* Input: `s = "eceba", k = 2` Output: `3` Explanation: The longest substring with 2 distinct characters is "ece" or "eba".
* Input: `s = "aa", k = 1` Output: `2` Explanation: The longest substring with 1 distinct character is "aa".

## Approach
To solve this problem, we will use a sliding window approach along with a hash map to keep track of the frequency of each character in the current window. The algorithm can be explained as follows: 
1. Initialize two pointers, `left` and `right`, to the start of the string and a hash map `char_freq` to store the frequency of each character in the current window.
2. Expand the window to the right by incrementing the `right` pointer and updating the `char_freq` hash map.
3. When the number of distinct characters in the window exceeds k, shrink the window from the left by incrementing the `left` pointer and updating the `char_freq` hash map.
4. Keep track of the maximum length of the substring with exactly k distinct characters.

## Solution
```python
def longest_substring_with_k_distinct(s: str, k: int) -> int:
    # Initialize variables
    left = 0
    max_length = 0
    char_freq = {}

    # Iterate over the string
    for right in range(len(s)):
        # Add the current character to the hash map
        char_freq[s[right]] = char_freq.get(s[right], 0) + 1

        # Shrink the window if the number of distinct characters exceeds k
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
- Time: O(n) — The time complexity is O(n) because we are scanning the string once, where n is the length of the string. The operations inside the loop (hash map updates and pointer increments) take constant time.
- Space: O(min(n, m)) — The space complexity is O(min(n, m)) because in the worst case, the size of the hash map can be equal to the number of unique characters in the string, which is min(n, m), where m is the size of the character set.

## Key Insight
The core trick to solve this problem is to use a sliding window approach along with a hash map to efficiently track the frequency of each character in the current window and to dynamically adjust the window size based on the number of distinct characters.