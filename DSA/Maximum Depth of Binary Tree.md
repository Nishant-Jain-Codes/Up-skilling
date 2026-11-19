# Maximum Depth of Binary Tree

- Link: https://neetcode.io/problems/depth-of-binary-tree
- Category: Trees · [[DSA-Index]]
- Pattern: recursive DFS, return a value up from the subtrees
- Language: Python
- Solved on: 2026-11-19 · minutes: 
- Needed the solution? 

## Accepted solution (mine)
```python
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if(root is None):
            return 0
        leftCount = self.maxDepth(root.left)
        rightCount = self.maxDepth(root.right)
        return 1 + max(leftCount,rightCount)
```

## Approach (own words)
An empty tree has depth 0. Otherwise the depth is 1 (this node) plus the deeper of the two subtrees. Ask each child for its depth, take the larger, add one.

## Complexity
- Time: **O(N)**, every node is visited once.
- Space: **O(H)** for the recursion stack, where H is the height. O(log N) if balanced, O(N) if skewed.

## Critique
- Correct, and it is the standard answer. It handles an empty tree, one node and a skewed tree.
- Style: drop the parentheses around the `if` condition, put spaces after the comma, and name the variables `left` and `right` (they are depths, not counts). The one-liner is also fine: `return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right))`.
- Same shape as [[Invert Binary Tree]]: base case on `None`, solve both children, combine. Most easy tree problems follow this template.

## Alternatives
| Approach | Time | Space |
|---|---|---|
| Recursive DFS (mine) | O(N) | O(H) |
| Iterative DFS with a stack of (node, depth) | O(N) | O(H) |
| BFS level by level: count the levels | O(N) | O(W), W = max width |

BFS version, which avoids the recursion limit:
```python
from collections import deque

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        q, depth = deque([root]), 0
        while q:
            for _ in range(len(q)):
                node = q.popleft()
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            depth += 1
        return depth
```

## Mistake / what to remember
- Depth = 1 + max(left depth, right depth), with 0 for `None`.
- The base case returns 0 here, not `None`, because the caller adds to it.
- Revisit: write the level-by-level BFS from memory at the +14 day review, because it is the base for level-order traversal.

## Review
- [ ] +3 days (2026-11-22)
- [ ] +14 days (2026-12-03)
