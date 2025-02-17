# 014 寻找链表的中间结点

## 题目描述

给定一个单链表`L`，请编写程序输出`L`中间结点保存的数据。如果有两个中间结点，则输出第二个中间结点保存的数据。

例如：给定`L`为1->7->5，则输出应该为7；给定`L`为1->2->3->4，则输出应该为3。

**备注：**  
已确保输入的结点所构成的链表`L`不会成环，但会存在部分输入结点不属于链表`L`的情况。

## 输入描述

每个输入包含1个测试用例。每个测试用例的第1行给出链表首结点地址、结点总个数正整数`N`(N <= 10^5)。结点的地址是5位非负整数，`NULL`地址用-1表示。

接下来有`N`行，每行格式为：
```text
Address Data Next
```
其中`Address`是结点地址，`Data`是该结点保存的整数数据（0 <= Data <= 10^8），`Next`是下一个结点的地址。

## 输出描述

对每个测试用例，在一行中输出`L`中间结点保存的数据。如果有两个中间结点，则输出第二个中间结点保存的数据。

## 示例描述

### 示例一

**输入：**
```text
00100 4
00000 4 -1
00100 1 12309
33218 3 00000
12309 2 33218
```

**输出：**
```text
3
```

### 示例二

**输入：**
```text
10000 3
76892 7 12309
12309 5 -1
10000 1 76892
```

**输出：**
```text
7
```

## 解题思路

1. 初始化数据列表`data`。
2. 遍历链表：获取下一个结点，将下一个结点的数据加入到列表`data`中
3. 获取中间结点的数据

## 解题代码

```python
def solve_method(head, link):
    if len(link) == 0:
        return -1

    data = []
    next_node = head
    # 遍历链表
    while next_node in link:
        # 获取下一个结点
        node = link[next_node]
        # 将下一个结点的数据加入到列表中
        data.append(node[0])
        next_node = node[1]

    # 获取中间结点的数据
    return data[len(data) // 2]


if __name__ == '__main__':
    head = "00100"
    n = 4
    link = {"00000": [4, -1],
            "00100": [1, "12309"],
            "33218": [3, "00000"],
            "12309": [2, "33218"]}
    assert solve_method(head, link) == 3

    head = "10000"
    n = 3
    link = {"76892": [7, "12309"],
            "12309": [5, -1],
            "10000": [1, "76892"]}
    assert solve_method(head, link)
```

## 自解
```python
# 解题断路
# 用字典存储数据，减小查找时间复杂度


head_address, num = map(int, input().split())

# 用字典存，减小查找时间
dct_in = {}
for i in range(num):
    addr, data, nex = map(int, input().split())
    dct_in[addr] = (data, nex)


def find_mid_node(dct):
    val = dct[head_address][0]
    follow = dct[head_address][1]

    ans_list = [val]
    while follow != -1:
        ans_list.append(dct[follow][0])
        follow = dct[follow][1]
    n = len(ans_list)
    return ans_list[n//2] if n%2 == 0 else ans_list[(n-1)//2]


print(find_mid_node(dct_in))
```
