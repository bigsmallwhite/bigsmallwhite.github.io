# 平衡二叉树判断


<!--more-->

{{< typeit >}}

**依旧是递归的思想！**{{< /typeit >}}

{{< admonition note"题目">}}

一棵高度平衡二叉树定义为：一个二叉树每个节点的左右两个子树的高度差的绝对值不超过1。给定一个二叉树，判断它是否是高度平衡的二叉树。{{< /admonition >}}

示例如下：

```
示例 1:
给定二叉树 [3,9,20,null,null,15,7]
    3
   / \
  9  20
    /  \
   15   7
返回 true 

示例 2:
给定二叉树 [1,2,2,3,3,null,null,4,4]
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
返回 false 
```

#### 思路一：暴力迭代

从本质上讲，要求所有子树的高度差不大于1，

所以我们直接暴力求解，遍历所有节点的左右子树高度，求差，看是否符合要求。

代码：

```python
class Solution:
    # 求高度
    def getHight(self, node):
        if node is None:
            return 0
        else:
            return max(self.getHight(node.left), self.getHight(node.right)) + 1

    # 暴力遍历每个节点的左右子树高度差
    def isBalanced1(self, root: TreeNode) -> bool:
        if root is None:
            return True
        # 判断左右子树是否平衡，如果平衡，看高度差是否大于1
        if (self.isBalanced1(root.left) and self.isBalanced1(root.right) and 
        abs(self.getHight1(root.left) - self.getHight1(root.right)) <= 1):
            return True
        else:
            return False
```

#### 思路二：自下而上递归

递归，实际上对思路一的优化，

思路一中存在大量重复的计算，按理说只需要从下往上计算一次就应该可以判断出来，

设置一个全局变量`flag = True`，

递归地计算每一层左右子树的高度，遇到差值大于1的，更改全局变量`flag = False`，

最后返回全局变量。

代码如下：

```python
class Solution:
    # 定义全局控制变量
    def __init__(self):
        self.flag = True
  
    # 从下往上的一次性递归判断
    def isBalanced2(self, root):
        self.getHight2(root)
        return self.flag

    def getHight2(self, node):
        if node is None:
            return 0
        left = self.getHight2(node.left)
        right = self.getHight2(node.right)
        if abs(left-right) > 1:
            self.flag = False
        return max(left, right) + 1
```

{{< admonition question >}}

其实上述递归的方法还有一个缺点就是没有在发现**已经不是平衡树**的时候提前退出递归，

网上大多给出的方法是通过对全局变量来捕获异常，但是捕获异常的机制反而导致运行时间更长，不如直接一层层退出递归。

{{< /admonition >}}

#### 思路三：多值递归

思路二需要设置一个全局变量，

我们可以让返回的信息应该是既包含子树的深度的`int类型`的值，又包含子树是否是平衡二叉树的`bool类型`的值

代码如下：

```python
# 定义返回的结果类，包括树的深度和子树是否平衡的bool值
class Result:
    def __init__(self, detpth, isb):
        self.detpth = detpth
        self.isb = isb

class Solution:
    def main(self, root):
        # 最后返回"返回值"的布尔值
        return self.isBalanced3(root).isb

    def isBalanced3(self, node):
        if node is None:
            return Result(0, True)
        left = self.isBalanced3(node.left)
        right = self.isBalanced3(node.right)
        # 非平衡的情况：任意子树不平衡、左右子树高度差大于1
        if left.isb is False or right.isb is False:
            return Result(0, False)
        if abs(left.detpth - right.detpth) > 1:
            return Result(0, False)
        return Result(max(left.detpth, right.detpth)+1, True)
```


