# Valid Anagram

- Link: https://neetcode.io/problems/is-anagram
- Category: Arrays & Hashing · [[DSA-Index]]
- Pattern: frequency count (hash map / counter)
- Language: Python
- Solved on: 2026-11-16 · minutes: 
- Needed the solution? 

## Accepted solution (mine)
```python
class Solution:
    # time and space: O(N)
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        hash = {}
        for x in s:
            hash[x] = hash.get(x, 0) + 1
        for x in t:
            if x not in hash:
                return False
            if hash[x] <= 0:
                return False
            hash[x] -= 1
        return True
```

## Approach (own words)
Two strings are anagrams if they have the same length and the same count of every character. Count the characters of `s`. Then walk `t` and use up one count per character. If a character is missing or its count is already 0, return `False`. Equal lengths guarantee that nothing is left over.

## Complexity
- Time: O(n)
- Space: O(k), where k is the number of distinct characters, so at most O(n). If the input is only lowercase English letters (check the constraints), k ≤ 26, so it's **O(1)**.

## Critique
- Correct, and the early length check is good.
- `hash` shadows the built-in again (same as in [[Contains Duplicate]]). Name it `count`.
- The two `if`s can be one: `if count.get(x, 0) == 0: return False`.
- Space is better described as O(k) than O(n).

Tidier version:
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        count: dict[str, int] = {}
        for c in s:
            count[c] = count.get(c, 0) + 1
        for c in t:
            if count.get(c, 0) == 0:
                return False
            count[c] -= 1
        return True
```
One-liners: `Counter(s) == Counter(t)` or `sorted(s) == sorted(t)`.

## Other approaches
| Approach | Time | Space |
|---|---|---|
| Hash map count (mine) | O(n) | O(k) |
| Array of 26 counts (lowercase only) | O(n) | O(1) |
| Sort both, compare | O(n log n) | see note |

- **Sort + compare:** my idea of "sort, then two pointers" works, but you only need one index, because after sorting you compare `s[i]` with `t[i]`. Space is O(1) only if you can sort in place (C++ `sort` on a string). In Python, strings can't be sorted in place, and `sorted()` makes a list, so it's **O(n)** space.
- **Follow-up (Unicode):** the hash map still works for any characters. A fixed 26-slot array does not.

## Mistake / what to remember
- Repeated habit: naming a variable `hash`. Use `count` / `seen`.
- Check the constraints before calling a hash map O(n) space. A fixed alphabet makes it O(1).
- "Same multiset of items" means a frequency count.

## Review
- [ ] +3 days (2026-11-19)
- [ ] +14 days (2026-11-30)
