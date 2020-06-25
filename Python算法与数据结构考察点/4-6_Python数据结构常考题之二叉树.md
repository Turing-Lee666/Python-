# Python数据结构常考题之二叉树

## 二叉树

二叉树涉及到递归和指针操作，常结合递归考察

* 二叉树的操作很多可以用递归的的方式解决，不了解递归会比较吃力
* 常考题：二叉树的镜像

* 常考题：如何层序遍历二叉树(广度优先)

```python
class TreeNode:
    def __init__(self, val, left, right):
        '''对每一个树的节点，他有一个值，还有指向左孩子和右孩子的指针'''
        self.val, self.left, self.right = val, left, right  # 初始化
```

先根遍历(深度优先)：先处理根，然后再去递归地处理左右子树

```python
def pre(root):
    if root:  # 如果根不为空的话
        print(root.val)  # 处理根
        pre(root.left)  # 递归地处理左子树
        pre(root.right)  # 递归地处理右子树
    
    
```





LeetCode 226. 翻转二叉树

每次遇到一个结点的时候，如果它的左右孩子不为空，就可以把左右孩子做一个交换

```python
def invert(root):
    """完成树的左右交换"""
    if root:  # 如果根不为空的话，说明有左右子树
        root.left, root.right =  root.right, root.left  # 把左孩子和右孩子来进行一个交换
        invert(root.left)  # 递归地处理左子树
        invert(root.right)  # 递归地处理右子树
    return root
        
```

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if root:  # 如果root不为空的话
            root.left, root.right = root.right, root.left  # 把root的左右孩子交换
            self.invertTree(root.left)  # 递归处理左子树
            self.invertTree(root.right)  # 递归处理右子树
        return root

```

代码很简单，递归需理解

LeetCode [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root:  # 如果根节点为空，即该二叉树不存在
            return []
        res = []
        cur_nodes = [root]
        next_nodes = []
        res.append([i.val for i in cur_nodes])  # 把第一层的值放进去  res == [[3]]
        while cur_nodes or next_nodes:  # 当前层或者下一层不为空值[]的话
            for node in cur_nodes:  # 遍历当前层的所有节点
                if node.left:  # 如果不为空None
                    next_nodes.append(node.left)
                if node.right:  # 如果不为空None
                    next_nodes.append(node.right)
            if next_nodes:  # 如果不为空值[]
                res.append([i.val for i in next_nodes])  # res添加这一层的值  res == [[3], [9, 20]]
            cur_nodes = next_nodes  # 更新cur_nodes为当前的next_nodes
            next_nodes = []  # 把下一层的节点置为空值
        return res

```

变形：输出左右视角的二叉树结果。利用层序遍历就可以解决，其实只需要把每一层的最左边，最右边拿出来作为结果就可以。

