# 3Sum

- Link: https://neetcode.io/problems/three-integer-sum
- Category: Two Pointers · [[DSA-Index]]
- Pattern: sort, fix one number, then two pointers on the rest (reduces 3Sum to a sorted Two Sum)
- Language: Python
- Solved on: 2026-11-19 · minutes: 
- Needed the solution? 

## Accepted solution (mine)
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        ans = []
        for i in range(0,len(nums)-2):
            first = nums[i]
            si = i+1
            ei = len(nums)-1
            while(si<ei):
                if((nums[si]+nums[ei]+first) == 0 ):
                    val = sorted([nums[si],nums[ei],first])
                    if(val not in ans):
                        ans.append(val)
                    ei-=1
                elif ((nums[si]+nums[ei]+first) > 0):
                    ei-=1
                else :
                    si+=1
        return ans
```
Your stated complexity: time O(N²), auxiliary space O(1).

## Approach (own words)
Sort the array. For each `first`, look for two numbers after it that sum to `-first`, using one pointer at each end of the rest: if the total is too big move the right pointer left, if too small move the left pointer right. When the total is 0, record the triplet (only if it isn't already in the answer) and move on.

## Complexity
Let N = `len(nums)`.
- Sort: O(N log N).
- Pointer part: for each `i` the two pointers cross once, so **O(N²)** in total.
- **The `val not in ans` check is the catch.** It scans the whole answer list, comparing triplets one by one, so each hit costs O(|ans|). With many triplets (|ans| can grow to about N²) the worst case is no longer O(N²). Your O(N²) claim is right for the pointer part only.
- Space: the sort (Timsort) can use O(N) temp space, and the output isn't counted, so "O(1) auxiliary" is true only if you ignore the sort.

## Critique
- Correct. I ran it against a brute-force checker on 3,000 random arrays and found no mismatches.
- Duplicates are handled by checking the answer list at the end, not by skipping repeated values while you move. That's the weak point: it's what pushes the cost above O(N²) in the worst case, and it does repeated work on equal values of `first`.
- `sorted([nums[si], nums[ei], first])` is redundant. The array is already sorted and `i < si < ei`, so `[first, nums[si], nums[ei]]` is already in order.
- On a match you only move `ei`. That's fine, since the next step moves `si` as well, but moving both is the usual pattern.
- Missing early exit: once `first > 0`, no triple can sum to 0, so you can `break`.
- Use a local `total` instead of repeating `nums[si] + nums[ei] + first` three times. Name pointers `left` / `right`.

## Improved version (no `in` check)
Skip duplicates while moving instead of filtering afterwards:
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        res = []
        for i in range(len(nums) - 2):
            if nums[i] > 0:                      # smallest is positive, so no zero sum
                break
            if i > 0 and nums[i] == nums[i - 1]:  # skip a repeated first value
                continue
            left, right = i + 1, len(nums) - 1
            while left < right:
                total = nums[i] + nums[left] + nums[right]
                if total > 0:
                    right -= 1
                elif total < 0:
                    left += 1
                else:
                    res.append([nums[i], nums[left], nums[right]])
                    left += 1
                    while left < right and nums[left] == nums[left - 1]:
                        left += 1                 # skip a repeated second value
        return res
```
This is **O(N²)** in the worst case (checked against the same brute force, 3,000 random arrays, no mismatches).

## Other approaches
| Approach | Time | Space |
|---|---|---|
| Brute force, three loops + set | O(N³) | O(N) |
| Sort + two pointers + `in ans` check (mine) | worst case above O(N²) | O(1) aux, O(N) from the sort |
| **Sort + two pointers + skip duplicates** | **O(N²)** | O(1) aux |
| Fix one, hash set for the other two | O(N²) | O(N) |

## Mistake / what to remember
- 3Sum is Two Sum II on a sorted array, once per element.
- **Deduplicate while you walk** (skip equal neighbours), never by searching the result list. A `in list` check is O(length), and it hides extra cost.
- Skip a repeated `first`, and after a hit skip repeated `left` values.
- Revisit: write the improved version from memory at the +14 day review.

## Review
- [ ] +3 days (2026-11-22)
- [ ] +14 days (2026-12-03)
