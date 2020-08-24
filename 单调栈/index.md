# 单调栈


<!--more-->

做完题后喜欢看看别人的思路，总能发现不一样的方法，让人直呼:call_me_hand::call_me_hand::call_me_hand:

在用递归做完最大二叉树后，也是看到别人用单调栈的方法，发现很巧妙，想着深入学习一下~

{{< admonition note "什么是单调栈" >}}

单调栈指的是：**栈中存放的数据出栈应该是有序的**

**单调递增栈**：栈中数据**出栈**的序列为单调递增序列(栈顶到栈底-->递增)

**单调递减栈**：栈中数据**出栈**的序列为单调递减序列(栈顶到栈底-->递减)

{{< /admonition >}}

栈结构大家都很熟悉（先进后出），那单调栈到底有啥用呢？

单调栈主要回答这样的几种问题:

- 比当前元素**更大的下一个**元素
- 比当前元素**更大的前一个**元素
- 比当前元素**更小的下一个**元素
- 比当前元素**更小的前一个**元素

废话不多说，还是直接用代码解释更简单明了……

#### 更大的下一个

```python
def nextGreaterElement(self, nums):
        '''
        res:'-1'代表没有大于当前元素的值，其他值代表大于当前值的下一个值，下同
        '''
        stack = list()
        res = [-1] * len(nums)
        for i, n in enumerate(nums):
            # 入栈元素只能比栈顶元素小，新元素大于栈顶，就要更新
            while stack and nums[stack[-1]] < n:
                # 这一步执行具体的操作，这里是记录下一个大的数
                res[stack.pop()] = n
            stack.append(i)
        return res
```

#### 更大的前一个

```python
def preGreaterElement(self, nums):
        stack = list()
        res = [-1] * len(nums)
        for i, n in enumerate(nums):
            # 保证先进栈的元素大，如果来个更大的，就把前面小的全部pop掉，保证站内元素是紧密相连的有序序列
            while stack and nums[stack[-1]] < n:
                stack.pop()
            if stack:
                res[i] = nums[stack[-1]]
            stack.append(i)
        return res
```

#### 更小的下一个

```python
def nextSmallerElement(self, nums):
        stack = list()
        res = [-1] * len(nums)
        for i, n in enumerate(nums):
            # 入栈元素只能比栈顶元素大
            while stack and nums[stack[-1]] > n:
                # 这一步执行具体的操作，这里是记录下一个小的数
                res[stack.pop()] = n
            stack.append(i)
        return res
```

#### 更小的前一个

```python
def preSmallerElement(self, nums):
        stack = list()
        res = [-1] * len(nums)
        for i, n in enumerate(nums):
            # 保证先进栈的元素小
            while stack and nums[stack[-1]] > n:
                stack.pop()
            if stack:
                res[i] = nums[stack[-1]]
            stack.append(i)
        return res
```

> 总结：单调栈就是维护一个栈内数据有序的栈，只要栈内元素不是单调的，就一个个把栈顶元素pop掉，直到新加入的元素和剩余的元素单调为止。在pop操作的同时，更新具体需求的操作就行。具体的实例有很多，可以查看上一篇文章[最大二叉树](https://dllyy.xyz/%E6%9C%80%E5%A4%A7%E4%BA%8C%E5%8F%89%E6%A0%91/)的单调栈解法。
