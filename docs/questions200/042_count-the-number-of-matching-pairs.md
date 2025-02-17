# 042 统计匹配的二元组个数

## 题目描述

给定两个数组`A`和`B`，若数组`A`的某个元素`A[i]`与数组`B`中的某个元素`B[j]`满足`A[i] == B[j]`，则寻找到一个值匹配的二元组`(i, j)`。

请统计在这两个数组`A`和`B`中，一共存在多少个这样的二元组。

## 输入描述

第一行输入数组`A`的长度`M`。

第二行输入数组`B`的长度`N`。

第三行输入数组`A`的值。

第四行输入数组`B`的值。

取值范围是：
- 1 <= M,N <= 100000
- `A`、`B`数组中数值的取值均小于100000

## 输出描述

输出匹配的二元组个数。

## 说明

若不存在相等的值，则输出0。所采用的算法复杂度需小于`O(N^2)`，否则会超时。输入数组中允许出现重复数字，一个数字可以匹配多次。

## 示例描述

### 示例一

**输入：**
```text
5
4
1 2 3 4 5
4 3 2 1
```

**输出：**
```text
4
```

**说明：**

若下标从0开始，则匹配的二元组分别为(0, 3), (1, 2), (2, 1), (3, 0)共计4个。

### 示例二

**输入：**
```text
6
3
1 2 4 4 2 1
1 2 3
```

**输出：**
```text
4
```

**说明：**

若下标从0开始，则匹配的二元组分别为(0, 0), (1, 1), (4, 1), (5, 0)共计4个。

## 解题思路

**基本思路：**

题目要求时间复杂度小于`O(N^2)`，不能采用两个`for`循环的方式，可以考虑用空间换取时间。

**代码思路：**

1. 创建两个`Counter`类，存储数组A和数组B中元素出现的次数。
2. 遍历`A`的`Counter`类的元素：
    - 若元素在`A`和`B`中同时存在，则匹配的个数等于两处出现次数的乘积，将计算值累加到结果中。
3. 返回结果值。

## 解题代码
```python
from collections import Counter


def solve_method(A, B):
    dict_A = Counter(A)
    dict_B = Counter(B)

    count = 0
    for key in dict_A:
        if key in dict_B:
            count += dict_A[key] * dict_B[key]
    return count


if __name__ == "__main__":
    # 5
    # 4
    # 1 2 3 4 5
    # 4 3 2 1
    # M = int(input().strip())
    # N = int(input().strip())
    # A = list(map(int, input().strip().split()))
    # B = list(map(int, input().strip().split()))
    # print(solve_method(A, B))

    assert solve_method([1, 2, 3, 4, 5], [4, 3, 2, 1]) == 4
    assert solve_method([1, 2, 4, 4, 2, 1], [1, 2, 3]) == 4
```
## 自解 2024-2-6
```python
"""
哈希
"""
m = int(input())
n = int(input())
alist = list(input().split())
blist = list(input().split())

adct = {}
for i in alist:
    adct[i] = adct.get(i, 0) +1
ans = 0
for j in blist:
    if j in adct:
        ans += adct[j]
print(ans)
```
