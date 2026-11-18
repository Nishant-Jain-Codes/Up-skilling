# Number of 1 Bits

- Link: https://neetcode.io/problems/number-of-one-bits
- Category: Bit Manipulation · [[DSA-Index]]
- Pattern: check each bit with shift and mask (`(n >> i) & 1`)
- Language: Python
- Solved on: 2026-11-18 · minutes: 
- Needed the solution? 

## Accepted solution (mine)
```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        i = 0
        count = 0
        while(n>>i):
            count += (n>>i)&1
            i+=1
        return count
```

## Approach (own words)
Shift `n` right by `i` and mask with `& 1` to read bit `i`. Add it to the count. Keep going while shifting still leaves something non-zero, which means there are higher set bits left. When `n >> i` is 0, every set bit has been counted.

## Complexity
- Time: **O(log n)**, one pass per bit up to the highest set bit. For a 32-bit input that's at most 32 steps, so effectively O(1).
- Space: **O(1)**.

## Critique
- Correct, and the loop condition `n >> i` is neat: it stops at the highest set bit instead of always running 32 times. `n = 0` returns 0.
- It only works because `n` is non-negative here. In Python an int is unbounded, so for a negative `n`, `n >> i` never reaches 0 and the loop never ends. The problem's constraints rule that out. If a negative input were allowed, you'd mask first (`n & 0xFFFFFFFF`).
- It does `n >> i` twice per step. Shifting `n` itself is simpler and avoids the index:
```python
while n:
    count += n & 1
    n >>= 1
```
- Style: drop the parentheses around the `while` condition, and add spaces around `+=` and `>>`.

## Other approaches
| Approach | Time | Space |
|---|---|---|
| Shift and mask every bit (mine) | O(log n) | O(1) |
| **Brian Kernighan: `n &= n - 1`** | **O(number of set bits)** | O(1) |
| `n.bit_count()` (Python 3.10+) or `bin(n).count("1")` | O(log n) | O(1) |

Brian Kernighan's trick is the one to remember. `n & (n - 1)` clears the lowest set bit, so the loop runs once per 1 bit, not once per position:
```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        count = 0
        while n:
            n &= n - 1     # drop the lowest set bit
            count += 1
        return count
```
Why it works: subtracting 1 flips the lowest set bit to 0 and every bit below it to 1, so `&` with the original keeps everything above and clears that bit.

## Mistake / what to remember
- Counting bits: shift and mask is the baseline, and `n &= n - 1` is the faster follow-up.
- `n & (n - 1)` also checks power of two (`n > 0 and n & (n - 1) == 0`). That comes up again in other bit problems.
- Revisit: write the Kernighan version from memory at the +14 day review.

## Review
- [ ] +3 days (2026-11-21)
- [ ] +14 days (2026-12-02)
