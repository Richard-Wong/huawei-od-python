# 263 计算网络信号

## 题目描述

网络信号经过传递会逐层衰减，且遇到阻隔物无法直接穿透，在此情况下需要计算某个位置的网络信号值。

> 注意：网络信号可以绕过阻隔物

- `array[m][n]`的二维数组代表网格地图。
- `array[i][j]=0`代表i行j列是空旷位置。
- `array[i][j]=x`（x为正整数）代表`i`行`j`列是信号源，信号强度是`x`。
- `array[i][j]=-1`代表`i`行`j`列是阻隔物。
- 信号源只有1个，阻隔物可能有0个或多个。
- 网络信号衰减是上下左右相邻的网格衰减1。
- 现要求输出对应位置的网络信号值。

## 输入描述

输入为三行，第一行是`m`和`n`，代表输入是一个`m*n`的数组。

第二行是一串`m*n`个用空格分隔的整数。每连续`n`个数代表一行，再往后`n`个代表下一行，以此类推。对应的值代表对应的网格是空的位置，还是信号源，还是阻隔物。

第三行是`i`和`j`，代表需要计算`array[i][j]`的网络信号值。

注意：此处`i`和`j`均从0开始，即第一行`i`为0。

例如
```
6 5
0 0 0 -1 0 0 0 0 0 0 0 0 -1 4 0 0 0 0 0 0 0 0 0 0 -1 0 0 0 0 0
1 4
```

代表如下地图
![045-001-sample-analysis1](images/045-001-sample-analysis1.png)

需要输出第1行第4列的网络信号值，如下图，值为2
![045-002-sample-analysis2](images/045-002-sample-analysis2.png)

## 输出描述

输出对应位置的网络信号值，如果网络信号未覆盖到，也输出0。一个网格如果可以途径不同的传播衰减路径传达，取较大的值作为其信号值。

### 备注

- `m`不一定等于`n`，m < 100，n < 100，网络信号之小于1000。
- 信号源只有1个，阻隔物可能有0个或多个。
- 输入的`m`、`n`与第二行的数组是合法的，无需处理数量对不上的异常情况。
- 要求输出信号值的位置，不会是阻隔物。

## 示例描述

### 示例一

**输入：**
```text
6 5
0 0 0 -1 0 0 0 0 0 0 0 0 -1 4 0 0 0 0 0 0 0 0 0 0 -1 0 0 0 0 0
1 4
```

**输出：**
```text
2
```

### 示例二

**输入：**
```text
6 5
0 0 0 -1 0 0 0 0 0 0 0 0 -1 4 0 0 0 0 0 0 0 0 0 0 -1 0 0 0 0 0
2 1
```

**输出：**
```text
0
```

## 解题思路

**基本思路：** 使用深度优先搜索DFS求解。

1. 得到信号源`source`。
2. 构造`Block`存储网格的信息，包括坐标`i`、`j`、信号值`value`。
3. 从信号源开始深度优先搜索，遍历所有信号：
  - 遍历信号源上下左右四个相邻位置，并更新信号值。
  - 若相邻位置仍可扩散，则继续递归遍历。
4. 返回对应坐标的信号值。

## 解题代码
```python
class Block:
    def __init__(self, i, j, value):
        self.i = i
        self.j = j
        self.value = value


def solve_method(nums, dest):
    source = [0, 0]
    for i in range(len(nums)):
        for j in range(len(nums[0])):
            if nums[i][j] > 0:
                source = [i, j]

    blocks = [Block(source[0], source[1], nums[source[0]][source[1]])]
    visited = [[0] * len(nums[0]) for _ in range(len(nums))]
    while len(blocks) > 0:
        block = blocks.pop(0)
        diffuse(nums, blocks, visited, block.i, block.j, block.value)
    return nums[dest[0]][dest[1]]


def diffuse(nums, blocks, visited, i, j, value):
    for x, y in [[1, 0], [-1, 0], [0, 1], [0, -1]]:
        if 0 <= i + x < len(nums) and 0 <= j + y < len(nums[0]) and visited[i + x][j + y] == 0:
            visited[i + x][j + y] = 1
            if nums[i + x][j + y] == 0:
                nums[i + x][j + y] = value - 1
            if nums[i + x][j + y] > 1:
                blocks.append(Block(i + x, j + y, nums[i + x][j + y]))


if __name__ == "__main__":
    # 6 5
    # 0 0 0 -1 0 0 0 0 0 0 0 0 -1 4 0 0 0 0 0 0 0 0 0 0 -1 0 0 0 0 0
    # 1 4
    # m, n = map(int, input().strip().split())
    # array = list(map(int, input().strip().split()))
    # nums = [array[i:i + n] for i in range(0, len(array), n)]

    # 目的单元格
    # dest = list(map(int, input().strip().split()))
    # print(solve_method(nums, dest))

    grid = [[0, 0, 0, -1, 0],
            [0, 0, 0, 0, 0],
            [0, 0, -1, 4, 0],
            [0, 0, 0, 0, 0],
            [0, 0, 0, 0, -1],
            [0, 0, 0, 0, 0]]

    assert solve_method(grid, [1, 4]) == 2

    grid = [[0, 0, 0, -1, 0],
            [0, 0, 0, 0, 0],
            [0, 0, -1, 4, 0],
            [0, 0, 0, 0, 0],
            [0, 0, 0, 0, -1],
            [0, 0, 0, 0, 0]]
    assert solve_method(grid, [2, 1]) == 0
```

## 自解 2024年2月5日
```python
row, col = map(int, input().split())
alist = list(map(int, input().split()))
matrix = []
for i in range(row):
    temp = alist[5*i: 5*i+col]
    matrix.append(temp[:])
# print(matrix)
visited = [[False]*col for _ in range(row)]
for i in range(row):
    for j in range(col):
        if matrix[i][j] > 0:
            start = [i, j]
        elif matrix[i][j] == -1:
            visited[i][j] =True

target_x, target_y = map(int, input().split())


from collections import deque
dq = deque()
dq.append(start)


def bfs(ls, signal):
        # 坐标在边界内，且信号强度大于0
        for x, y in [(1, 0), (0, -1), (-1, 0), (0, 1)]:
            if 0 <= signal[0]+x < row and 0 <= signal[1]+y < col and not visited[signal[0]+x][signal[1]+y]:
                dq.append([signal[0]+x, signal[1]+y])
                matrix[signal[0]+x][signal[1]+y] = matrix[signal[0]][signal[1]] -1


while dq:
    pos = dq.popleft()
    visited[pos[0]][pos[1]] = True
    bfs(matrix, pos)
    if visited[target_x][target_y]:
        print(matrix[target_x][target_y])
        for i in matrix:
            print(i)
        break

```
## 自解二 2024年2月5日
```python
row, col = map(int, input().split())
alist = list(map(int, input().split()))
grid = []
for i in range(row):
    temp = alist[i*col:(i+1)*col]
    grid.append(temp)
# print(grid)
tarx, tary = map(int, input().split())  # 目标值的坐标

source = (0, 0)
source_flag = False
for i in range(row):
    for j in range(col):
        if grid[i][j] > 0:
            source = (i, j)  # 信号源的坐标
            source_flag = True
            break
    if source_flag:
        break

visited = [[0]*col for _ in range(row)]  # 存储访问过的位置

from collections import deque

queue = deque()
queue.append(source)
visited[source[0]][source[1]] = 1  # 放入队列时，立即将该位置1

while queue:  # 队列不为空时
    curx, cury = queue.popleft()  # 弹出当前要处理的位置
    val = grid[curx][cury]
    if val <= 0:
        continue
    for dx, dy in [(1, 0), (0, 1), (-1, 0), (0, -1)]:
        if 0 <= curx+dx < row and 0 <= cury+dy < col and not visited[curx+dx][cury+dy] and grid[curx+dx][cury+dy] != -1:
            queue.append((curx+dx, cury+dy))
            visited[curx+dx][cury+dy] = 1
            grid[curx+dx][cury+dy] = val -1
    if visited[tarx][tary]:
        print(grid[tarx][tary])
        break
if not visited[tarx][tary]:
    print(0)




```
