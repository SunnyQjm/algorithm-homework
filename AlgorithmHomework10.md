# Algorithm Homework 10

> 姓名：阙建明
>
> 学号：1901213139

## 作业要求：

Leetcode: 787, 934, 692

- ### Leetcode problem 787

  > [K 站中转内最便宜的航班](https://leetcode-cn.com/problems/cheapest-flights-within-k-stops/)

  - **算法思路：** 

    1. 用Djkstra算法求解单源最短路径的思路；
    2. 遇到大于限定跳数就跳出进行剪枝；
    3. 遇到目标节点就直接返回其最小开销，结束算法。

  - **Leetcode提交结果：**

    ![image-20200507221022918](assets/1590673071603.png)

  - **代码：**

    ```python
    class Solution(object):
        def findCheapestPrice(self, n, flights, src, dst, K):
    
            # 构造图
            graph = collections.defaultdict(dict)
            for u, v, w in flights:
                graph[u][v] = w
    
            # 存储已确定最短路径的节点
            best = {}
    
            # 存储自src所指节点起始，到目的节点的的开销，跳数，和目的节点标号
            pq = [(0, 0, src)]
            
            # 用Djkstra求单源最短路径
            while pq:
                cost, k, place = heapq.heappop(pq)
                if k > K + 1 or cost > best.get((k, place), float('inf')):
                    continue
                if place == dst:
                    return cost
    
                for nei, wt in graph[place].items():
                    newCost = cost + wt
                    if newCost < best.get((k + 1, nei), float('inf')):
                        heapq.heappush(pq, (newCost, k + 1, nei))
                        best[k + 1, nei] = newCost
            return -1
    ```

- ### Leetcode problem 934

  > [最短的桥](https://leetcode-cn.com/problems/shortest-bridge/)

  - **算法思路：** 
    1. 首先用深度优先遍历找到两座岛屿，并随机选一座作为初始点source；
    2. 再用广度优先遍历的方法从source岛屿向target岛屿扩散
  - **Leetcode提交结果：**

  ![image-20200507222442113](assets/1590673939071.png)

  - **代码：**

    ```python
    class Solution(object):
        def shortestBridge(self, A):
            R, C = len(A), len(A[0])
    
            def neighbors(r, c):
                for nr, nc in ((r-1,c),(r,c-1),(r+1,c),(r,c+1)):
                    if 0 <= nr < R and 0 <= nc < C:
                        yield nr, nc
    
            def get_components():
                done = set()
                components = []
                for r, row in enumerate(A):
                    for c, val in enumerate(row):
                        if val and (r, c) not in done:
                            # Start dfs
                            stack = [(r, c)]
                            seen = {(r, c)}
                            while stack:
                                node = stack.pop()
                                for nei in neighbors(*node):
                                    if A[nei[0]][nei[1]] and nei not in seen:
                                        stack.append(nei)
                                        seen.add(nei)
                            done |= seen
                            components.append(seen)
                return components
    
            # 首先用深度优先遍历找到两座岛屿，并随机选一座作为初始点source
            source, target = get_components()
    
            #　用广度优先遍历的方法从source岛屿向target岛屿扩散
            queue = collections.deque([(node, 0) for node in source])
            done = set(source)
            while queue:
                node, d = queue.popleft()
                if node in target: return d-1
                for nei in neighbors(*node):
                    if nei not in done:
                        queue.append((nei, d+1))
                        done.add(nei)
    
    ```

- ### Leetcode problem 692

  > [前K个高频单词](https://leetcode-cn.com/problems/top-k-frequent-words/)

  - **算法思路：** 
    1. 首先调用collections.Counter统计单词出现的词频；
    2. 然后根据词频来从大到小进行排序；
    3. 最后取出前K个就是结果。
  - **Leetcode提交结果：**

  ![image-20200507225110291](assets/1590674294326.png)

  - **代码：**

    ```python
    class Solution(object):
        def topKFrequent(self, words, k):
            # 使用python的类库计算单词的词频
            count = collections.Counter(words)
            candidates = list(count.keys())
            
            # 根据词频排序
            candidates.sort(key=lambda w: (-count[w], w))
            return candidates[:k]
    ```

    