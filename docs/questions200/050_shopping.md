# 050 购物

## 题目描述

商店里有`N`件唯一性商品，每件商品有一个价格，第`i`件商品的价格是`a[i]`。一个购买方案可以是从`N`件商品种选择任意件进行购买（至少一件），花费即价格之和。

现在你需要求出所有购买方案中花费前`K`小的方案，输出这些方案的花费。

当两个方案选择的商品集合至少有一件不同，视为不同方案，因此可能存在两个方案花费相同。

## 输入描述

第一行包含两个整数`N,K`，整数之间通过空格隔开，分别表示商品的个数、需要求得的花费个数。取值范围是1 < N < 10000、1 <= K <= min(2^N-1, 100000)

第二行包含`N`个整数`a1,a2,...,an`，整数之间通过空格隔开。表示`N`件商品的价格，其中1 <= a1 <= a2 <= ... <= an <= 10000。

## 输出描述

按花费从小到大的顺序依次输出`K`行，一行一个整数。表示花费前`K`小的购买方案的花费。

## 示例描述

### 示例一

**输入：**
```text
5 6
1 1 2 3 3
```

**输出：**
```text
1
1
2
2
3
3
```

**说明：**  

花费前10小的方案：
```text
集合   花费
 1     1
 2     1
 3     2
1,2    2
 4     3
 5     3
1,3    3
2,3    3
1,2,3  4
1,4    4 
```

### 示例二

**输入：**
```text
4 15
1 2 3 4
```

**输出：**
```text
1
2
3
3
4
4
5
5
6
6
7
7
8
9
10
```

### 示例三

**输入：**
```text
3 7
1 10 100
```

**输出：**
```text
1
10
11
100
101
110
111
```

## 解题思路

**基本思路：**

如果暴力地求出全组合，然后按组合之和排序，求出前`K`小的组合，肯定会超时。

下面以示例二`[1, 2, 3, 4]`为例，逐个遍历：
1. 第一小的组合肯定是`[1]`：可以看成基于空组合`[]`叠加了最小元素1产生。
2. 第二小的组合有两个来源：
    - `[1]`组合加下一个最小元素2，形成`[1,2]`。
    - `[]`组合加下一个最小元素2，形成`[2]`。
    - 可得到第二小组合为`[2]`。

3. 此时我们有了三个最小组合`[]`,`[1]`,`[2]`，为每个组合标出下一个合入的元素，可得到：
    - 当前组合为`[]`，将合入元素3。
    - 当前组合为`[1]`，将合入元素2。
    - 当前组合为`[2]`，将合入元素3。
    - 根据当前组合，将被合入的元素，可以产生新的组合。

可以使用优先队列来找出每轮最小的“将要产生的新组合”。

**解题思路：**

1. 定义元素类`Item`：当前组合之和`cur_sum`，将要被加入当前组合的新元素索引`next_idx`，"将要产生的新组合"之和`next_sum = curSum + nums[next_idx]`，而对于一个组合，`next_sum`越小，则排序靠前。
2. 将数组按照从小到大排序。
3. 初始化队列`pq`（用于存储元素），包含一个元素：空组合当前的和为0、将要加入的下一个元素为`nums[0]`（即索引为0）、将要产生的新组合的和为`0+nums[0]`。
4. 遍历数组中的所有元素：
    - 取出优先队列`pq`中最小组合（注意这里的最小，指的是"将要产生的新组合"最小），并存入结果列表中。
    - 如果当前组合还有可合入的下一个元素，即`cur_item.next_idx + 1 < n`, 则说明可以基于当前组合产生一个新组合。
        - 基于当前组合模型产生的新组合，也是本轮最小的组合。
        - 当前组合需要更新`next_idx`后，重新加入优先队列。
5. 返回结果列表。

## 解题代码
```python
import heapq


class Item:
    def __init__(self, cur_sum, next_idx, next_sum):
        self.cur_sum = cur_sum
        self.next_idx = next_idx
        self.next_sum = next_sum

    def __lt__(self, other):
        # 按照总和排序
        return self.next_sum < other.next_sum


def solve_method(n, k, nums):
    nums.sort()

    # 空组合当前的和为0，将要加入的下一个元素为nums[0]（即索引为0），将要产生的新组合的和为0+nums[0]
    pq = [Item(0, 0, nums[0])]
    res = []
    while k > 0 and pq:
        # 取出优先队列中最小组合（注意这里的最小，指的是"将要产生的新组合"最小）
        cur_item = heapq.heappop(pq)
        res.append(cur_item.next_sum)
        k -= 1
        # 如果当前组合还有可合入的下一个元素，则说明可以基于当前组合产生一个新组合
        if cur_item.next_idx + 1 < n:
            # 基于当前组合产生的新组合，也是本轮最小的组合
            heapq.heappush(pq, Item(cur_item.next_sum, cur_item.next_idx + 1,
                                    cur_item.next_sum + nums[cur_item.next_idx + 1]))
            # 当前组合需要更新next_idx后，重新加入优先队列
            cur_item.next_idx += 1
            cur_item.next_sum = cur_item.cur_sum + nums[cur_item.next_idx]
            heapq.heappush(pq, cur_item)
    return res


if __name__ == '__main__':
    # n, k = map(int, input().split())
    # nums = list(map(int, input().split()))
    # print(solve_method(n, k, nums))
    # print(solve_method(4, 15, [1, 2, 3, 4]))

    assert solve_method(5, 6, [1, 1, 2, 3, 3]) == [1, 1, 2, 2, 3, 3]
    assert solve_method(4, 15, [1, 2, 3, 4]) == [1, 2, 3, 3, 4, 4, 5, 5, 6, 6, 7, 7, 8, 9, 10]
    assert solve_method(3, 7, [1, 10, 100]) == [1, 10, 11, 100, 101, 110, 111]
```

## 自解一 2024-1-30
```python
'''
用回溯套循环的方法暴力组合，考试中的话肯定会超时
'''
n, k = map(int, input().split())
value = list(map(int, input().split()))


def solve(v, depth):
    ans_1 = []
    backtracing(0, v, depth, [], 0, ans_1)
    return ans_1


def backtracing(start, value, depth, path, total, ans_1):
    if len(path) == depth:
        ans_1.append(sum(path))
        return
    for j in range(start, n):
        path.append(value[j])
        total += value[j]
        backtracing(j + 1, value, depth, path, total, ans_1)
        total -= path.pop()


ans = []
for i in range(1, n+1):
    ans.extend(solve(value, i))
ans.sort()
for i in range(k):
    print(ans[i])
```

## 自解二
```python
n, k = map(int, input().split())
ls = list(map(int, input().split()))


class Group:
    def __init__(self, cur_sum, nex_idx, nex_sum):
        self.cur_sum = cur_sum
        self.nex_idx = nex_idx
        self.nex_sum = nex_sum

    def __lt__(self, other):
        return self.nex_sum < other.nex_sum


import heapq
heap = []
heapq.heappush(heap, Group(0, 0, ls[0]))
while k > 0:
    item = heapq.heappop(heap)
    print(item.nex_sum)
    k -= 1
    if item.nex_idx + 1 < n:
        heapq.heappush(heap, Group(item.nex_sum, item.nex_idx + 1, item.nex_sum + ls[item.nex_idx + 1]))
        heapq.heappush(heap, Group(item.cur_sum, item.nex_idx + 1, item.cur_sum + ls[item.nex_idx + 1]))

````
