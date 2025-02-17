# 044 解压缩算法

## 题目描述

现需要实现一种算法，能将一组压缩字符串还原成原始字符串，还原规则如下：
1．字符后面加数字`N`，表示重复字符`N`次。例如：压缩内容为`A3`，表示原始字符串为`AAA`。
2．花括号中的字符串加数字`N`，表示花括号中的字符串重复`N`次。例如：压缩内容为`{AB}3`，表示原始字符串为`ABABAB`。
3．字符加`N`和花括号后面加`N`，支持任意的嵌套，包括互相嵌套。例如：压缩内容可以`{A3B1{C}3}3`。

## 输入描述

输入一行压缩后的字符串。

## 输出描述

输出压缩前的字符串。

**说明：**

- 输入输出字符串区分大小写。
- 输入的字符串长度为范围[1,10000]。
- 输出的字符串长度为范围[1,100000]。
- 数字`N`范围[1,10000]。

## 示例描述

### 示例一

**输入：**
```text
A3
```

**输出：**
```text
AAA
```
**说明：**

`A3`代表`A`字符重复3次。

### 示例二

**输入：**
```text
{A3B1{C}3}3
```

**输出：**
```text
AAABCCCAAABCCCAAABCCC
```

**说明：**

`{A3B1{C}3}3`代表`A`字符重复3次、`B`字符重复1次、花括号中的`C`字符重复3次，最外层花括号中的`AAABCCC`重复3次。

## 解题思路

**基本思路：**

使用栈（数字不进栈）求解，将数字的结束作为计算的标志。
1. 若当前元素为数字，需要单独存储数字（因为数字可能是多位数）。
2. 若当前元素不为数字（进行计算）： 
	- 若当前数字域不为空，判断栈顶元素：
		- 若栈顶元素为字母：将字母累计次数后，重新存入栈顶。
		- 若栈顶元素为`}`，则寻找最近的`{`，将中间的字母域合并，并累计次数后，重新存入栈顶。
	- 将当前元素压入栈顶。
3. 原始字符串末尾添加结束符`#`，确保最后一个数字得到计算。

**代码思路：**

1. 原始字符串末尾添加结束符`#`，确保最后一个数字得到计算。
2. 初始化数字字符串`num`和字母字符串`temp`。
3. 遍历所有字符串：
   - 若当前元素为数字，单独存储数字到数字字符串`nums`（因为数字可能是多位数）
   - 若当前元素不为数字，若数字字符串不为空，将`{`和`}`中的字符串重复`N`次，然后将当前元素存入栈中。
4. 拼接栈内元素（记得排除结束符`#`）。

## 解题代码
```python
def solve_method(chars):
    chars += "#"
    stack = []
    num, temp = 0, ""
    for c in chars:
        if c.isdigit():
            num = num * 10 + int(c)
            continue
        if num != 0:
            # 数字统计结束，进行计算
            # 此时栈肯定非空，且栈顶元素为字母或}
            if stack[-1].isalpha():
                stack[-1] = stack[-1] * num
            elif stack[-1] == "}":
                # 弹出}
                stack.pop()
                while stack[-1] != "{":
                    temp = stack.pop() + temp
                # 弹出{，并保存解压后的字符串
                stack[-1] = temp * num
            num, temp = 0, ""
        stack.append(c)

    return "".join(stack[:-1])


if __name__ == "__main__":
    # {A3B1{C}3}3
    # string = input().strip()
    # print(solve_method(string))

    assert solve_method("{A3B1{C}3}3") == "AAABCCCAAABCCCAAABCCC"
    assert solve_method("A3") == "AAA"
    assert solve_method("{AD11B1{CF}3}3") == "ADDDDDDDDDDDBCFCFCFADDDDDDDDDDDBCFCFCFADDDDDDDDDDDBCFCFCF"
```

## 自解 2024-2-5
```python
"""
这一题的关键是将字符串压入栈后，在遇到数字时进行回溯，而不是遇到反花括号时
"""

instr = input()

stack = []
n = len(instr)
i = 0
while i < n:
    if instr[i].isdigit():
        num_str = instr[i]
        while i+1 < n and instr[i+1].isdigit():
            i += 1
            num_str += instr[i]
        num = int(num_str)
        temp = stack.pop()
        if temp.isalpha():
            stack.append(temp * num)
        elif temp == "}":
            str_temp = ""
            temp = stack.pop()
            while temp != "{":
                str_temp = temp + str_temp
                temp = stack.pop()
            stack.append(str_temp * num)
    else:
        stack.append(instr[i])
    i += 1
print("".join(stack))
```
