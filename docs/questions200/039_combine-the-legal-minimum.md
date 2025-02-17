# 039 组合出合法最小数

## 题目描述

给一个数组，数组里面都是代表非负整数的字符串，将数组里所有的数值排列组合拼接起来组成一个数字，输出拼接成的最小的数字。

## 输入描述

一个数组，数组不为空，数组里面都是代表非负整数的字符串，可以是0开头，例如：`["13","045","09","56"]`。

- 数组的大小范围：[1,50]。
- 数组中每个元素的长度范围：[1,30]。

## 输出描述

以字符串的格式输出一个数字，需要满足下列要求：
- 如果最终结果是多位数字，要优先选择输出不是0开头的最小数字。
- 如果拼接出的数字都是0开头，则选取值最小的，并且把开头部分的0都去掉再输出。
- 如果是单位数0，可以直接输出0。

## 示例描述

### 示例一

**输入：**
```text
20 1
```

**输出：**
```text
120
```

**说明：**

`["20","1"]`能组成201和120，其中120比较小。

### 示例二

**输入：**
```text
08 10 2
```

**输出：**
```text
10082
```

### 示例三

**输入：**
```text
01 02
```

**输出：**
```text
102
```

**说明：**

`["01","02"]`能拼接成`0102`和`0201`，都是0开头，选取较小的`0102`，去掉前面的0，输出`102`。

## 解题思路

**基本思路：**

关键点1：找到一个排序规则，使数字按此规则排序后，按顺序连接后得到的数字最小
> 可以发现，将首数字取值小的数字排在前面，可使得连接后的数字最小（即比较数字表示的字符串大小）

关键点2：对于"0"开头的数字特殊处理
> 可以将"0"开头的数字和非"0"开头的数字存在两个数组中，单独排序；最后合并

**代码思路：**
1. 将"0"开头的数字存入数组`nums_zero`中。
2. 将非"0"开头的数字存入数组`nums_non_zero`中。
3. 如果不存在0开头的数字，直接拼接非0开头的数字，返回结果。
4. 如果存在0开头的数字：
    - 如果也存在非0开头的数字，优先选取一个非0开头的数字，将所有0开头的数字拼接，再把剩余非0开头的数字拼接在一起，返回结果。
    - 如果不存在非0开头的数字，拼接所有非0开头的数字，然后把开头部分的"0"都去掉，返回结果。

## 解题代码
```python
def solve_method(nums):
    nums_zero, nums_non_zero = [], []
    for num in nums:
        if num.startswith('0'):
            nums_zero.append(num)
        else:
            nums_non_zero.append(num)

    nums_zero.sort()
    nums_non_zero.sort()

    # 如果不存在0开头的数字
    if len(nums_zero) == 0:
        return "".join(nums_non_zero)
    # 如果存在0开头的数字，也存在非0开头的数字
    if len(nums_non_zero) > 0:
        return "".join([nums_non_zero[0]] + nums_zero + nums_non_zero[1:])
    # 如果只存在0开头的数字
    return "".join(nums_zero).lstrip('0')


if __name__ == "__main__":
    # 20 1
    # nums = list(map(str, input().strip().split()))
    # print(solve_method(nums))

    assert solve_method(["20", "1"]) == "120"
    assert solve_method(["08", "10", "2"]) == "10082"
    assert solve_method(["01", "02"]) == "102"
```

## 自解 2024-2-18
```python
# 输入0，还有0 0 的特殊情况还未处理
nums = list(input().split())
nums_zero, nums_no_zero = [], []
for i in nums:
    if i[0] == "0":
        nums_zero.append(i)
    else:
        nums_no_zero.append(i)

# 排序
import functools
compare = lambda x,y:1 if y+x < x+y else -1
nums_zero.sort(key=functools.cmp_to_key(compare))
nums_no_zero.sort(key=functools.cmp_to_key(compare))

if len(nums_zero) == 0:
    print("".join(nums_no_zero))
elif len(nums_no_zero) == 0:
    print("".join(nums_zero).lstrip("0"))
else:
    print(nums_no_zero[0] + "".join(nums_zero) + "".join(nums_no_zero[1:]))
```
