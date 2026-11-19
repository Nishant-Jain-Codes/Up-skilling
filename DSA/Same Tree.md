# Same Tree

- Link: https://neetcode.io/problems/same-binary-tree
- Category: Trees · [[DSA-Index]]
- Pattern: recursive DFS on two trees at once
- Language: Python
- Solved on: 2026-11-19 · minutes: 
- Needed the solution? 

## Accepted solution (mine)
```python
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if(p != None and q!=None):
            if(p.val != q.val): 
                return False
            else :
                return self.isSameTree(p.left,q.left) and self.isSameTree(p.right,q.right)
        else: 
            if(p is None and q is None): 
                return True
            else: 
                return False
```

## Approach (own words)
Walk both trees together. If both nodes exist, their values must match and both pairs of children must match too. If only one exists, the shapes differ, so return False. If both are `None`, that branch matches, so return True.

## Complexity
- Time: **O(min(N, M))** in the worst case, which is within your O(N+M) bound. The walk stops as soon as one tree runs out or a value differs, so it never visits more than the smaller tree's nodes.
- Space: **O(H)** for the recursion stack, where H is the height of the smaller tree. O(N) in the worst case if skewed.

## Critique
- Correct. It handles both empty, one empty, different shapes and different values.
- The logic is right but nested: the three cases can be flattened into guard clauses, which is easier to read and to get right:
  1. both `None` → True
  2. one `None` → False
  3. values differ → False
  4. recurse on both sides
- `and` short-circuits, so if the left subtrees differ the right side is never visited.
- Style: use `is None` / `is not None` instead of `!= None`, and drop the parentheses around the conditions.

## Tidier code
```python
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if not p and not q:
            return True
        if not p or not q or p.val != q.val:
            return False
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

## Alternatives
| Approach | Time | Space |
|---|---|---|
| Recursive DFS (mine) | O(min(N, M)) | O(H) |
| Iterative: a queue of pairs `(p, q)`, apply the same checks on each pair | O(min(N, M)) | O(W) |
| Serialise both trees (with `None` markers) and compare the strings | O(N + M) | O(N + M) |

## Mistake / what to remember
- Compare two trees by recursing on both in step, and handle the four cases: both `None`, one `None`, values differ, recurse.
- `None` markers matter: a shape difference counts even when the values match.
- This is the helper for the next problem, [[Subtree of Another Tree]] (#28): "is `subRoot` the same tree as the tree rooted at each node of `root`".
- Revisit: write the guard-clause version from memory at the +14 day review.

## Review
- [ ] +3 days (2026-11-22)
- [ ] +14 days (2026-12-03)
