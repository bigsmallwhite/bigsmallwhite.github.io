# 算法学习


<!--more-->

{{< admonition >}} 本文仅是个人的学习记录，大都是根据网上资源的个人总结{{< /admonition >}}

## 1 辗转相除法求最大公约数

#### 原理：

辗转相除法基于如下原理：两个整数的最大公约数等于其中较小的数和两数相除余数的最大公约数。

例如，252 和 105 的最大公约数是 21；因为 `252 − 105 = 21 × (12 − 5) = 147 `所以 147 和 105 的最大公约数也是 21。在这个过程中，较大的数缩小了，所以继续进行同样的计算可以不断缩小这两个数直至其中一个变成零。这时，所剩下的还没有变成零的数就是两数的最大公约数。由辗转相除法也可以推出，两数的最大公约数可以用两数的整数倍相加来表示，如 21 = 5 × 105 + (−2) × 252 。这个重要的结论叫做裴蜀定理。

思想：不断求较小数与两数余数的最大公约数，实际上是大数不断地减去小数，最后保留的数就是最大公约数

#### Python实现

```python
'''普通计算'''
m = int(input('please input a number:'))
n = int(input('please input a number:'))
max_n, min_n = max(m, n), min(m, n)
r = max_n % min_n
while r != 0:
    max_n = min_n
    min_n = r
    r = max_n % min_n
print('最大公约数：',min_n)

'''利用递归'''
def highcf(m, n):
    m, n = max(m, n), min(m, n)
    r = m % n
    if r == 0:
        return n
    else:
        return highcf(n, r) # 单独写highcf(n, r)可以计算出结果，但是没有返回值

'''直接使用math.gcd()'''
```

## 2 质数判断

思想：除以2一直到平方根，都除不尽就是质数

```python
import math

# 用户输入数字
num = int(input("请输入一个数字: "))
if num > 1: # 质数大于 1
    square_num = math.floor( num ** 0.5 )  # 找到其平方根（ √ ），减少算法时间
    for i in range(2, (square_num+1)):  #将平凡根加1是为了能取到平方根那个值
        if (num % i) == 0:
            print(num, "是合数")
            print(i, "乘于", num // i, "是", num)
            break
    else:
        print(num, "是质数")
else:  # 如果输入的数字小于或等于 1，不是质数
    print(num, "既不是质数，也不是合数")
```

## 3 范围内素数计算

思想：在范围内迭代进行质数判断

```python
def get_prime_number(lower, bigger):
    for num in range(lower, bigger+1):
        if num > 1:
            sqrt = int(num ** 0.5)
            for i in range(2, sqrt+1):
                if num % i == 0:
                    break
            else:
                yield num
        else:
            return 

lower = int(input('please input a lower number:'))
bigger = int(input('please input a bigger number:'))
lst = [i for i in get_prime_number(lower, bigger)]
print(lst)

'''骚气的filter一行语法糖'''
list(filter(lambda x:len(list(filter(lambda y:x%y==0,range(2,int(x**0.5)+1))))==0,range(100,150))) #100-150之内的质数
```

## 4 实现斐波那契数列

思想：迭代和递归

```python
'''递归'''
def fab(n):
    if n == 1:
        return 0
    if n == 2:
        return 1
    else:
        return fab(n-1)+fab(n-2)
list(map(fab,range(1, 10)))
>>>[0, 1, 1, 2, 3, 5, 8, 13, 21]

'''迭代'''
def fab(n):
    arr = [0,1]
    a, b = 0, 1
    while a < n:
        a, b = b, a+b
        arr.append(a)
    return arr
fab(10)
>>>[0, 1, 1, 1, 2, 3, 5, 8, 13]
```

## 5 最小公倍数

```python 
"""两个数的最小公倍数等于两数乘积除以最大公约数"""
def lcm(b,s):
    m = b*s
    while (b!=0) and (s!=0):
        b, s = s, b%s
    return m//b
print(lcm(b,s))
print(lcm(12,8))
>>>24

"""先看大数是否能整除小数，不能的话，就是大数的整倍数"""
def lcm(a, b): #a>b
    count = 2
    if a%b == 0:
        return a
    else:
        while a*count % b != 0:
            count += 1
    return a*count
print(lcm(12, 8))
```

## 6 约瑟夫环问题

30 个人在一条船上，超载，需要 15 人下船。于是人们排成一队，排队的位置即为他们的编号。

报数，从 1 开始，数到 9 的人下船，然后下一个从1开始。如此循环，直到船上仅剩 15 人为止，问都有哪些编号的人下船了呢？

思想：把不相关的元素挪到列表尾部，每挪8个元素后，就把下一个元素剔除掉

```python 
def josephus(nums, step, stay):
    '''nums:总人数; step:摇号下船; stay:留在船上的人数'''
    lst = list(range(1, nums+1))
    drop = []
    while len(lst) > stay:
        i = 1
        while i < step:
            lst.append(lst.pop(0))
            i += 1
        drop.append(lst.pop(0))
    return drop
josephus(30, 9, 28)
>>>[9, 18, 27, 6, 16, 26, 7, 19, 30, 12, 24, 8, 22, 5, 23]
```

## 7 五人分鱼问题

A、B、C、D、E 五人在某天夜里合伙去捕鱼，到第二天凌晨时都疲惫不堪，于是各自找地方睡觉。日上三杆，A 第一个醒来，他将鱼分为五份，把多余的一条鱼扔掉，拿走自己的一份。B 第二个醒来，也将鱼分为五份，把多余的一条鱼扔掉拿走自己的一份。 C、D、E 依次醒来，也按同样的方法拿鱼。问他们台伙至少捕了多少条鱼？

思想：暴力增加鱼的个数，保证连续五次满足分鱼条件

```python
def get_fish():
    fish = 1
    while True:
        total = fish                     #不能直接操作fish，导致死循环，要用total变量代替
        for _ in range(5):
            if (total-1) % 5 == 0:
                total = (total-1)//5*4
            else:
                break
        else:                            #for else 结构，for循环必须完全遍历完成后才会执行else语句，保证五次分鱼
            return fish
        fish += 1
get_fish()
```

## 8 两变量交换

在不采用第三变量的情况下，交换两个变量的值

思路：

- 采用加减法
- 位运算（相同的数异或为0，与0异或为本身）

```
a = a + b;
b = a - b;
a = a - b;
------------
x = x ^ y   // （1）
y = x ^ y   // （2）
x = x ^ y   // （3）
```
