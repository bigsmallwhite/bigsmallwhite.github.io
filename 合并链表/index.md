# 合并有序链表


<!--more-->



{{< admonition info >}}

将两个升序链表合并为一个新的升序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

示例：

输入：`1->2->4`;`1->3->4`

输出：`1->1->2->3->4->4`

{{< /admonition >}}

## 思路一：迭代

合并有序链表其实和归并排序中合并有序序列一模一样，所以可以完全参照归并排序，

**就是不断比较两个链表的头节点，如果某一个链表迭代完成，另一个链表还有节点，直接指向剩余的链表即可。**

代码：

```python
class ListNode:
    def __init__(self, val=None, next=None):
        self.val = val
        self.next = next


class Solution:

    # 迭代
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        newlist = ListNode(None)
        # head为指向头部的游标
        head = newlist
        while l1 and l2:
            if l1.val < l2.val:
                head.next = ListNode(l1.val)
                l1 = l1.next
            else:
                head.next = ListNode(l2.val)
                l2 = l2.next
            # 注意游标的位置要不断后移
            head = head.next
        
        # 这里可以优化一下，直接head.next = l1 or l2就行，因为有序
        # head.next = l1 or l2
        
        while l1:
            head.next = ListNode(l1.val)
            l1 = l1.next
            head = head.next
            
        while l2:
            head.next = ListNode(l2.val)
            l2 = l2.next
            head = head.next
        return newlist.next
```

## 思路二：递归

递归：**递归是逆向思维，先理清逻辑关系，再返回实际结果**

以`[1,2,3]`和`[4,5,6]`为例

先对比头结点`1`和`4`，很明显`1`放在最前面，

后面的结果如何先不管，但是逻辑上，只需要求出`[2,3]`和`[4,5,6]`的合并结果`[……]`，然后`1-->[……]`即可，

继续求`[……]`……

对比`2`和`4`，很明显`2`在前面，以此类推，具体过程如下：

```
merge([1,2,3], [4,5,6])
     1 -- > merge([2,3], [4,5,6])
​           2 -- > merge([3], [4,5,6])
​                  3 -- > merge([], [4,5,6])
​                         [4,5,6]

==>[1,2,3,4,5,6]
```

代码：

```python
class ListNode:
    def __init__(self, val=None, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:

        # 终止条件
        if not l1:
            return l2
        if not l2:
            return l1

        if l1.val < l2.val:
             l1.next = self.mergeTwoLists2(l1.next, l2)
             # 哪个链表开头就返回哪个链表
             return l1
        else:
            l2.next = self.mergeTwoLists2(l2.next, l1)
            return l2
```

可以明显发现，递归给人的感觉更加简洁，但是相对理解起来也更加有难度。
