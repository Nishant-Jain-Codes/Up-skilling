# Valid Palindrome

- Link: https://neetcode.io/problems/is-palindrome
- Category: Two Pointers · [[DSA-Index]]
- Pattern: two pointers from both ends, skipping non-alphanumeric characters
- Language: Python
- Solved on: 2026-11-18 · minutes: 
- Needed the solution? 

## Accepted solution (mine)
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        si = 0
        ei = len(s) - 1

        while si < ei:
            while si < ei and not s[si].isalnum():
                si += 1

            while si < ei and not s[ei].isalnum():
                ei -= 1

            if s[si].lower() != s[ei].lower():
                return False

            si += 1
            ei -= 1

        return True
```

## Approach (own words)
Put one pointer at each end. Move each inward past any character that isn't a letter or digit. Compare the two characters ignoring case. If they differ, it's not a palindrome. If the pointers meet without a mismatch, it is.

## Complexity
- Time: **O(n)**. Each pointer moves inward at most n times in total, so every character is visited once.
- Space: **O(1)**. No cleaned copy of the string is built.

## Critique
- Correct, and this is the optimal approach (O(n) time, O(1) space).
- The `si < ei` guard inside both inner loops is the part people get wrong. Without it the pointers cross on input like `".,"` and you index out of range or compare the wrong characters. You have it.
- Edge cases work: an empty string, a string of only punctuation (pointers meet, returns `True`), and a single character.
- Names: `left` / `right` read better than `si` / `ei`.
- `isalnum()` accepts non-ASCII letters too. That doesn't matter here (the problem is ASCII), but it's worth knowing.

## Other approaches
| Approach | Time | Space |
|---|---|---|
| Clean the string, then `cleaned == cleaned[::-1]` | O(n) | O(n) |
| **Two pointers, skip in place (mine)** | **O(n)** | **O(1)** |

The clean-and-reverse version is shorter and fine as a first answer. Two pointers is what an interviewer wants when they ask for O(1) space:
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        cleaned = [c.lower() for c in s if c.isalnum()]
        return cleaned == cleaned[::-1]
```

## Mistake / what to remember
- "Same from both ends" means two pointers moving inward.
- When skipping characters inside a loop, keep the `left < right` check on every inner `while`.
- Revisit: write it from memory at the +14 day review and handle `".,"` first.

## Review
- [ ] +3 days (2026-11-21)
- [ ] +14 days (2026-12-02)
