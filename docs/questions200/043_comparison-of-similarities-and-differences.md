# 043 统计差异值大于相似值二元组个数

## 题目描述

对于任意两个正整数`A`和`B`，定义它们之间的差异值和相似值：

- 差异值：`A`、`B`转换成二进制后，对于二进制的每一位，对应位置的`bit`值不相同则为1，否则为0。
- 相似值：`A`、`B`转换成二进制后，对于二进制的每一位，对应位置的`bit`值都为1则为1，否则为0。

现在有`n`个正整数`A[0]`到`A[n-1]`，问有多少对`(i, j)`(0 <= i < j < n)，`A[i]`和`A[j]`的差异值大于相似值。

> 假设A=5，B=3；  
则A的二进制表示101；B的二进制表示011；  
则A与B的差异值二进制为110；相似值二进制为001；  
A与B的差异值十进制等于6，相似值十进制等于1，满足条件。

## 输入描述

第一行输入一个`n`。

第二行输入`n`个正整数。

数据范围：1 <= n <= 10^5、1 <= A[i] < 2^30。

## 输出描述

满足差异值大于相似值的对数

## 示例描述

### 示例一

**输入：**
```text
4
4 3 5 2
```

**输出：**
```text
4
```

**说明：**

满足条件的分别是`(0,1)`、`(0,3)`、`(1,2)`、`(2,3)`，共4对。

### 示例二

**输入：**
```text
5
3 5 2 8 4
```

**输出：**
```text
8
```

## 解题思路

**基本思路：**

- 差异值等于两数**按位异或**运算的结果。
- 相似值等于两数**按位与**运算的结果。

**代码思路：**

1. 初始化统计值`count`。 
2. 依次遍历数组：
   - 计算两数按位异或，即差异值。
   - 计算两数按位与，即相似值。
   - 若差异值大于相似值，则`count`累加1。
3. 返回统计结果。    

## 解题代码
```python
def solve_method(n, nums):
    count = 0
    for i in range(n):
        for j in range(i + 1, n):
            diff = nums[i] ^ nums[j]
            simi = nums[i] & nums[j]
            if diff > simi:
                count += 1
    return count


if __name__ == "__main__":
    # 4
    # 4 3 5 2
    # n = int(input().strip())
    # nums = list(map(int, input().strip().split()))
    # print(solve_method(n, nums))

    assert solve_method(4, [4, 3, 5, 2]) == 4
    assert solve_method(5, [3, 5, 2, 8, 4]) == 8
```
## 自解 2024-2-6
```python
"""
求位与与异或，会导致超时
问题可以直接简化为求两个数转化为二进制后，长度是否相等，长度相等时，最高位的1在同一个位置，相似大于差异；长度不同时，最高位分别是1和0，差异大于相似
"""
n = int(input())
nums = list(map(int, input().split()))

dct = {}
for num in nums:
    length = len(bin(num))
    if length not in dct:
        dct[length] = 0
    dct[length] += 1

ans = 0
alist = [i for i in dct.values()]
for i in range(len(alist)-1):
    for j in range(i+1, len(alist)):
        ans += alist[i] * alist[j]
print(ans)

```
