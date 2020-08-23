# 对称、合并、翻转二叉树二叉树


<!--more-->

最近一直在练习二叉树的题:upside_down_face: ，再接再厉:joy:!!!

#### 对称二叉树

{{< admonition info >}}

给定一个二叉树，检查它是否是镜像对称的。例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的。

```
    1
   / \
  2   2
   \   \
   3    3
```

{{< /admonition >}}

{{< admonition note "方法" >}}

思路一：BFS迭代，层次遍历每一层，判断每一层的值是否是回文数

思路二：递归，如果左树的左孩子与右树的右孩子对称，左树的右孩子与右树的左孩子对称，那么这个左树和右树就对称：`def 函数A（左树，右树）：左树节点值等于右树节点值 且 函数A（左树的左子树，右树的右子树）and 函数A（左树的右子树，右树的左子树）为真 才返回真`

{{< /admonition >}}

思路一代码如下：

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution:
    def isSymmetric1(self, root: TreeNode) -> bool:
        if root is None:
            return True
        # 队列存储中间节点
        queue = [root]
        while queue:
            # 存储每一层节点的值
            result = []
            length = len(queue)
            for i in range(length):
                node = queue.pop(0)
                # 如果节点为空，直接添加None，不再进队列
                if not node:
                    result.append(None)
                    continue
                queue.append(node.left)
                queue.append(node.right)
                result.append(node.val)
            # 判断当前层是否对称
            temp = result[::-1]
            if result != temp:
                return False 
        return True
```

思路二代码如下：

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution:
    def isSymmetric2(self, root: TreeNode) -> bool:
        def check(node1, node2):
            # 两个节点都为空，对称
            if not node1 and not node2:
                return True
            # 有一个节点不为空，不对称
            elif not node1 or not node2:
                return False
            # 节点的值不等，不对称
            if node1.val != node2.val:
                return False
            return check(node1.left, node2.right) and check(node1.right, node2.left)
        return check(root, root)
```

#### 合并二叉树

{{< admonition info >}}

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，

否则不为 NULL 的节点将直接作为新二叉树的节点。

示例 1:

输入: 

```
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7         
```

输出: 

合并后的树:

```
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
```

注意: 合并必须从两个树的根节点开始。

{{< /admonition >}}

{{< admonition note "方法" >}}

思路一：递归，不断合并两棵树对应节点的值，如果一个为空，就直接指向另一个节点

思路二：迭代，BFS，广度优先遍历

{{< /admonition >}}

思路一代码如下：

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution:
    # 递归修改树结构
    def mergeTrees1(self, t1: TreeNode, t2: TreeNode) -> TreeNode:
        if not t1:
            return t2
        if not t2:
            return t1
        # 合并根节点
        t1.val += t2.val
        # 迭代左子节点
        t1.left = self.mergeTrees1(t1.left, t2.left)
        # 迭代右子节点
        t1.right = self.mergeTrees1(t1.right, t2.right)
        return t1
    
    #递归不修改树结构
    def mergeTrees2(self, t1: TreeNode, t2: TreeNode) -> TreeNode:
        if not t1:
            return t2
        if not t2:
            return t1
        root = TreeNode(-1)
        root.val = t1.val + t2.val
        root.left = self.mergeTrees2(t1.left, t2.left)
        root.right = self.mergeTrees2(t1.right, t2.right)
        return root
```

思路二代码如下：

```python
class Solution:
    # 迭代，修改树结构(t1)
    def mergeTrees3(self, t1: TreeNode, t2: TreeNode) -> TreeNode:
        if t1 is None:
            return t2
        if t2 is None:
            return t1
        # 这里也可以不用元组结构，但是需要同时pull和push两树对应的节点
        queue = [(t1, t2)]
        while queue:
            n1, n2 = queue.pop(0)
            # 进队列的都不为空,直接赋值相加
            n1.val += n2.val
            # 如果节点的左子节点都不为空，则都进队列
            if n1.left and n2.left:
                queue.append((n1.left, n2.left))
            # 由于是合并到t1，不需要考虑t2左子节点为空,t1左子节点不为空的情况
            if not n1.left:
                n1.left = n2.left
            # 右节点同理
            if n1.right and n2.right:
                queue.append((n1.right, n2.right))
            if not n1.right:
                n1.right = n2.right
        return t1
```

#### 翻转二叉树

{{< admonition info >}}

翻转一棵二叉树。

示例：

输入：

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

输出：

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

{{< /admonition >}}

{{< admonition note "方法" >}}

这道题的主要思路就是从上到下交换子树，再交换子树的子树，循环下去……，把每一个节点的子树交换后，即可。所以核心的任务就是遍历节点了，不管是用递归还是迭代，都行。

思路一：递归，包括前序遍历、中序遍历、后序遍历

思路二：迭代，层次遍历，把节点一个个放在队列中

{{< /admonition >}}

思路一代码如下：

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution:
    
    # 自己想的方法，直接交换镜像位置的节点的值，但是有些案例跑不通，费解……
        #     return root
        # def invert(node1, node2):
        #     if not node1 and not node2:
        #         return 
        #     elif not node1 and node2:
        #         node1 = TreeNode(node2.val)
        #         node2 = TreeNode(None)
        #     elif not node2 and node1:
        #         node2 = TreeNode(node1.val) 
        #         node1 = TreeNode(None)
        #     else:
        #         node1.val, node2.val = node2.val, node1.val
        #     invert(node1.left, node2.right)
        #     invert(node1.right, node2.left)
        # invert(root.left, root.right)
        # return root
    
    # 递归：前序遍历
    def invertTree1(self, root: TreeNode) -> TreeNode:
        if root is None:
            return root
        # 先交换当前节点的左右子树
        root.left, root.right = root.right, root.left
        # 递归左子节点
        self.invertTree1(root.left)
        # 递归右子节点
        self.invertTree1(root.right)
        return root
    
    # 递归：中序遍历
    def invertTree2(self, root: TreeNode) -> TreeNode:
        if root is None:
            return root
        # 先交换左子节点
        self.invertTree2(root.left)
        # 再交换当前节点
        root.left, root.right = root.right, root.left
        # 最后交换右子节点，注意此时的右子节点已经是root.left了
        self.invertTree2(root.left)
        return root
    
    # 递归：后序遍历
    def invertTree3(self, root: TreeNode) -> TreeNode:
        if root is None:
            return root
        # 递归左子节点
        self.invertTree3(root.left)
        # 递归右子节点
        self.invertTree3(root.right)
        # 交换左右子树
        root.left, root.right = root.right, root.left
        return root
```

思路二代码如下：

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution:
    # 层次遍历
    def invertTree4(self, root: TreeNode) -> TreeNode:
        if root is None:
            return root
        queue = [root]
        while queue:
            node = queue.pop(0)
            node.left, node.right = node.right, node.left
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        return root
```

> 总结：对于二叉树的题，递归是最常见的思路，相对于其他思路也比较简单，但是要尽量思考一下非递归（如层次遍历）的解决思路，加深理解。
