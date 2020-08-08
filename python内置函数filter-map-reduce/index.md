# Python filter,map,reduce


<!--more-->

> `filter`、`map`、`reduce`，都是对一个集合进行处理，**filter** 用于过滤，**map** 用于映射，**reduce** 用于归并。是 `Python` 列表方法的三架马车。

## 1 filter 

> `filter`会根据提供的函数对指定序列做过滤。

`filter` 函数的定义：`filter(function or None, sequence) -> list, tuple, or string`

`function` 是一个谓词函数，接受一个参数，返回布尔值 `True` 或 `False`。

`filter` 函数会对序列参数 `sequence` 中的每个元素调用 `function` 函数，最后返回的结果包含调用结果为 `True` 的元素。

返回值的类型和参数 `sequence` 的类型相同

比如返回序列中的所有偶数：

```python 
def is_even(x):
    return x & 1 != 0
filter(is_even, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
>>>[1, 3, 5, 7, 9]
```

{{< admonition >}}

如果 `function` 参数为 `None`，返回结果和 sequence 相同。

```python 
filter(None, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
>>>[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

{{< /admonition >}}

## 2 map

> `map` 函数会根据提供的函数对指定序列做映射。

`map` 函数的定义：`map(function, sequence[, sequence, ...]) -> list`

通过定义可以看到，这个函数的第一个参数是一个函数，剩下的参数是一个或多个序列，返回值是一个集合。

`function` 可以理解为是一个一对一或多对一函数，map 的作用是以参数序列中的每一个元素调用 `function` 函数，返回包含每次 `function` 函数返回值的 `list`。

比如要对一个序列中的每个元素进行**平方**运算：

```python
map(lambda x: x ** 2, [1, 2, 3, 4, 5])
[1, 4, 9, 16, 25]
```

在参数存在多个序列时，会依次以每个序列中相同位置的元素做参数调用 `function` 函数。

比如要对两个序列中的元素依次求和。

```python
map(lambda x, y: x + y, [1, 3, 5, 7, 9], [2, 4, 6, 8, 10])
```

`map` 返回的 

- `list[0]` :`参数序列 1 的第一个元素加参数序列 2 中的第一个元素 (1 + 2)`
- `list[1]`:`参数序列 1 中的第二个元素加参数序列 2 中的第二个元素 (3 + 4)`
- ……
- 依次类推，最后的返回结果为：`[3, 7, 11, 15, 19]`

要注意 `function` 函数的参数数量，要和 `map` 中提供的集合数量相匹配。

而且如果集合长度不相等，会以最小长度对所有集合进行截取。

{{< admonition tip>}}

当函数为 None 时，操作和 zip 相似：

```python
map(None, [1, 3, 5, 7, 9], [2, 4, 6, 8, 10])
>>>[(1, 2), (3, 4), (5, 6), (7, 8), (9, 10)]
```

{{< /admonition >}}

## 3 reduce

> `reduce` 函数会对参数序列中元素进行累积

`reduce` 函数的定义：`reduce(function, sequence[, initial]) -> value`

`function` 参数是一个有**两个参数**的函数，`reduce` 依次从 `sequence` 中取一个元素，和上一次调用 `function` 的结果做参数再次调用 `function`。

{{< admonition >}}

第一次调用 `function` 时，如果提供 `initial` 参数，会以 `sequence` 中的第一个元素和 `initial` 作为参数调用 `function`，否则会以序列 `sequence` 中的前两个元素做参数调用 `function`。

```python
from functools import reduce #已经从全局命名空间删除，需要从functools导入

reduce(lambda x, y: x + y, [2, 3, 4, 5, 6], 1) #1为initial参数
结果为 21 ((((((1+2)+3)+4)+5)+6))

reduce(lambda x, y: x + y, [2, 3, 4, 5, 6])
结果为 20
```

注意 function 函数不能为 None。

{{< /admonition >}}

## 4 应用实例

4.1、**用 `map` 和 `reduce` 实现 5 的阶乘相加（5!+4!+3!+2!+1!）**

```python
>>>print (reduce(lambda x,y:x*y,range(1,6)))    120
>>>print (reduce(lambda x,y:x*y,range(1,5)))    24
>>>print (reduce(lambda x,y:x*y,range(1,4)))    6
>>>print (reduce(lambda x,y:x*y,range(1,3)))    2
>>>print (reduce(lambda x,y:x*y,range(1,2)))    1
```

把上一步的结果变成一个阶乘列表

```python
>>>print(map(lambda a:reduce(lambda x,y:x*y,range(1,a+1)),range(1,6)))
[1, 2, 6, 24, 120]
```

最后把阶乘列表相加，第一题解决

```python 
>>>print(reduce(lambda m,n:m+n,map(lambda a:reduce(lambda x,y:x*y,range(1,a+1)),range(1,6))))
153
```

4.2、**用 `filter` 将 100~200 以内的质数过滤出来**

质数，又称素数，指在一个大于 1 的自然数中，除了 1 和此整数自身外，不能被其他自然数整除的数

```python
>>>list(filter(lambda x:len(list(filter(lambda y:x%y==0,range(2,int(x**0.5)+1))))==0,range(23,30)))
#不能直接对迭代器使用len()函数，先转化为list
```
