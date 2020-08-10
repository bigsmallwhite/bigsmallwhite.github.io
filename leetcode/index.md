# 集合所有子集


<!--more-->



{{< admonition note "目标">}}

`lst = [1, 2, 3] --> [[], [1], [2], [1,2], [3], [1,3], [2,3], [1,2,3]]`

{{< /admonition >}}

### 思路一：循环

对于任何一个集合，我们先从一个空集合`[[]]`开始，然后一个个添加元素，

从`1`开始，本来是空集`[[]]`，加入`1 --> [[1]]`，

但是不能忘记空集，因为`[1]`的所有子集为`[[], [1]]`，

所以加入一个1后的集合应该是`[[], [1]]`,

加入`2`，需要先把`2`加入已经存在的每一个子集，得到`[2],[1,2]`

然后合并原来的集合 `[[], [1], [2], [1,2]]`

以此类推，新加入一个元素，合并原来所有子集，新加入一个元素，合并原来所有子集……

最后得出  `[[], [1], [2], [1,2], [3], [1,3], [2,3], [1,2,3]]`

**代码如下：**

```python
def allsubsets1(lst):
    result = [[]]
    if len(lst) == 0:
        return result
    for i in range(len(lst)):
        for item in result[:]:
            result.append(item+[lst[i]])
    print(len(result))
    return result
```

### 思路二：递归

递归的本质与循环相同，只不过换了一种形式，
递归是一种逆向思维，

如果想要`[1, 2, 3]`的所有子集，需要知道`[1,2]`的子集，
继而需要`[1]`的子集，`[]`的子集，

然后在最后一层的子集中加入新元素，再合并子集，加入新元素，合并子集……

代码如下：

```python
def allsubsets2(lst):
    if len(lst) == 0:
        return [[]]
    else:
        # 这里等同于 result.append(item+[lst[i]])
        return allsubsets2(lst[1:]) + [[lst[0]] + i for i in allsubsets2(lst[1:])]
```

### 思路三：位运算

将子集与二进制映射

- 0 [] --> 0000
- 1[1] --> 0001
- 2 [2] --> 0010
- 3 [1, 2] --> 0011

可以发现，所有子集其实就是所有元素位置为1或0的排列。
我们知道`len(lst)=N`的集合子集总数为`2^N`个，
而`list(range(2^N))`正好一一对应所有子集为1或0的排列。

代码如下：

```python
def allsubsets3(lst):
    result = []
    if len(lst) == 0:
        return [[]]
    # 遍历0~2^n-1
    for i in range(2 << (len(lst)-1)):
        tmp = []
        # 遍历每位是否为1
        for j in range(len(lst)):
            if i >> j & 1 == 0:
                tmp.append(lst[j])
        result.append(tmp)
    print(len(result))
    return result
```


