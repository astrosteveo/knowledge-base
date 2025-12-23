---
title: Trees
category: Concepts/Data-Structures
tags:
  - data-structures
  - hierarchical
  - interviews
  - recursion
difficulty: intermediate
status: evergreen
date-created: 2025-12-08
date-updated: 2025-12-08
sources:
  - https://www.geeksforgeeks.org/binary-tree-data-structure/
  - https://visualgo.net/en/bst
---

# Trees

> [!summary]
> A tree is a hierarchical data structure consisting of nodes connected by edges, with a single root and no cycles. Trees are everywhere: file systems, HTML DOM, database indices, decision trees in ML, and organizational charts. Binary trees and their variants (BST, heaps, tries) are among the most frequently tested data structures in interviews.

## Theory

### What Is a Tree?

A tree consists of:
- **Root** — The topmost node (no parent)
- **Nodes** — Elements containing data
- **Edges** — Connections between nodes
- **Leaves** — Nodes with no children
- **Subtree** — A node and all its descendants

```
          1           ← Root
        /   \
       2     3        ← Level 1
      / \     \
     4   5     6      ← Level 2 (leaves: 4, 5, 6)
```

### Tree Terminology

| Term | Definition |
|------|------------|
| **Parent** | Node directly above |
| **Child** | Node directly below |
| **Sibling** | Nodes sharing same parent |
| **Depth** | Distance from root (root = 0) |
| **Height** | Longest path to a leaf |
| **Level** | All nodes at same depth |

### Types of Trees

| Type | Description |
|------|-------------|
| **Binary Tree** | Each node has at most 2 children |
| **Binary Search Tree (BST)** | Left < Parent < Right |
| **Balanced Tree** | Height difference between subtrees ≤ 1 |
| **Complete Tree** | All levels full except possibly last |
| **Full Tree** | Every node has 0 or 2 children |
| **Perfect Tree** | All internal nodes have 2 children, all leaves at same level |

### Binary Search Tree Property

```
        8
       / \
      3   10
     / \    \
    1   6    14
       / \   /
      4   7 13

For any node:
- All left descendants < node value
- All right descendants > node value
```

### Time Complexity (BST)

| Operation | Average | Worst (unbalanced) |
|-----------|---------|-------------------|
| Search | $O(\log n)$ | $O(n)$ |
| Insert | $O(\log n)$ | $O(n)$ |
| Delete | $O(\log n)$ | $O(n)$ |

> [!warning] Unbalanced Trees
> A BST can degrade to a linked list if elements are inserted in sorted order, making operations $O(n)$. Use self-balancing trees (AVL, Red-Black) to guarantee $O(\log n)$.

## Practical Examples

### Tree Node Structure

```java
// Binary Tree Node
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    
    TreeNode(int val) {
        this.val = val;
    }
}
```

```python
class TreeNode:
    def __init__(self, val: int):
        self.val = val
        self.left = None
        self.right = None
```

### Tree Traversals (The Big Three)

```java
public class TreeTraversals {
    
    // In-order: Left → Root → Right (BST gives sorted order!)
    public void inorder(TreeNode root) {
        if (root == null) return;
        
        inorder(root.left);
        System.out.print(root.val + " ");
        inorder(root.right);
    }
    
    // Pre-order: Root → Left → Right (useful for copying trees)
    public void preorder(TreeNode root) {
        if (root == null) return;
        
        System.out.print(root.val + " ");
        preorder(root.left);
        preorder(root.right);
    }
    
    // Post-order: Left → Right → Root (useful for deleting trees)
    public void postorder(TreeNode root) {
        if (root == null) return;
        
        postorder(root.left);
        postorder(root.right);
        System.out.print(root.val + " ");
    }
    
    // Level-order (BFS): Level by level
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) return result;
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            List<Integer> level = new ArrayList<>();
            
            for (int i = 0; i < levelSize; i++) {
                TreeNode node = queue.poll();
                level.add(node.val);
                
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
            
            result.add(level);
        }
        
        return result;
    }
}

/*
        1
       / \
      2   3
     / \
    4   5

In-order:   4 2 5 1 3
Pre-order:  1 2 4 5 3
Post-order: 4 5 2 3 1
Level-order: [[1], [2, 3], [4, 5]]
*/
```

```python
from collections import deque
from typing import Optional

def inorder(root: Optional[TreeNode]) -> list[int]:
    """Left → Root → Right"""
    if not root:
        return []
    return inorder(root.left) + [root.val] + inorder(root.right)

def preorder(root: Optional[TreeNode]) -> list[int]:
    """Root → Left → Right"""
    if not root:
        return []
    return [root.val] + preorder(root.left) + preorder(root.right)

def postorder(root: Optional[TreeNode]) -> list[int]:
    """Left → Right → Root"""
    if not root:
        return []
    return postorder(root.left) + postorder(root.right) + [root.val]

def level_order(root: Optional[TreeNode]) -> list[list[int]]:
    """BFS - Level by level"""
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level = []
        for _ in range(len(queue)):
            node = queue.popleft()
            level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(level)
    
    return result
```

### Binary Search Tree Operations

```java
public class BST {
    private TreeNode root;
    
    // Search - O(log n) average
    public TreeNode search(int val) {
        TreeNode current = root;
        
        while (current != null) {
            if (val == current.val) {
                return current;
            } else if (val < current.val) {
                current = current.left;
            } else {
                current = current.right;
            }
        }
        
        return null;  // Not found
    }
    
    // Insert - O(log n) average
    public void insert(int val) {
        root = insertRec(root, val);
    }
    
    private TreeNode insertRec(TreeNode node, int val) {
        if (node == null) {
            return new TreeNode(val);
        }
        
        if (val < node.val) {
            node.left = insertRec(node.left, val);
        } else if (val > node.val) {
            node.right = insertRec(node.right, val);
        }
        // If val == node.val, don't insert (no duplicates)
        
        return node;
    }
    
    // Delete - O(log n) average
    public void delete(int val) {
        root = deleteRec(root, val);
    }
    
    private TreeNode deleteRec(TreeNode node, int val) {
        if (node == null) return null;
        
        if (val < node.val) {
            node.left = deleteRec(node.left, val);
        } else if (val > node.val) {
            node.right = deleteRec(node.right, val);
        } else {
            // Found node to delete
            
            // Case 1: Leaf node
            if (node.left == null && node.right == null) {
                return null;
            }
            
            // Case 2: One child
            if (node.left == null) return node.right;
            if (node.right == null) return node.left;
            
            // Case 3: Two children
            // Find in-order successor (smallest in right subtree)
            TreeNode successor = findMin(node.right);
            node.val = successor.val;
            node.right = deleteRec(node.right, successor.val);
        }
        
        return node;
    }
    
    private TreeNode findMin(TreeNode node) {
        while (node.left != null) {
            node = node.left;
        }
        return node;
    }
}
```

### Common Interview Problems

```python
# Maximum Depth of Binary Tree
def max_depth(root: Optional[TreeNode]) -> int:
    if not root:
        return 0
    
    return 1 + max(max_depth(root.left), max_depth(root.right))


# Check if Tree is Balanced
def is_balanced(root: Optional[TreeNode]) -> bool:
    def check(node):
        if not node:
            return 0
        
        left = check(node.left)
        right = check(node.right)
        
        # -1 signals unbalanced
        if left == -1 or right == -1 or abs(left - right) > 1:
            return -1
        
        return 1 + max(left, right)
    
    return check(root) != -1


# Validate Binary Search Tree
def is_valid_bst(root: Optional[TreeNode]) -> bool:
    def validate(node, min_val, max_val):
        if not node:
            return True
        
        if node.val <= min_val or node.val >= max_val:
            return False
        
        return (validate(node.left, min_val, node.val) and
                validate(node.right, node.val, max_val))
    
    return validate(root, float('-inf'), float('inf'))


# Lowest Common Ancestor (LCA)
def lca(root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
    if not root or root == p or root == q:
        return root
    
    left = lca(root.left, p, q)
    right = lca(root.right, p, q)
    
    if left and right:
        return root  # p and q are in different subtrees
    
    return left if left else right


# Invert Binary Tree (the famous interview question)
def invert_tree(root: Optional[TreeNode]) -> Optional[TreeNode]:
    if not root:
        return None
    
    # Swap children
    root.left, root.right = root.right, root.left
    
    # Recursively invert subtrees
    invert_tree(root.left)
    invert_tree(root.right)
    
    return root


# Path Sum - Does any root-to-leaf path equal target?
def has_path_sum(root: Optional[TreeNode], target: int) -> bool:
    if not root:
        return False
    
    # Leaf node
    if not root.left and not root.right:
        return root.val == target
    
    remaining = target - root.val
    return (has_path_sum(root.left, remaining) or 
            has_path_sum(root.right, remaining))
```

### Iterative Traversals (Without Recursion)

```python
def inorder_iterative(root: Optional[TreeNode]) -> list[int]:
    """Use stack to simulate recursion."""
    result = []
    stack = []
    current = root
    
    while current or stack:
        # Go left as far as possible
        while current:
            stack.append(current)
            current = current.left
        
        # Process node
        current = stack.pop()
        result.append(current.val)
        
        # Go right
        current = current.right
    
    return result


def preorder_iterative(root: Optional[TreeNode]) -> list[int]:
    if not root:
        return []
    
    result = []
    stack = [root]
    
    while stack:
        node = stack.pop()
        result.append(node.val)
        
        # Push right first so left is processed first
        if node.right:
            stack.append(node.right)
        if node.left:
            stack.append(node.left)
    
    return result
```

## Common Patterns

> [!tip] Recursion is Natural for Trees
> Most tree problems have elegant recursive solutions:
> ```python
> def solve(root):
>     if not root:
>         return base_case
>     
>     left_result = solve(root.left)
>     right_result = solve(root.right)
>     
>     return combine(root.val, left_result, right_result)
> ```

> [!tip] Choose Your Traversal
> - **In-order** (BST): Get sorted elements
> - **Pre-order**: Copy/serialize tree
> - **Post-order**: Delete tree, calculate sizes
> - **Level-order**: Level-by-level processing, shortest path

> [!warning] Tree vs Graph
> Trees are a special case of graphs (connected, acyclic). Tree algorithms assume no cycles—if cycles are possible, track visited nodes!

## Edge Cases & Gotchas

- **Null/empty tree** — Always check `if not root`
- **Single node** — It's both root and leaf
- **Skewed tree** — All nodes in one direction (linked list)
- **Duplicate values** — Decide: BST typically uses "left < node ≤ right" or prohibits duplicates
- **Integer overflow** — When summing paths, use `long`

## Related Topics

- [[Binary-Search-Trees]] — In-depth BST coverage
- [[Heaps]] — Complete binary tree with heap property
- [[Tries]] — Tree for string prefix operations
- [[Recursion]] — Most tree solutions are recursive
- [[BFS]] and [[DFS]] — Graph traversal extends to trees

## References

- [Visualgo - BST Visualization](https://visualgo.net/en/bst)
- [GeeksforGeeks - Tree Traversals](https://www.geeksforgeeks.org/tree-traversals-inorder-preorder-and-postorder/)
- [LeetCode Tree Problems](https://leetcode.com/tag/tree/)
