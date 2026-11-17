# Longest Consecutive Sequence

- Link: https://neetcode.io/problems/longest-consecutive-sequence
- Category: Arrays & Hashing · [[DSA-Index]]
- Pattern: hash set + expand from each element; each number is visited once
- Language: Python
- Solved on: 2026-11-17 · minutes: 
- Needed the solution? 

## Accepted solution (mine)
```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        # time : O(N)
        # space : O(N)
        hsh = set(nums)
        ans = 0

        for x in list(hsh):
            if x not in hsh:
                continue

            curcnt = 1
            incVal = x + 1
            decVal = x - 1

            while incVal in hsh:
                curcnt += 1
                hsh.remove(incVal)
                incVal += 1

            while decVal in hsh:
                curcnt += 1
                hsh.remove(decVal)
                decVal -= 1

            hsh.remove(x)
            ans = max(ans, curcnt)

        return ans
```

## Approach (own words)
Put all numbers in a set. For each number still in the set, walk upward (`x+1`, `x+2`, …) and downward (`x-1`, `x-2`, …) as long as the neighbours exist, counting the run. Remove every number as it's counted, so no number is part of a second walk. The answer is the longest run.

## Complexity
- Time: **O(N)**. Each number is removed from the set once, and each `in` check is O(1) on average.
- Space: O(N) for the set, plus O(N) for the `list(hsh)` copy. Still O(N) overall.

## Critique
- Correct, including the empty array (returns 0) and duplicates (the set removes them).
- Iterating over `list(hsh)` is needed because you modify the set inside the loop. Without the copy you'd get "Set changed size during iteration". Good that you spotted it.
- Removing visited numbers is what keeps it linear. Without it, you'd rewalk the same run from every element and become O(N²).
- The more common version doesn't mutate the set. It only starts counting from a number whose predecessor is missing (`x - 1 not in nums`), so each run is walked exactly once.

Tidier version (no mutation, no list copy):
```python
class Solution:
    def longestConsecutive(self, nums: list[int]) -> int:
        nums_set = set(nums)
        best = 0
        for x in nums_set:
            if x - 1 not in nums_set:       # x starts a sequence
                length = 1
                while x + length in nums_set:
                    length += 1
                best = max(best, length)
        return best
```

## Other approaches
| Approach | Time | Space |
|---|---|---|
| Sort, then count runs (skip duplicates) | O(N log N) | O(1) if sorted in place |
| Set, expand and delete visited (mine) | O(N) | O(N) |
| Set, start only at sequence starts | O(N) | O(N) |

The nested `while` looks like O(N²), but it isn't. Each number is walked once as part of its run. Be ready to explain that in an interview.

## Mistake / what to remember
- The O(N) claim only holds because a number never gets walked twice. Say why, not just "it's O(N)".
- Pattern: "consecutive / adjacent values" means a set for O(1) neighbour checks.
- Variable names: `hsh` is a fine name; `incVal` / `decVal` could be `up` / `down`.

## Review
- [ ] +3 days (2026-11-20)
- [ ] +14 days (2026-12-01)
