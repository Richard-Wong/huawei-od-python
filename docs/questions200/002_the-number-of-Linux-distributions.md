# 002 Linux发行版的数量

## 题目描述

Linux 操作系统有多个发行版，[distrowatch.com](http://distrowatch.com/) 提供了各个发行版的资料。这些发行版互相存在关联，例如Ubuntu基于Debian开发，而Mint又基于Ubuntu开发，那么我们认为Mint同Debian也存在关联。

发行版集是一个或多个相关存在关联的操作系统发行版，集合内不包含没有关联的发行版。

给你一个`n*n`的矩阵`isConnected`，其中`isConnected[i][j] = 1`表示第`i`个发行版和第`j`个发行版直接关联，而`isConnected[i][j]=0`表示二者不直接相连。

返回最大的发行版集中发行版的数量。

## 输入描述

第一行输入发行版的总数量`N`，取值范围是1 <= N <= 200，之后每行表示各发行版间是否直接相关。

## 输出描述

输出最大的发行版集中发行版的数量。

## 示例描述

**输入**

```text
4
1 1 0 0
1 1 1 0
0 1 1 0
0 0 0 1
```

**输出**

```text
3
```

**说明**

Debian(1)和Unbuntu(2)相关，Mint(3)和Ubuntu(2)相关，EeulerOS(4)和另外三个都不相关，所以存在两个发行版集，发行版集中发行版的数量分别是3和1，所以输出3。

## 解题思路

**一些知识点：**

- 深度优先搜索（DFS），广度优先搜索（BFS）
- 最大连通分量

**简单提示：**

1. 输入 = 方阵行列数 + 方阵 = 发行版数 + 它们之间的关联性 = 图的邻接矩阵
   ![输入分析](images/002-001-sample-analysis.png)
2. 发行版集 = 发行版的相关性 + 组成集合 = 最大连通分量 = 图的最大连通子图的大小
    - 无向图的G的极大连通子图称为G的连通分量（Connected）。任何连通图的连通分量都只有一个，即使是其本身，非连通的无向图有多个连通分量。
    - 可以使用BFS或DFS来计算线性时间内图的连通分量（以图的顶点和边的数量表示）
        - DFS可以通过递归搜索找到所有的连通分量。
        - BFS可以按层级搜索最短距离到达其他连通分量。
3. 算法简单描述
    - dfs递归标记已访问并统计连通分量大小
    - 主循环中找到未访问的新起点，从起点dfs获取最大大小
    - 读入数据，调用函数，处理输入输出

**一种解题步骤：**

1. 初始化一个空的邻接矩阵`matrix`来表示图。
2. 定义DFS搜索函数`dfs`，输入为当前节点`i`，访问计数`count`，已访问标记数组`visited`
    - 在`dfs`函数中，先标记节点`i`已访问，计数`count`加1。
    - 然后遍历该节点`i`的所有出边`j`，如果`j`未访问过，则递归调用`dfs`搜索`j`。
3. 在主函数中，先初始化所有节点未访问：
    - 遍历每个节点，如果一个节点未被访问，说明它是一个新的连通分量。
    - 重新初始化访问计数`count`为0。
    - 从该节点开始dfs，统计`count`大小。
    - `record`下最大的`count`作为结果。

## 解题代码

```python
def dfs(matrix, i, count, visited):
    # 标记节点i已访问
    visited[i] = 1
    # 记录遍历的结点数
    count[0] += 1

    for j in range(len(matrix)):
        # 对邻接矩阵中相连结点做DFS
        if matrix[i][j] == 1 and visited[j] == 0:
            dfs(matrix, j, count, visited)


def solve_method(matrix):
    # 初始化访问数组    
    visited = [0] * len(matrix)
    res = 0

    for i in range(len(matrix)):
        # 发现新连通分量进行DFS 
        if visited[i] == 0:
            count = [0]
            dfs(matrix, i, count, visited)
            # 记录最大结点数
            res = max(res, count[0])

    return res


if __name__ == '__main__':
    matrix = [[1, 1, 0, 0],
              [1, 1, 1, 0],
              [0, 1, 1, 0],
              [0, 0, 0, 1]]
    assert solve_method(matrix) == 3
```

## 扩展：图相关算法

- **图的表示**：有效存储和处理图信息的方式，例如邻接矩阵、邻接表、关联矩阵等。
- **图的遍历**：从一个顶点出发，按照一定的规则访问图中的其他顶点，例如深度优先搜索、广度优先搜索、拓扑排序等。
- **图的连通性**：判断图中是否存在一条或多条路径连接两个顶点，以及如何找出这些路径，例如连通分量、强连通分量、欧拉回路、哈密顿回路等。
- **图的着色**：给图中的顶点或边分配一种或多种颜色，满足一定约束条件，例如四色定理、染色多项式、列表着色等。
- **图的匹配**：在图中选出一些边，使得每个顶点至多与一条边相邻，以及如何找出最优的匹配，例如最大匹配、完美匹配、稳定婚姻问题等。
- **图的流量**：在图中给每条边赋予一个流量值，使得满足一定的限制条件，例如最大流、最小割、费用流等。
- **图的优化**：在图中找出一些特殊的子图或路径，使得满足一定的目标函数，例如最小生成树、最短路径、旅行商问题等。

- 在一个基于项目规划建模的有向无环带权图中，找出一条权重最大的关键路径，可以用来安排工程项目、生产计划、课程设计等。
- 在一个带权的图中，找出两个顶点之间权值最小的路径，可以用来规划交通路线、网络路由、导航系统等。最常见的求解最短路径的算法是迪杰斯特拉算法，它以起始点为中心向外层层扩展，直到扩展到终点为止。

## 参考资料

1. [图论：连通分量和强连通分量](https://zhuanlan.zhihu.com/p/37792015)
2. [AmosCloud Wiki题解](https://wiki.amoscloud.com/zh/ProgramingPractice/NowCoder/Adecco/Topic0244)
3. [从七桥问题开始：全面介绍图论及其应用](https://zhuanlan.zhihu.com/p/34442520)

## 自解
```python
# 处理输入
n = int(input())
in_list = []
for _ in range(n):
    in_list.append(list(map(int, input().split())))

ans = 0
for i in range(n):
    stack = [i]
    visited = set()
    while stack:
        i = stack.pop()
        visited.add(i)
        for j in range(n):  # [1, 1, 0, 0]
            if in_list[i][j] == 1 and j not in visited:
                stack.append(j)
    ans = max(ans, len(visited))
print(ans)


```
