# Graph


+ [Course Schedule II](#course-schedule-ii)

+ [Course Schedule](#course-schedule)

+ [Number of Islands](#number-of-islands)

+ [Is Graph Bipartite?](#is-graph-bipartite)

+ [Cheapest Flights Within K Stops](#cheapest-flights-within-k-stops)

+ [Shortest Path in Binary Matrix](#shortest-path-in-binary-matrix)

+ [Maximum Depth Of N-ary Tree](#maximum-depth-of-n-ary-tree)

## Course Schedule II

https://leetcode.com/problems/course-schedule-ii/

```python
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        sortedOrder = []
        
        if numCourses <= 0:
            return sortedOrder
        
        inDegree = {i:0 for i in range(numCourses)}
        adMatrix = {i:[] for i in range(numCourses)}
        
        for edge in prerequisites:
            parent, child = edge[1], edge[0]
            inDegree[child] += 1
            adMatrix[parent].append(child)
        
        sources = deque()
        for key in inDegree:
            if inDegree[key] == 0:
                sources.append(key)
        
        while sources:
            source = sources.popleft()
            sortedOrder.append(source)
            for child in adMatrix[source]:
                inDegree[child] -= 1
                if inDegree[child] == 0:
                    sources.append(child)
        
        if len(sortedOrder) != numCourses:
            return []
        
        return sortedOrder
```

## Course Schedule

https://leetcode.com/problems/course-schedule/ 

```python
def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
    g = defaultdict(list)
    for item in prerequisites:
        a = item[0]
        b = item[1]
        g[b].append(a)  
    vis = [-1 for i in range(numCourses)]
        
def dfs(root):
    vis[root] = 1
    for adj in g[root]:
        if(vis[adj]==-1):
            if(dfs(adj)):
                return True
        elif(vis[adj]==1):
            return True
    vis[root]=2
    return False 
```


## Number of Islands

https://leetcode.com/problems/number-of-islands/

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        def surroundCheck(binaryMatrix,i,j):
            lr=len(binaryMatrix)-1 
            lc=len(binaryMatrix[0])-1


            if binaryMatrix[i][j]=="0":
                return
            binaryMatrix[i][j]="0" 


            if (i+1) <= lr:
                 surroundCheck(binaryMatrix,i+1,j)

            if (j+1) <= lc:
                surroundCheck(binaryMatrix,i,j+1)

            if (i-1) >= 0:
                surroundCheck(binaryMatrix,i-1,j) 

            if (j-1) >= 0:
                surroundCheck(binaryMatrix,i,j-1)  

        numIsland=0
        for i in range(len(grid)):
            for j in  range(len(grid[0])):

                if grid[i][j]=="1":
                    surroundCheck(grid,i,j)
                    numIsland+=1

        return  numIsland
```

## Is Graph Bipartite?

https://leetcode.com/problems/is-graph-bipartite/

```python
class Solution:
    def isBipartite(self, graph: List[List[int]]) -> bool:
        color = {}
        for node in range(len(graph)): 
            if node not in color:
                stack = [node]
                color[node] = 0 
                while stack:
                    node = stack.pop()
                    for nei in graph[node]:
                        if nei not in color:
                            if color[node] == 0: 
                                color[nei] = 1
                            elif color[node] == 1:
                                color[nei] = 0
                            stack.append(nei)
                        else:
                            if color[nei] == color[node]:
                                return False
        return True
```


## Cheapest Flights Within K Stops

https://leetcode.com/problems/cheapest-flights-within-k-stops/

```python
class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        G = defaultdict(set)
        for f, t, p in flights: 
            G[f].add((t, p))
            
        P = defaultdict(lambda: inf)
            
        res = inf
        q = [(src, 0, 0)]
        for loc, price, stops in q: 
            if stops > k + 1: 
                break
            if loc == dst: 
                res = min(res, price)
            for to, p in G[loc]: 
                if P[to] > price + p: 
                    P[to] = price + p
                    q.append((to, price + p, stops + 1))
                
        return res if res < inf else -1
```

## Shortest Path in Binary Matrix

https://leetcode.com/problems/shortest-path-in-binary-matrix/

```python
def shortestPathBinaryMatrix(self, grid: List[List[int]]) -> int:
    rows = len(grid)
    cols = len(grid[0])
    if grid[0][0] == 1:
        return -1
    m = [(0, 0, 1)]
    while len(m) > 0:
        x, y, z = m.pop(0)
        if x == rows - 1 and y == cols - 1:
            return z
        arr = [(x+1, y), (x, y-1), (x, y+1), (x-1, y+1), (x+1, y-1), (x-1, y-1), (x+1, y+1), (x-1, y)]
        for a, b in arr:
            if 0 <= a < rows and 0 <= b < cols and grid[a][b] == 0:
                grid[a][b] = 1
                m.append((a, b, z + 1))
    return -1
```

## Maximum Depth Of N-ary Tree

https://leetcode.com/problems/maximum-depth-of-n-ary-tree/ 

```python
def maxDepth(self, root: 'Node') -> int:
    if not root:
        return 0
    queue = [root]
    depth = 0
    while queue:
        for i in range(len(queue)):
            node = queue.pop(0)
            if node.children:
                for child in node.children:
                    queue.append(child)
        depth += 1
    return depth
```