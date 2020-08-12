# 查找数组中重复的数字


<!--more-->

{{< admonition info >}}

找出数组中重复的数字。


在一个长度为 n 的数组 `nums` 里的所有数字都在 `0～n-1` 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中**任意**一个重复的数字。

示例 ：

输入：
`[2, 3, 1, 0, 2, 5, 3]`
输出：`2 或 3` 

{{< /admonition >}}

### 思路一：哈希表

遍历所有的元素，发现不在哈希表内的元素，直接放入哈希表内，否则，`return当前元素`

代码如下：

```python 
def findRepeatNumber(self, nums: List[int]) -> int:
    dic = {}
    for i in nums:
        if i not in dic:
            dic[i] = 1
        else:
            return i
```

### 思路二：数组内查找

思路一消耗了额外的内存空间，如果要求空间复杂度为O(1)，则只能在数组内查找

由于元素的大小小于`len(nums)`，则可以根据列表的索引和元素一一对应的关系来查找重复，如果没有重复，那么元素就一一对应索引，否则，元素与索引就存在多对一的关系。

代码如下：

```python
def findRepeatNumber(self, nums: List[int]) -> int:
        for index, value in enumerate(nums):
            if index != value:
                if value == nums[value]:
                    return value
                # 由于交换的过程中value会变化，所以需要提前记录一下value的值
                temp = value
                value, nums[temp] = nums[temp], value
```


