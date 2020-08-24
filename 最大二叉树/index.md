# 最大二叉树


<!--more-->

这题其实用递归很简单，重点是单调栈的解法~

{{< admonition info >}}

给定一个不含重复元素的整数数组。一个以此数组构建的最大二叉树定义如下：

二叉树的根是数组中的最大元素。

左子树是通过数组中最大值左边部分构造出的最大二叉树。

右子树是通过数组中最大值右边部分构造出的最大二叉树。

通过给定的数组构建最大二叉树，并且输出这个树的根节点。

示例：

输入：

```
[3,2,1,6,0,5]
```

输出：返回下面这棵树的根节点：

```
      6
    /   \
   3     5
    \    / 
     2  0   
       \
        1
```

{{< /admonition >}}

{{< admonition note "方法" >}}

思路一：递归，思路不难

- 找到当前数组的最值，构建节点
- 节点的左子结点-->找到当前数组（上一个节点的左侧）的最值，构建节点；
- 节点的右子结点-->找到当前数组（上一个节点的右侧）的最值，构建节点。

思路二：单调栈，单调栈维护的是严格的单调数组，在本题中，通过递增栈（出栈顺序）构建树

```
stack = []
[]    
3     --> 3入栈[3]
2<3   --> 2入栈[3,2], 3.right = 2
1<1   --> 1入栈[3,2,1], 2.right = 1
6>1   --> 1出栈[3,2], 6.left = 1
6>2   --> 2出栈[3], 6.left = 2
6>3   --> 3出栈[], 6.left = 3
[]
6     --> 6入栈[6]
0<6   --> 0入栈[6,0], 6.right = 0
5>0   --> 0出栈[6], 5.left = 0
5<6   --> 5入栈[6,5], 6.right = 5
return stack[0]
```

{{< /admonition >}}

方法一代码如下：

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution:
    # 递归
    def constructMaximumBinaryTree1(self, nums) -> TreeNode:
        if len(nums) == 0:
            return 
        max_num = max(nums)
        root = TreeNode(max_num)
        root.left = self.constructMaximumBinaryTree1(nums[:nums.index(max_num)])
        root.right = self.constructMaximumBinaryTree1(nums[nums.index(max_num)+1:])
        return root
```

方法二代码如下：

```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution:
    # 单调栈
    def constructMaximumBinaryTree2(self, nums) -> TreeNode:
        stack = list()
        for n in nums:
            cur = TreeNode(n)
            # 当前节点大于栈顶元素，pop的仅次于当前节点的元素是当前元素的左子树根
            while stack and stack[-1].val < n:
                cur.left = stack.pop()
            # 新加入的加点一定小于栈顶元素，一定是栈顶元素的右子树根
            if stack:
                stack[-1].right = cur
            stack.append(cur)
        # 最后的节点最大，为整棵树的根
        return stack[0]
```


