# 015 开心消消乐

## 题目描述

给定一个`N`行`M`列的二维矩阵，矩阵中每个位置的数字取值为0或1，矩阵示例如下：
```text
1 1 0 0
0 0 0 1
0 0 1 1
1 1 1 1
```

现需要将矩阵中所有的1进行反转为0，规则如下：
1. 当点击一个1时，该1被反转为0，同时相邻的上、下、左、右，以及左上、左下、右上、右下8个方向的1（如果存在1）均会自动反转为0；
2. 进一步，一个位置上的1被反转为0时，与其相邻的8个方向的1（如果存在1）均会自动反转为0

按照上述规则，示例中的矩阵只最少需要点击2次后，所有值均为0，请问，给定一个矩阵，最少需要点击几次后，所有数字均为0？

## 输入描述

第一行输入两个整数，分别表示矩阵的行数`N`和列数`M`，取值范围均为[1,100]

接下来`N`行表示矩阵的初始值，每行均为`M`个数，取值范围[0,1]

## 输出描述

输出一个整数，表示最少需要点击的次数。

## 示例描述

### 示例一

**输入：**
```text
3 3
1 0 1
0 1 0
1 0 1
```

**输出：**
```text
1
```

**说明：**  
上述样例中，四个角上的1均在中间的1的相邻8个方向上，因此只需要点击1次即可。

### 示例二

**输入：**
```text
4 4
1 1 0 0
0 0 0 1
0 0 1 1
1 1 1 1
```

**输出：**
```text
2
```

**说明：**  
上述样例中，只需要点击2次，可将所有的1进行消除。

## 解题思路

1. 本题使用深度优先遍历（DFS）算法
2. 遍历矩阵，使用`path`列表存放待处理的位置，使用深度优先遍历算法，次数加1
3. 深度优先遍历：
    1. 确认递归函数的参数：参数包括矩阵的行数`n`和列数`m`、矩阵`arr`、待处理列表`path`。
    2. 终止条件：列表中的位置都处理完毕，列表长度为0。
    3. 处理目前搜索节点出发的路径：将8个方位的1进行反转，如果8个方向上的位置有1存在，则将该位置存入待处理列表`path`中。
    4. 递归遍历
4. 返回次数

## 解题代码

```python
def solve_method(n, m, arr):
    count = 0
    for x in range(n):
        for y in range(m):
            node_value = arr[x][y]
            if node_value == 1:
                # 如果是1，则进行反转
                count += 1
                # 存放待处理的位置
                path = [[x, y]]
                # 使用深度优先遍历
                dfs(n, m, arr, path)
    return count

def dfs(n, m, arr, path):
    # 直到列表中的位置都处理完毕，结束
    if len(path) == 0:
        return

    # 开始处理这个位置，从待处理列表中删除这个位置
    node = path.pop(0)
    # 8个方位的相对位置
    directions = [(-1, -1), (0, -1), (1, -1), (-1, 0), (1, 0), (-1, 1), (0, 1), (1, 1)]
    # 将8个方位的1进行反转
    for d in directions:
        new_x = node[0] + d[0]
        new_y = node[1] + d[1]
        if 0 <= new_x < n and 0 <= new_y < m and arr[new_x][new_y] == 1:
            # 反转为0
            arr[new_x][new_y] = 0
            # 将新位置存放到待处理列表中
            path.append([new_x, new_y])
    # 继续处理列表中的位置，连锁反应
    dfs(n, m, arr, path)


if __name__ == '__main__':
    arr = [[1, 0, 1],
           [0, 1, 0],
           [1, 0, 1]]
    assert solve_method(3, 3, arr) == 1

    arr = [[1, 1, 0, 0],
           [0, 0, 0, 1],
           [0, 0, 1, 1],
           [1, 1, 1, 1]]
    assert solve_method(4, 4, arr) == 2
```
## 自解
```python
def solution(g):
    ans = 0
    n = len(g)
    m = len(g[0])
    for j in range(m):
        for i in range(n):
            if g[i][j] == 1:
                ans += 1
                dfs(g, i, j, n, m)
    return ans


def dfs(g, i, j, n, m):
    if i < 0 or j < 0 or i >= n or j >= m or g[i][j] == 0:
        return
    g[i][j] = 0
    dfs(g, i-1, j-1, n, m)
    dfs(g, i, j-1, n, m)
    dfs(g, i+1, j-1, n, m)
    dfs(g, i-1, j, n, m)
    dfs(g, i+1, j, n, m)
    dfs(g, i-1, j+1, n, m)
    dfs(g, i, j+1, n, m)
    dfs(g, i+1, j+1, n, m)


n, m = map(int, input().split())
graph = []
for _ in range(n):
    graph.append(list(map(int, input().split())))

print(solution(graph))
```
