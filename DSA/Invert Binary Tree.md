# Invert Binary Tree

- Link: https://neetcode.io/problems/invert-a-binary-tree
- Category: Trees · [[DSA-Index]]
- Pattern: recursive DFS, solve the subtrees and then combine
- Language: Python
- Solved on: 2026-11-19 · minutes: 
- Needed the solution? 

## Accepted solution (mine)
```python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if root is None:
            return root

        left = root.left
        right = root.right

        root.left = self.invertTree(right)
        root.right = self.invertTree(left)

        return root
```

## Approach (own words)
An empty tree is its own mirror, so return it. Otherwise save both children, invert each one recursively, and attach them the other way round: the inverted right subtree becomes the left child and the inverted left subtree becomes the right child. Return the same root.

## Complexity
- Time: **O(N)**, every node is visited once.
- Space: **O(H)** for the recursion stack, where H is the tree height. That is O(log N) for a balanced tree and O(N) for a skewed one.

## Critique
- Correct. It handles an empty tree, a single node and a skewed tree.
- Saving `left` and `right` into variables first is the right habit: if you wrote `root.left = self.invertTree(root.right)` and then used `root.left` for the other side, you would lose the original left child.
- `return root` on `None` works, but `return None` says what you mean.
- You don't need the temporaries in Python, since the right-hand side is evaluated fully before the assignment:
  `root.left, root.right = self.invertTree(root.right), self.invertTree(root.left)`. Keep your version if it reads more clearly.

## Alternatives
| Approach | Time | Space |
|---|---|---|
| Recursive DFS (mine) | O(N) | O(H) |
| Iterative DFS with a stack | O(N) | O(H) |
| BFS with a queue: swap the children of each node as you pop it | O(N) | O(W), W = max width |

Iterative BFS, which avoids the recursion limit for very deep trees:
```python
from collections import deque

class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None
        q = deque([root])
        while q:
            node = q.popleft()
            node.left, node.right = node.right, node.left
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
        return root
```

## Mistake / what to remember
- Tree recursion template: base case on `None`, solve the children, combine, return the root.
- Swap safely: save the references first, or use a tuple assignment.
- Python's default recursion limit is about 1000. A skewed tree with more nodes than that needs the iterative version.
- Revisit: write the BFS version from memory at the +14 day review.

## Review
- [ ] +3 days (2026-11-22)
- [ ] +14 days (2026-12-03)
