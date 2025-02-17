# 005 云短信平台优惠活动

## 题目描述

某云短信厂商，为庆祝国庆，推出充值优惠活动。现在给出客户预算和优惠售价序列，求最多可获得的短信总条数。

## 输入描述

第一行客户预算`M`，其中0 <= M <= 1000000。

第二行给出售价表，`P1,P2,...,Pn`，其中1 <= n <= 100，`Pi`为充值`i`元获得的短信条数。取值范围是1 <= Pi <= 1000、1 <= n <= 100。

## 输出描述

最多获得的短信条数。

## 示例描述

### 示例一

**输入：**

```text
6
10 20 30 40 60
```

**输出：**

```text
70
```

**说明：**  
分两次充值最优，1元、5元各充一次。总条数为10+60=70。

### 示例二

**输入：**

```text
15
10 20 30 40 60 60 70 80 90 150
```

**输出：**

```text
210
```

**说明：**  
分两次充值最优，10元、5元各充一次。总条数150+60=210。

## 解题思路

**基本思路：** 使用回溯法解题。
1. 使用回溯法：
    - 确定参数：售价表`P`、客户剩余预算`n`、当权选择的充值方案`paths`、索引`index`。
    - 终止条件：当预算已耗光，计算当前总条数，并记录最大值短信条数。
    - 遍历售价表：
        - 将充值售价添加到充值方案中。
        - 处理：剩余预算减去当权充值售价，继续遍历。
        - 回溯：将充值售价移出充值方案。
2. 返回最大值短信条数。

## 解题代码

```python
max_count = 0


def solve_method(M, P):
    backstracking(P, M, [], 0)
    return max_count


def backstracking(P, n, paths, index):
    global max_count
    # 预算已耗光，计算当前总条数，并记录最大值
    if n == 0:
        count = sum(paths)
        max_count = max(max_count, count)
        return

    for i in range(index, len(P)):
        x = int(P[i])
        paths.append(x)
        # 继续遍历所有可能性
        backstracking(P, n - (i + 1), paths, i + 1)
        paths.pop()


if __name__ == '__main__':
    nums = [10, 20, 30, 40, 60]
    assert solve_method(6, nums) == 70

    nums = [10, 20, 30, 40, 60, 60, 70, 80, 90, 150]
    assert solve_method(15, nums) == 210
```
## 自解
```python
money = int(input())
message = [0]
message.extend(list(map(int, input().split())))


def func(mess, m):
    ans = []
    back(mess, m, ans, [], 0)
    return max(ans)


def back(mess, m, ans, path, total):
    if m == 0:
        ans.append(total)
        return
    for i in range(1, len(mess)):
        if i > m:
            break
        m -= i
        total += mess[i]
        path.append((i, mess[i]))
        back(mess, m, ans, path, total)
        temp = path.pop()
        m += temp[0]
        total -= temp[1]


print(func(message, money))

```
