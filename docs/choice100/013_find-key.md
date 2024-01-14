# 013 寻找密码

## 题目描述

小王在进行游戏大闯关，有一个关卡需要输入一个密码才能通过，密码获得的条件如下：

在一个密码本中，每一页都有一个由26个小写字母组成的若干位密码，从它的末尾开始依次去掉一位得到的新密码也在密码本中存在。

请输出符合要求的密码，如果有多个符合要求的密码，则返回字典序最大的密码。如果没有符合要求的密码，则返回空字符串。

**备注：**
1 <= 密码本的页数 <= 105
1 <= 每页密码的长度 <= 105

## 输入描述

密码本由一个字符串数组组成，不同元素之间使用空格隔开，每一个元素代表密码本每一页的密码。

## 输出描述

一个符合要求的密码字符串。

## 示例描述

### 示例一

**输入：**
```text
h he hel hell hello
```

**输出：**
```text
hello
```

**说明：**  
"hello"从末尾依次去掉一位得到的"hell"、"hel"、"he"、"h"在密码本中都存在。

### 示例二

**输入：**
```text
b eredderd bw bww bwwl bwwln bwwlm
```

**输出：**
```text
bwwln
```

**说明：**  
"bwwlm"和"bwwln"从末尾依次去掉一位得到的密码在密码本中都存在，但是"bwwln"比"bwwlm"字典序排序大，所以应该返回"bwwln"。

## 解题思路

1. 将输入的字符串序列进行排序，保证字典序
2. 逆向遍历密码本，通过设置标志位`flag`，使用`while`循环判断从末尾开始依次去掉一位的密码是否在密码本中
3. 如果密码都存在，则返回该密码，如果不存在，则跳出`while`循环，继续逆序遍历下一个密码。

## 解题代码

```python
def solve_method(line):
    codebook = line.split()
    # 保证字典序
    codebook.sort()
    # 用于去重判断
    code_set = set(codebook)
    # 逆向遍历密码本
    for i in range(len(codebook) - 1, -1, -1):
        code = codebook[i]
        tmp_code = code[:-1]
        # 是否存在的标识
        flag = True
        # 循环判断从末尾开始依次去掉一位的密码是否在密码本中
        while len(tmp_code) > 0:
            if tmp_code in code_set:
                tmp_code = tmp_code[:-1]
            else:
                flag = False
                break
        if flag:
            return code

    return ""


if __name__ == '__main__':
    assert solve_method("h he hel hell hello") == "hello"
    assert solve_method("b eredderd bw bww bwwl bwwln bwwlm") == "bwwln"
```

## 自解
```python
# 解题思路：
# 将所有密码按长度进行排序，然后按长度从小到大，去找加一位的密码是否存在

# 处理输入
in_list = list(input().split())
# 将密码按长度进行分类存储
def find_secret(ls: list):
    dct = {}
    for secret in ls:
        if len(secret) not in dct:
            dct[len(secret)] = []
        dct[len(secret)].append(secret)

    if 1 not in dct or 2 not in dct:
        return ""
    sorted_dct = sorted(dct.items(), reverse=True)
    temp = sorted_dct.pop()
    num = temp[0]
    ans_list = temp[1]
    while sorted_dct:
        temp_list = []
        temp = sorted_dct.pop()
        if temp[0] != num+1:
            break
        for i in temp[1]:
            if i[:num] in ans_list:
                temp_list.append(i)
        if len(temp_list) == 0:
            break
        num += 1
        ans_list = temp_list
    ans_list.sort()
    return ans_list[-1]


print(find_secret(in_list))
```
