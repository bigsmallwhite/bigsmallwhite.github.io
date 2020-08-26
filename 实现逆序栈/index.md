# 实现逆序栈


<!--more-->

偶然间看到一道题，不能使用其他的数据结构，只能用递归，把一个栈的元素反转。

假如一个空栈`[]`,先后入栈元素`3, 2, 1`--> `[3, 2, 1]`,

只利用递归实现`[3, 2, 1] --> [1, 2, 3]`。

**解决该问题的核心思想是：**

如何通过递归算法来弹出**栈底元素**，也就是实现一个`popLast`函数，只要获取了栈底的元素，然后递归的进行逆序就行

```python
def reverseStack(stack):
    # 递归地把栈内元素取出来
    if stack:
        temp = getLast(stack)
        reverseStack(stack)
        # 递归后，后取出的先入栈
        stack.append(temp)
    return stack

def popLast(stack):
    top = stack.pop()
    if not stack:
        return top
    else:
        # 递归的弹出元素
        res = popLast(stack)
        # 把前两次递归弹出的元素按相同的顺序入栈
        stack.append(top)
        return res

if __name__ == '__main__':
    stack = [3, 2, 1]
    print(reverseStack(stack))
    
>>>[1, 2, 3]
```


