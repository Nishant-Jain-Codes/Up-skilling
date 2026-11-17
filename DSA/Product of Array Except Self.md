# Product of Array Except Self

- Link: https://neetcode.io/problems/products-of-array-discluding-self
- Category: Arrays & Hashing · [[DSA-Index]]
- Pattern: prefix product + suffix product (two passes)
- Language: Python
- Solved on: 2026-11-17 · minutes: 
- Needed the solution? 

## Accepted solution (mine)
```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        # 2 pass flow - prefix product
        # aux-space : O(1)
        # time : O(N)
        out = []
        prefProd = 1
        for i in range(len(nums)):
            out.append(prefProd)
            prefProd *= nums[i]

        sufProd = 1
        for i in range(len(nums) - 1, -1, -1):
            out[i] *= sufProd
            sufProd *= nums[i]

        return out
```

## Approach (own words)
The answer at index `i` is (product of everything to the left of `i`) × (product of everything to the right of `i`). Pass 1, left to right: store the running left product in `out[i]` *before* multiplying in `nums[i]`. Pass 2, right to left: multiply `out[i]` by the running right product, again before including `nums[i]`. No division is used.

## Complexity
- Time: O(N), two passes.
- Space: **O(1) extra**, not counting the output array. Correct, and this is the optimal answer.

## Critique
- Correct and optimal. Your "aux-space O(1)" is the right claim, because the output array doesn't count as extra space.
- The comment says "prefix **sum**", but you're computing a prefix **product**. Say product.
- Zeros are handled for free, because you never divide. The division approach (total product ÷ `nums[i]`) breaks with zeros, so it needs special cases.
- `out = [1] * len(nums)` would let you write `out[i] = prefProd` in pass 1, which is slightly cleaner than `append`.

Tidier version:
```python
class Solution:
    def productExceptSelf(self, nums: list[int]) -> list[int]:
        n = len(nums)
        out = [1] * n
        prefix = 1
        for i in range(n):
            out[i] = prefix
            prefix *= nums[i]
        suffix = 1
        for i in range(n - 1, -1, -1):
            out[i] *= suffix
            suffix *= nums[i]
        return out
```

## Other approaches
| Approach | Time | Space |
|---|---|---|
| Total product ÷ each element | O(N) | O(1), but breaks with zeros and is usually forbidden |
| Separate `prefix[]` and `suffix[]` arrays | O(N) | O(N) extra |
| Prefix in output, suffix as a running variable (mine) | O(N) | O(1) extra |

## Mistake / what to remember
- Update the running product **after** writing it into `out[i]`, so that index `i` is excluded from its own product.
- Pattern: "answer for `i` depends on everything left and right of it" means a prefix pass and a suffix pass. This comes back in Trapping Rain Water and similar problems.
- The O(1) extra space trick is to reuse the output array for the first pass.

## Review
- [ ] +3 days (2026-11-20)
- [ ] +14 days (2026-12-01)
