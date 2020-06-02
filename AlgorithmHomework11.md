# Algorithm Homework 11

> 姓名：阙建明
>
> 学号：1901213139

## 作业要求：

Leetcode: 743, 847

- ### Leetcode problem 743

  > [网络延迟时间](https://leetcode-cn.com/problems/network-delay-time/)

  - **算法思路：** 
1. 使用Dijkstra算法找到发信号的点到其它所有点的最短路径（时间作为边的权重）；
    2. 结束时判断是否遍历了所有的点，没有则返回-1，有则返回所有点里面距离最大的点的距离作为总的耗时。
    3. 遇到大于限定跳数就跳出进行剪枝；
  4. 遇到目标节点就直接返回其最小开销，结束算法。
  
- **Leetcode提交结果：**
  
  ![image-20200602172430250](AlgorithmHomework11.assets/image-20200602172430250.png)
  
- **代码：**
  
    ```python
    class Solution:
        def networkDelayTime(self, times: List[List[int]], N: int, K: int) -> int:
            # 构造图
            graph = collections.defaultdict(list)
            for u, v, w in times:
                graph[u].append((w, v))
    
            # 用一个堆来存储已只一个非无穷的距离，但是还没用进行松弛的item => 每个item包含 当前已知的源到点的距离，以及点的编号
            pq = [(0, K)]
    
            # 记录已经松弛的点
            visited = {}
            while pq:
                distance, node = heapq.heappop(pq)
                # 这边过滤到多余push的点
                if node in visited:
                    continue
                visited[node] = distance
                for distance2, neighbour in graph[node]:
                    if neighbour not in visited:
                        # 遍历每个邻居的时候都会执行push操作，可能有一个点被push若干次，但是由于堆的特性，最先被访问到的一定是距离最小的那个
                        heapq.heappush(pq, (distance + distance2, neighbour))
            return max(visited.values()) if len(visited) == N else -1
    
    ```
  
- ### Leetcode problem 934

  > [最短的桥](https://leetcode-cn.com/problems/shortest-bridge/)

  - **算法思路：** 
    1. 以每个点为起点，同时进行BFS搜索；
    2. 用 `(cover, cur)` 元组代表一个状态 （State）=> 其中cover表示当前已经走过的节点（用二进制位的压缩表示，每个二进制位代表一个节点是否走过），cur表示当前所出的节点;
    3. 用dict字典记录达成每个状态（State）所需的最小跳数;
  4. 每次从队列中取出一个状态，遍历当前节点的所有邻居转移到若干个新状态，添加到队列当中（同时使用dict来过滤掉一些不必要的状态）。
  - **Leetcode提交结果：**

  ![image-20200602192555935](AlgorithmHomework11.assets/image-20200602192555935.png)

  - **代码：**
  
    ```python
    class Solution:
        def shortestPathLength(self, graph: List[List[int]]) -> int:
            N = len(graph)
            # 使用队列，保证先进先出，可以用来做BFS遍历
            queue = collections.deque((1 << i, i) for i in range(N))
            # 最短路径必然小于N * N
            dist = collections.defaultdict(lambda: N * N)
            # 结束标记
            END = (1 << N) - 1
            # 初始化，出发点所走路径为0
            for i in range(N):
                dist[(1 << i, i)] = 0
            while queue:
                cover, cur = queue.popleft()
                d = dist[(cover, cur)]
                if cover == END:
                    return d
                for child in graph[cur]:
                    newCover = cover | (1 << child)
                    if d + 1 < dist[newCover, child]:       # 每个状态只记录最短路径
                        dist[newCover, child] = d + 1
                        queue.append((newCover, child))
    ```



