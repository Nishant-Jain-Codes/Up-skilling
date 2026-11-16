# Contains Duplicate

- Link: https://neetcode.io/problems/duplicate-integer
- Category: Arrays & Hashing · [[DSA-Index]]
- Pattern: hash set ("seen before?")
- Language: Python
- Solved on: 2026-11-16 · minutes: 
- Needed the solution? 

## Accepted solution (mine)
```python
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        # time O(n)
        # space O(1)   <- wrong, see below
        hash = {}
        for x in nums:
            if x in hash:
                return True
            hash[x] = True
        return False
```

## Approach (own words)
Scan once, remembering each number. If a number is already remembered, it's a duplicate, so return `True`. If the scan finishes, every number was unique, so return `False`.

## Complexity
- Time: O(n) on average. Each lookup and insert is O(1) on average.
- Space: **O(n)**, since the dict can hold every element. My comment said O(1), which was wrong.

## Critique
- Space complexity was mislabeled (O(1) vs O(n)).
- `hash` shadows the built-in `hash()`. Use a `set` named `seen`, since only membership matters, not a value.

```python
class Solution:
    def hasDuplicate(self, nums: List[int]) -> bool:
        seen = set()
        for x in nums:
            if x in seen:
                return True
            seen.add(x)
        return False
```

## Other approaches
| Approach | Time | Space |
|---|---|---|
| Set while scanning | O(n) | O(n) |
| `len(set(nums)) != len(nums)` | O(n) | O(n) |
| Sort, then compare neighbours | O(n log n) | O(1) if sorted in place |

The sort version is the answer if an interviewer says "no extra space".

## Mistake / what to remember
- State space complexity carefully: a dict or set that can grow to n items is O(n).
- "Seen before?" means a set or hash map. This comes back in Two Sum, Group Anagrams and sliding window.

## Review
- [ ] +3 days (2026-11-19)
- [ ] +14 days (2026-11-30)
