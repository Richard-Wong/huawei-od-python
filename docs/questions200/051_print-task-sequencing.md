# 051 打印任务排序

## 题目描述

某个打印机根据打印队列执行打印任务。打印任务分为9个优先级，分别用数字1-9表示，数字越大优先级越高。打印机每次从队列头部取出第一个任务`A`，然后检查队列余下任务中有没有比`A`优先级更高的任务，如果有比`A`优先级高的任务，则将任务`A`放到队列尾部，否则就执行任务`A`的打印。

请编写一个程序，根据输入的打印队列，输出实际的打印顺序。

## 输入描述

输入一行，为每个任务的优先级，优先级之间用逗号隔开，优先级取值范围是1-9。

## 输出描述

输出一行，为每个任务的打印顺序，打印顺序从0开始，用逗号隔开。

## 示例描述

### 示例一

**输入：**
```text
9,3,5
```

**输出：**
```text
0,2,1
```

**说明：**  

队列头部任务的优先级为9，最先打印，故序号为0。接着队列头部任务优先级为3，队列中还有优先级为5的任务，优先级3任务被移到队列尾部；接着打印优先级为5的任务，故其序号为1；最后优先级为3的任务的序号为2。

### 示例二

**输入：**
```text
1,2,2
```

**输出：**
```text
2,0,1
```

**说明：** 

队列头部任务的优先级为1，被移到队列尾部；接着顺序打印两个优先级为2的任务，故其序号分别为0和1；最后打印剩下的优先级为1的任务，其序号为2。

## 解题思路

1. 构建优先级序号列表，每个元素有两个值，分别是任务优先级、任务序号。
2. 判定是否当前优先级最高：可以提前将所有任务升序，用`max_idx`标记当前优先级最高的任务   
3. 依次遍历各个打印任务：
    - 如果该任务是当前优先级最高的，存入结果列表。
    - 如果不是当前优先级最高的，加入队尾。
4. 返回结果列表。

## 解题代码
```python
def solve_method(tasks):
    task_index = [(task, idx) for idx, task in enumerate(tasks)]
    n = len(tasks)

    # 标记当前优先级最高的任务
    tasks.sort()
    max_idx = n - 1
    # 标记打印顺序
    res = [0] * n
    printIdx = 0
    while task_index:
        task, idx = task_index.pop(0)
        # 是优先级最高的任务
        if task == tasks[max_idx]:
            res[idx] = printIdx
            max_idx -= 1
            printIdx += 1
        # 不是优先级最高的任务，加入队尾
        else:
            task_index.append((task, idx))
    return res


if __name__ == '__main__':
    assert solve_method([9, 3, 5]) == [0, 2, 1]
    assert solve_method([1, 2, 2]) == [2, 0, 1]
```
## 自解 
```python
# 使用排序方法错误，例如 454输出为201， 而434输出则为021，需要按照题目，模拟输出的过程
in_list = list(map(int, input().split(",")))
enumerate_list = []
for i in enumerate(in_list):
    enumerate_list.append(i)
enumerate_list.sort(key=lambda x: x[1],reverse=True)
n = len(enumerate_list)
ans_list = [0] * n
for i in range(n):
    ans_list[enumerate_list[i][0]] = i
print(",".join(map(str, ans_list)))

```

## 自解二 2024-2-17
```python
from collections import deque

alist = list(map(int, input().split(",")))

numbers = [0] * 9
for i in alist:
    numbers[i-1] += 1
print(numbers)


index_list = [(i, j) for i, j in enumerate(alist)]
dq = deque(index_list)

ans_list = [-1] * len(alist)

count = 0
while dq:
    temp = dq.popleft()
    if sum(numbers[temp[1]:]) > 0:
        dq.append(temp)
    else:
        ans_list[temp[0]] = count
        count += 1
        numbers[temp[1]-1] -= 1
print(",".join(map(str, ans_list)))
```
