---
title: Binary Tree
date: "2021-01-17T22:40:32.169Z"
template: "post"
draft: false
slug: "Binary-Tree"
category: "Data Algorithm"
tags:
  - "Data Algorithm"
  - "Leetcode"
description: "Binary Tree"
socialImage: "/media/blog/2021-01-17_01.png"
---

# Binary Tree

- [Binary Tree](#binary-tree)
  - [Introduction](#introduction)
  - [Traverse a Tree](#traverse-a-tree)
    - [Iteration](#iteration)
    - [144. Binary Tree Preorder Traversal](#144-binary-tree-preorder-traversal)
    - [145. Binary Tree Postorder Traversal](#145-binary-tree-postorder-traversal)
    - [94. Binary Tree Inorder Traversal](#94-binary-tree-inorder-traversal)
  - [Level-order Traversal (Breadth-First Search)](#level-order-traversal-breadth-first-search)
    - [102. Binary Tree Level Order Traversal](#102-binary-tree-level-order-traversal)
    - [107. Binary Tree Level Order Traversal II](#107-binary-tree-level-order-traversal-ii)
  - [Recursion](#recursion)
    - [**"Top-down" Solution**](#top-down-solution)
    - [**"Bottom-up" Solution**](#bottom-up-solution)
    - [**Conclusion**](#conclusion)
    - [100. Same Tree](#100-same-tree)
    - [101. Symmetric Tree](#101-symmetric-tree)
    - [112. Path Sum](#112-path-sum)
  - [Construct Binary Tree](#construct-binary-tree)
    - [105. Construct Binary Tree from Preorder and Inorder Traversal](#105-construct-binary-tree-from-preorder-and-inorder-traversal)
    - [106. Construct Binary Tree from Inorder and Postorder Traversal](#106-construct-binary-tree-from-inorder-and-postorder-traversal)
  - [Populating Pointers in Each Node](#populating-pointers-in-each-node)
    - [116. Populating Next Right Pointers in Each Node](#116-populating-next-right-pointers-in-each-node)
  - [LeetCode - Binary Tree 題目](#leetcode---binary-tree-題目)
    - [104. Maximum Depth of Binary Tree (Easy)](#104-maximum-depth-of-binary-tree-easy)
    - [111. Minimum Depth of Binary Tree](#111-minimum-depth-of-binary-tree)
    - [226. Invert Binary Tree](#226-invert-binary-tree)
    - [543. Diameter of Binary Tree](#543-diameter-of-binary-tree)
    - [563. Binary Tree Tilt](#563-binary-tree-tilt)
    - [671. Second Minimum Node In a Binary Tree](#671-second-minimum-node-in-a-binary-tree)
    - [965. Univalued Binary Tree](#965-univalued-binary-tree)



## Introduction

Binary Tree 最容易培養框架思維，大部分算法技巧本質上都是tree traversal.

## Traverse a Tree

- Inorder: left - root - right
- Preorder: root - left - right
- Postorder: left - right - root

![Untitled.png](/media/blog/binary_tree/Untitled.png)

### Iteration

其實是用相反方向的recursion 改造

用iteration 解Binary Tree Traversal都是運用stack 跟 list，

- Stack : 暫時將歷遍的數值儲存下來 + 利用pop( ) 提除Last in value 進入下一個node

 -List: 返回的result 

只是node放進stack 的次序不太一樣而已

![/media/blog/binary_tree/Slide1.jpg](/media/blog/binary_tree/Slide1.jpg)

### 144. Binary Tree Preorder Traversal

[Binary Tree Preorder Traversal - LeetCode](https://leetcode.com/problems/binary-tree-preorder-traversal/)

Given a binary tree, return the *preorder* traversal of its nodes' values.

**Example:**

```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,2,3]
```

**Follow up:** Recursive solution is trivial, could you do it iteratively?

---

**Solution**

```python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        stack = [root]

        while stack:
            node = stack.pop() # extract the last element in stack
            if node:
                res.append(node.val)
                stack.append(node.right)
                stack.append(node.left)
        return res
```

### 145. Binary Tree Postorder Traversal

這題利用了postorder 其實只是先左後右跟#144 調換，

Given a binary tree, return the *postorder* traversal of its nodes' values.

**Example:**

```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [3,2,1]
```

**Follow up:** Recursive solution is trivial, could you do it iteratively?

---

**Solution**

```python
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        stack = [root]

        while stack:
            node = stack.pop() # extract the last element in stack
            if node:
                res.append(node.val)
                print(node.val)
                stack.append(node.left)
                stack.append(node.right)
        return res[: : -1]
```

### 94. Binary Tree Inorder Traversal

[Binary Tree Inorder Traversal - LeetCode](https://leetcode.com/problems/binary-tree-inorder-traversal/)

Given a binary tree, return the *inorder* traversal of its nodes' values.

**Example:**

```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,3,2]
```

**Follow up:** Recursive solution is trivial, could you do it iteratively?

![/media/blog/binary_tree/Slide2.jpg](/media/blog/binary_tree/Slide2.jpg)

---

**Solution**

```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        res, stack = [],[]
        while True:
            while root:
                stack.append(root)
                root = root.left
                print(root)
            if not stack:
                return res
            node = stack.pop()
            res.append(node.val)
            root = node.right
```

## Level-order Traversal (Breadth-First Search)

---

- Traverse the tree level **by level** (層壓式搜索)
- **Breadth-First Search:** algorithm to traverse or search tree / graph
    - 從 root node visit the node itself, then traverse its neighbours → second level neighbours
![/media/blog/binary_tree/Untitled1.png](/media/blog/binary_tree/Untitled1.png)
![/media/blog/binary_tree/Untitled2.png](/media/blog/binary_tree/Untitled2.png)

### 102. Binary Tree Level Order Traversal

Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its level order traversal as:

```
[
  [3],
  [9,20],
  [15,7]
]
```

---

**Solution**

```python
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        if root is None:
            return []
        
        result, curr_level = [], [root] # curr_level default as root node of first level
        
        while curr_level:
            next_level, val = [], [] # clear in each layer to store new val in that level
            
            for node in curr_level:
                val.append(node.val)
                if node.left:
                    next_level.append(node.left)
                if node.right:
                    next_level.append(node.right)
            
            curr_level = next_level
            result.append(val)
            
        return result
```

### 107. Binary Tree Level Order Traversal II

Given inorder and postorder traversal of a tree, construct the binary tree.

**Note:**You may assume that duplicates do not exist in the tree.

For example, given

```
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
```

---

**Method 1: Iteration**

- Top down from root node to get each level node.val list

- WHILE in each layer other than root node, get the value of the nodes and put in to a List

- Why need for loop? Each level may have > 1 node, need to read child node.val of each node

- The list we get will be top down, that's why have to return result in reverse order.

```python
class Solution:
    def levelOrderBottom(self, root: TreeNode) -> List[List[int]]:
        if root is None:
            return []

        result, current = [], [root]

        while current:
            next_level, vals = [ ], [ ]
            
            # Because at 2nd there will be more than 1 node
            for node in current:
                vals.append(node.val)
                if node.left:
                    next_level.append(node.left)
                if node.right:
                    next_level.append(node.right)
            current = next_level
            result.append(vals)

        return result[:: -1]
```

---

## Recursion

- Recursion 是其中一個能有效solve tree 的technique。也是Tree natural feature
- A Tree 可以被定義為a Node 包含 value 及  a list of reference to children nodes
- 每一個recursive function 被call時，我們就會focus 目前的node 並call function recursively 去解決children

---

### **"Top-down" Solution**

- 在每個recursive call 先探訪node並得到value，然後再將value pass to children when calling function recursively
- considered as **preorder** traversal

Recursive function `top_down(root, params)` works like this:

```python
1. return specific value for null node
2. update the answer if needed                      // answer <-- params
3. left_ans = top_down(root.left, left_params)      // left_params <-- root.val, params
4. right_ans = top_down(root.right, right_params)   // right_params <-- root.val, params 
5. return the answer if needed                      // answer <-- left_ans, right_ans
```

**Example: Maximum Depth of a Tree**

`maximum_depth(root, depth)`

```python
1. return if root is null
2. if root is a leaf node:
3.      answer = max(answer, depth)         // update the answer if needed
4. maximum_depth(root.left, depth + 1)      // call the function recursively for left child
5. maximum_depth(root.right, depth + 1)     // call the function recursively for right child
```

---

### **"Bottom-up" Solution**

- call the function recursively for all children nodes
- 根據returned values 及 current node 去找到答案
- considered as **postorder** traversal

a "bottom-up" recursive function `bottom_up(root)` will be something like this:

```python
1. return specific value for null node
2. left_ans = bottom_up(root.left)          // call function recursively for left child
3. right_ans = bottom_up(root.right)        // call function recursively for right child
4. return answers                           // answer <-- left_ans, right_ans, root.val
```

![/media/blog/binary_tree/Untitled3.png](/media/blog/binary_tree/Untitled3.png)

---

### **[Conclusion](https://leetcode.com/explore/learn/card/data-structure-tree/17/solve-problems-recursively/534/#conclusion)**

如何決定什麼時候用`top-down` or `bottom up`?

1. Can you determine parameter to help the node knows it answer?
2. Can you use the parameters and value of the node itself to determine what parameters should be passed to its children? 
1 & 2 Yes → `top-down`
3. If you know the answer of its children, can you calculate the answer of that node? 
Yes → `bottom up`

---

### 100. Same Tree

Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

**Example 1:**

```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

Output: true
```

**Example 2:**

```
Input:     1         1
          /           \
         2             2

        [1,2],     [1,null,2]

Output: false
```

**Example 3:**

```
Input:     1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]

Output: false
```

---

**Logic**

- Case 1: Both tree nodes are null → `True`
- Case 2: If both nodes are existed
    - Check if `p.val == q.val`
    - Recursively call `isSameTree()` for both nodes left children nodes and right
- Return other scenario as `False`

---

**Solution**

```python
class Solution:
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        if not p and not q:
            return True

        if p and q:
            return p.val == q.val and self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)

        return False
```

Error: 把 `if p and q` 寫成 `if p.val == q.val`，但原來 `null` has no attribute `val`

Note: 必須先確定兩個node 是存在才能比較value

```python
Line 11: AttributeError: 'NoneType' object has no attribute 'val'
AttributeError: 'NoneType' object has no attribute 'val'
    if p.val == q.val:
```

### 101. Symmetric Tree

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree `[1,2,2,3,4,4,3]` is symmetric:

```
    1
   / \
  2   2
 / \ / \
3  4 4  3

```

But the following `[1,2,2,null,3,null,3]` is not:

```
    1
   / \
  2   2
   \   \
   3    3
```

**Solution**

```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if not root:
            return True
        
        return self.isSymmetricRecu(root.left, root.right)
    
    def isSymmetricRecu(self, left, right):
        if not left and not right:
            return True
        if not left or not right or left.val != right.val:
            return False
        return self.isSymmetricRecu(left.left, right.right) and self.isSymmetricRecu(left.right, right.left)
```

### 112. Path Sum

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

**Note:** A leaf is a node with no children.

**Example:**

Given the below binary tree and `sum = 22`,

```
      5/ \
    4   8
   /   / \
  11  13  4
 /  \      \
7    2      1
```

return true, as there exist a root-to-leaf path `5->4->11->2` which sum is 22.

---

**Solution**

```python
class Solution:
    def hasPathSum(self, root: TreeNode, sum: int) -> bool:
        if not root:
            return False
        if not root.left and not root.right and root.val == sum:
            return True
        else:
            return self.hasPathSum(root.left, sum - root.val) or self.hasPathSum(root.right, sum - root.val)
```

---

## Construct Binary Tree

### 105. Construct Binary Tree from Preorder and Inorder Traversal

Given preorder and inorder traversal of a tree, construct the binary tree.

**Note:**You may assume that duplicates do not exist in the tree.

For example, given

```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
```

---

![/media/blog/binary_tree/105_106.jpg](/media/blog/binary_tree/105_106.jpg)

- **root** : TreeNode starts with `preorder[0]`
- **index** : look for the index of `preorder[0]` (root) in `Inorder`
- **Left** :
    - `preorder[1: index + 1]` 由index 得知要由root node 後一個element to index + 1 (list[1:2] is extracting 2nd element)
    - `inorder[:index]` stop before root node

---

**Solution**

```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        if preorder == [] or inorder == []:
            return None
        
        root = TreeNode(preorder[0])
        index = inorder.index(preorder[0])
        
        root.left = self.buildTree(preorder[1: index + 1], inorder[:index])
        root.right = self.buildTree(preorder[index + 1:], inorder[index + 1:])
        
        return root
```

---

### 106. Construct Binary Tree from Inorder and Postorder Traversal

Given inorder and postorder traversal of a tree, construct the binary tree.

**Note:**You may assume that duplicates do not exist in the tree.

For example, given

```
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
```

---

![/media/blog/binary_tree/105_106.jpg](/media/blog/binary_tree/105_106.jpg)

---

**Solution**

```python
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        if inorder == [] or postorder == []:
            return None
        
        root = TreeNode(postorder[-1])
        index  = inorder.index(postorder[-1])
        
        root.left = self.buildTree(inorder[:index], postorder[:index])
        root.right = self.buildTree(inorder[index + 1:], postorder[index:-1])
        
        return root
```

---

## Populating Pointers in Each Node

---

### 116. Populating Next Right Pointers in Each Node

You are given a **perfect binary tree** where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

**Example 1:**

![https://assets.leetcode.com/uploads/2019/02/14/116_sample.png](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)

```
Input: root = [1,2,3,4,5,6,7]
Output: [1,#,2,3,#,4,5,6,7,#]
Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```

---

- Use Preorder to link up root.left and root.right
- This is a perfect binary tree so both children would definitely exist
- 得到第一個三角形關係就可以無限call recursion
- 要注意的是 leftNode.right and rightNode.left is connected because knowing it's previous level has .next

---

**Solution**

```python
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        if root and root.left and root.right:
            root.left.next = root.right
            if root.next:
                root.right.next = root.next.left
            self.connect(root.left)
            self.connect(root.right)
        return root
```

---

Algorithm?

**1) The maximum number of nodes at level ‘l’ of a binary tree is .**

$$2^ { l-1}$$

**2) Maximum number of nodes in a binary tree of height ‘h’ .**

$$2h-1$$

---

## LeetCode - Binary Tree 題目

### 104. Maximum Depth of Binary Tree (Easy)

![/media/blog/binary_tree/Untitled4.png](/media/blog/binary_tree/Untitled4.png)

```python
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if root is None:
            return 0
        else:
            ldepth = self.maxDepth(root.left)
            rdepth = self.maxDepth(root.right)
            return max(ldepth + 1, rdepth + 1)
```

1. If tree is empty then return 0
2. Else
    - Get the max depth of left subtree recursively i.e., call maxDepth( tree->left-subtree)
    - Get the max depth of right subtree recursively i.e., call maxDepth( tree->right-subtree)
    - Get the max of max depths of left and right subtrees and add 1 to it for the current node.
    max_depth = max(max dept of left subtree, max depth of right subtree) + 1
    - Return max_depth

### 111. Minimum Depth of Binary Tree

[Minimum Depth of Binary Tree - LeetCode](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

**Example:**

Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its minimum depth = 2.

---

1. If root is None -> 0
2. If is leaf node -> 1
3. If only right / left child -> right / left child + 1
4. If both right & left -> min (left, right) + 1

---

**Solution**

```python
class Solution:
    def minDepth(self, root: TreeNode) -> int:
        if root is None:
            return 0
        if root.left is None and root.right is None:
            return 1
        if root.left is None and root.right is not None:
            return self.minDepth(root.right) + 1
        if root.left is not None and root.right is None:
            return self.minDepth(root.left) + 1

        return min(self.minDepth(root.left), self.minDepth(root.right)) + 1
```

### 226. Invert Binary Tree

[Invert Binary Tree - LeetCode](https://leetcode.com/problems/invert-binary-tree/)

Given a binary search tree, write a function `kthSmallest` to find the **k**th smallest element in it.

**Note:** You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

**Example 1:**

```
Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1
```

**Example 2:**

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
```

**Follow up:** What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

---

**Solution**

```python
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return
        
        right = self.invertTree(root.right)
        left = self.invertTree(root.left)

        root.right = left
        root.left = right

        return root
```

### 543. Diameter of Binary Tree

[Diameter of Binary Tree - LeetCode](https://leetcode.com/problems/diameter-of-binary-tree/)

Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the **longest** path between any two nodes in a tree. This path may or may not pass through the root.

**Example:**Given a binary tree

```
          1
         / \
        2   3
       / \     
      4   5    
```

Return **3**, which is the length of the path [4,2,1,3] or [5,2,1,3].

**Note:** The length of path between two nodes is represented by the number of edges between them.

---

**Solution**

```python
class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        self.ans = 0
        
        def depth(node):
            if not node:
                return 0
            
            left, right = depth(node.left), depth(node.right)
            self.ans = max(self.ans, left + right)
            return max(left, right) + 1
            
        depth(root)
        return self.ans
```

### 563. Binary Tree Tilt

[Binary Tree Tilt - LeetCode](https://leetcode.com/problems/binary-tree-tilt/)

This question is controversial because of the unclear instruction

```python
class Solution:
    def findTilt(self, root):
        self.ans = 0
        def _sum(node):
            if not node: return 0
            left, right = _sum(node.left), _sum(node.right)
            self.ans += abs(left - right)
            return node.val + left + right
        _sum(root)
        return self.ans
```

### 671. Second Minimum Node In a Binary Tree

[Second Minimum Node In a Binary Tree - LeetCode](https://leetcode.com/problems/second-minimum-node-in-a-binary-tree/)

Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly `two` or `zero` sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes. More formally, the property `root.val = min(root.left.val, root.right.val)` always holds.

Given such a binary tree, you need to output the **second minimum** value in the set made of all the nodes' value in the whole tree.

If no such second minimum value exists, output -1 instead.

**Example 1:**

```
Input: 
    2
   / \
  2   5
     / \
    5   7

Output: 5
Explanation: The smallest value is 2, the second smallest value is 5.
```

**Example 2:**

```
Input: 
    2
   / \
  2   2

Output: -1
Explanation: The smallest value is 2, but there isn't any second smallest value.
```

---

這個做法是參考#107 Binary tree level order traversal ii 改動的

1. If root node only -> return `-1`
2. While current level, run through each node and save the value into `result` list
3. Repeat till the bottom level
4. Run through `result` list and extract unique node value into `final` list
5. If length of `final` list <= 1 -> return `-1`
6. Sort the `final` list and return `list[1]`

做完之後才發現自己沒有好好看題目，原來..

- Nodes with non-negative value
- each node has exactly 0 or 2 sub-node
- the sub-nodes value is the larger value

呃... 好吧 至少我的寫法可以覆蓋到更多condition

---

**Method 1: Iteration (沒看solution (也沒看清楚題目...))**

```python
class Solution:
    def findSecondMinimumValue(self, root: TreeNode) -> int:
        if root is None:
            return -1
        
        result, final, current = [], [], [root]

        while current:
            next_level = [ ]

            for node in current:
                result.append(node.val)
                if node.left:
                    next_level.append(node.left)
                if node.right:
                    next_level.append(node.right)
            current = next_level

        for val in result:
            if val not in final:
                final.append(val)

        if len(final) <= 1:
            return -1

        return sorted(final)[1]
```

### 965. Univalued Binary Tree

1. If root is null → `True`
2. If has child (Check left and right child) ?

    No → `False`
    Yes → (3)

3. If child value = root value?

- Recursively check (2) till leaf node

```python
class Solution:
    def isUnivalTree(self, root: TreeNode) -> bool:
        if not root:
            return True

        result_l = (not root.left) or (root.left.val == root.val) and self.isUnivalTree(root.left)
        result_r = (not root.right) or (root.right.val == root.val) and self.isUnivalTree(root.right)
        
        return result_l and result_r
```

Given a binary tree, return the *inorder* traversal of its nodes' values.

**Example:**

```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,3,2]
```

**Follow up:** Recursive solution is trivial, could you do it iteratively?