# Two Sum

- Link: https://neetcode.io/problems/two-integer-sum
- Category: Arrays & Hashing · [[DSA-Index]]
- Pattern: hash map of complements ("have I seen the number I need?")
- Language: Python
- Solved on: 2026-11-16 · minutes: longer than expected (accepted on the 6th submission)
- Needed the solution? 
- What failed on the earlier submissions? (fill in)

## Accepted solution (mine)
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hsh = {}
        for i in range(len(nums)):
            complement = target - nums[i]
            if complement in hsh:
                return [hsh[complement], i]
            hsh[nums[i]] = i
        return [-1, -1]
```

## Approach (own words)
For each number, work out which number would complete the sum (`target - nums[i]`). If I've already seen it, return its stored index and the current index. If not, store the current number with its index and move on.

## Complexity
- Time: O(n), one pass with O(1) average lookups.
- Space: O(n) for the dict.

## Critique
- Correct. Checking the complement **before** inserting the current number is the key detail. It means an element is never paired with itself (`[3, 2, 4]`, target 6 doesn't return `[0, 0]`), and duplicates still work (`[3, 3]`, target 6 returns `[0, 1]`).
- The returned order is smaller index first, which is what the problem wants.
- `hsh` is a good name; it doesn't shadow the built-in like `hash` did before.
- Use `enumerate` instead of `range(len(nums))`.
- `return [-1, -1]` is never reached, because the problem guarantees one answer. Harmless, but it shows you know the edge case.

Tidier version:
```python
class Solution:
    def twoSum(self, nums: list[int], target: int) -> list[int]:
        seen: dict[int, int] = {}  # value -> index
        for i, x in enumerate(nums):
            if target - x in seen:
                return [seen[target - x], i]
            seen[x] = i
        return []
```

## Other approaches
| Approach | Time | Space |
|---|---|---|
| Brute force, two loops | O(n²) | O(1) |
| One-pass hash map (mine) | O(n) | O(n) |
| Sort + two pointers | O(n log n) | O(n) in Python, because you must keep the original indices |

Sort + two pointers loses the original indices unless you store them, which is why the hash map wins here. It's the right approach for the sorted-input variant, Two Sum II.

## Mistake / what to remember
- Take the time to write down why the complement check goes before the insert. That ordering is the whole problem.
- Habit to build: before coding, say the brute force first, then ask "what am I re-searching for each time?". That's the sentence that leads to the hash map.
- Pattern: "find a pair that satisfies X" often means a hash map of what you've already seen.

## Review
- [ ] +3 days (2026-11-19)
- [ ] +14 days (2026-11-30)
