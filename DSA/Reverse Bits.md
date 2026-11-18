# Reverse Bits

- Link: https://neetcode.io/problems/reverse-bits
- Category: Bit Manipulation · [[DSA-Index]]
- Pattern: read bits low to high and build the result by shifting left (`ans = ans * 2 + bit`)
- Language: Python
- Solved on: 2026-11-18 · minutes: 
- Needed the solution? 

## Accepted solution (mine)
```python
class Solution:
    def reverseBits(self, n: int) -> int:
        # 0...10101
        i = 0
        ans = 0

        while i<32:
            # print(n&(1<<i))
            ans *=2
            if(n&(1<<i)>0):
                ans+=1
            i+=1
        return ans
```

## Approach (own words)
Go through bits 0 to 31 of `n`, from the lowest to the highest. For each one, double `ans` (shift it left by one) and add 1 if that bit of `n` is set. Bit 0 goes in first, so after 32 doublings it ends up as the top bit of `ans`. Bit 31 goes in last and lands at position 0. That is the reversal.

## Complexity
- Time: **O(32)**, which is O(1) for a fixed-width input.
- Space: **O(1)**.

## Critique
- Correct. I checked it on `0b10101` (gives `0b10101` followed by 27 zeros), `1` (gives `2**31`), `0` and all ones.
- Doing the `ans *= 2` before the check, for every one of the 32 steps (including zero bits), is what keeps each bit's position right. Putting the doubling inside the `if` is the classic bug here.
- `n&(1<<i)>0` is fine in Python: `&` binds tighter than `>`, so it reads as `(n & (1<<i)) > 0`. In C/C++ the precedence is the opposite and the same expression would be parsed as `n & ((1<<i) > 0)`. You know C++, so keep the explicit parentheses habit.
- Simpler: `ans = (ans << 1) | ((n >> i) & 1)`, which drops the `if`.
- Style: spaces around operators, no parentheses around the `if` condition, and `for i in range(32)` instead of a manual counter.

## Other approaches
| Approach | Time | Space |
|---|---|---|
| Bit by bit, 32 steps (mine) | O(32) | O(1) |
| String: `int(format(n, "032b")[::-1], 2)` | O(32) | O(32) |
| **Divide and conquer: swap halves, then quarters, and so on with masks** | O(log 32) = 5 steps | O(1) |

The mask-swap version is the follow-up an interviewer might ask for ("called many times, can you optimise?"):
```python
class Solution:
    def reverseBits(self, n: int) -> int:
        n = ((n & 0xFFFF0000) >> 16) | ((n & 0x0000FFFF) << 16)
        n = ((n & 0xFF00FF00) >> 8)  | ((n & 0x00FF00FF) << 8)
        n = ((n & 0xF0F0F0F0) >> 4)  | ((n & 0x0F0F0F0F) << 4)
        n = ((n & 0xCCCCCCCC) >> 2)  | ((n & 0x33333333) << 2)
        n = ((n & 0xAAAAAAAA) >> 1)  | ((n & 0x55555555) << 1)
        return n
```

## Mistake / what to remember
- Reverse by building: read from the low end, push onto the result from the low end with `ans = ans * 2 + bit`.
- Shift first, then add the bit, and do it for all 32 positions.
- Operator precedence differs between Python and C++, so use parentheses around bit tests.
- Revisit: write the one-line update `ans = (ans << 1) | ((n >> i) & 1)` from memory at the +14 day review.

## Review
- [ ] +3 days (2026-11-21)
- [ ] +14 days (2026-12-02)
