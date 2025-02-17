# 022 数组的中心位置

## 题目描述

给你一个整数数组`nums`，请计算数组的中心位置。数组中心位置是数组的一个下标，其左侧所有元素乘积等于右侧所有元素的乘积。数组第一个元素的左侧乘积为1，最后一个元素的右侧乘积为1.如果数组有多个中心位置，应该返回最靠近左边的那一个，如果数组不存在中心位置，返回-1。

## 输入描述

输入只有一行，给出`N`个正整数（用空格分隔）表示整数数组`nums`，例如
```text
2 5 3 6 5 6
```
其中：
- 1 <= nums.length <= 1024  
- 1 <= nums[i] <= 10

## 输出描述

输出数组的中心位置（元素下标）。

## 示例描述

### 示例一

**输入：**
```text
2 5 3 6 5 6
```

**输出：**
```text
3
```

**说明：**  
中心位置是3，其中：
- 左侧数的乘积：sum = nums[0] * nums[1] * nums[2] = 2 * 5 * 3 = 30
- 右侧数的乘积：sum = nums[4] * nums[5] = 5 * 6 = 30
可知二者相等，故中心位置为3。

## 解题思路

基本思路：先计算所有元素的乘积，再逐步剔除左侧元素，进行对比
1. 使用`reduce`方法初始化右侧乘积，计算数组所有元素乘积，用`r_prod`表示。
2. 遍历数组，
   - 将`r_prod`逐步剔除左侧元素
   - 与`l_prod`进行比较，如果不相等，将`l_prod`逐步乘以左侧元素
   - 如果相等，返回当前元素的下标
3. 如果都不相等，返回-1    

## 解题代码

```python
from functools import reduce


def solve_method(arr):
    l_prod = 1
    # 初始化右侧乘积，计算数组所有元素乘积
    r_prod = reduce(lambda x, y: x * y, arr)

    for i in range(len(arr)):
        # 逐步剔除左侧元素
        r_prod //= arr[i]
        if l_prod == r_prod:
            return i
        else:
            # 逐步乘以左侧元素
            l_prod *= arr[i]

    return -1


if __name__ == '__main__':
    assert solve_method([2, 5, 3, 6, 5, 6]) == 3
```
## 题解
```python
in_list = list(map(int, input().split()))


def solution(nums):
    n = len(in_list)
    # 双指针
    i = -1
    j = n
    left_sum = 1
    right_sum = 1
    while i < j:
        if left_sum < right_sum:
            i += 1
            left_sum *= in_list[i]
        elif left_sum > right_sum:
            j -= 1
            right_sum *= in_list[j]
        else:
            if i == j -1:
                return i
            if i == j - 2:
                return i+1
            i += 1
            j -= 1
            left_sum *= in_list[i]
            right_sum *= in_list[j]
    return -1


print(solution(in_list))
```
