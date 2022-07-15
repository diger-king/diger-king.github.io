---

title: (Algorithm) 그래프 탐색이란?
layout: post
description: DFS/BFS
post-image: https://i.ytimg.com/vi/5WnfXh0YQcQ/hq720.jpg?sqp=-oaymwEhCK4FEIIDSFryq4qpAxMIARUAAAAAGAElAADIQj0AgKJD&rs=AOn4CLCes55OA2RcBR9zjGGcNSQtN3Jxqw

tags:
- Graph
- DFS
- BFS
---

# Difference between BFS and DFS Binary Tree


|BFS| 	DFS|
|---|---|
|BFS finds the shortest path to the destination.|DFS goes to the bottom of a subtree, then backtracks.|
|The full form of BFS is Breadth-First Search.| 	The full form of DFS is Depth First Search.|
|It uses a queue to keep track of the next location to visit.| 	It uses a stack to keep track of the next location to visit.|
|BFS traverses according to tree level.| 	DFS traverses according to tree depth.|
|It is implemented using FIFO list.| 	It is implemented using LIFO list.|
|It requires more memory as compare to DFS.| 	It requires less memory as compare to BFS.|
|This algorithm gives the shallowest path solution.| 	This algorithm doesn’t guarantee the shallowest path solution.|
|There is no need of backtracking in BFS.| 	There is a need of backtracking in DFS.|
|You can never be trapped into finite loops.| 	You can be trapped into infinite loops.|
|If you do not find any goal, you may need to expand many nodes before the solution is found.| 	If you do not find any goal, the leaf node backtracking may occur.|

<br>
<br>



> 답의 깊이가 깊지 않으면 BFS가 더 적합하다.
> 
> 답의 깊이가 깊고 답이 흔치 않다면 DFS는 오래걸릴수있고 BFS는 빠를 수 있다.
> 
> 트리의 너비가 엄청 넓으면 BFS는 많은 메모리를 필요로 하므로 부적합할 수 있다.
> 
> 답이 자주 있지만 깊이가 깊다면 BFS보다 DFS가 더 적합하다

[DFS VS BFS](https://stackoverflow.com/questions/3332947/when-is-it-practical-to-use-depth-first-search-dfs-vs-breadth-first-search-bf)

---

# Applications of BFS

### Un-weighted Graphs:
BFS algorithm can easily create the shortest path and a minimum spanning tree to visit all the vertices of the graph in the shortest time possible with high accuracy.

### P2P Networks:
BFS can be implemented to locate all the nearest or neighboring nodes in a peer to peer network. This will find the required data faster.


### Web Crawlers:
Search engines or web crawlers can easily build multiple levels of indexes by employing BFS. BFS implementation starts from the source, which is the web page, and then it visits all the links from that source.

### Network Broadcasting:
A broadcasted packet is guided by the BFS algorithm to find and reach all the nodes it has the address for.

# Applications of DFS

### Weighted Graph:
In a weighted graph, DFS graph traversal generates the shortest path tree and minimum spanning tree.

### Detecting a Cycle in a Graph:
A graph has a cycle if we found a back edge during DFS. Therefore, we should run DFS for the graph and verify for back edges.

### Path Finding:
We can specialize in the DFS algorithm to search a path between two vertices.

### Topological Sorting:
It	is primarily used for scheduling jobs from the given dependencies among the group of jobs. In computer science, it is used in instruction scheduling, data serialization, logic synthesis, determining the order of compilation tasks.

### Searching Strongly Connected Components of a Graph:
It used in DFS graph when there is a path from each and every vertex in the graph to other remaining vertexes.

### Solving Puzzles with Only One Solution:
DFS algorithm can be easily adapted to search all solutions to a maze by including nodes on the existing path in the visited set.


---

# DFS vs BFS 선택 상황

> DFS
> 
> 재귀, 백트래킹을 통한 모든 경우의 수를 살펴 볼 때
> 
> DFS를 이용하여 순열, 조합 구현

<br>

> BFS
>
> 깊이를 이용한 최단 경로를 살펴볼 때


---

# DFS Base

    from collections import deque
    import sys
    
    input = sys.stdin.readline
    
    def dfs(graph, root):
        visited = []
        stack = [root]
        
            while stack:
                n = stack.pop()
        
                if n not in visited:
                    visited.append(n)
                    stack += graph[n] - set(visited)
            return visited

---

# BFS Base
    
    def bfs(graph, root):
        visited = []
        queue = deque([root])
        
            while queue:
                n = queue.popleft()
        
                if n not in visited:
                    visited.append(n)
                    queue += graph[n] - set(visited)
        
            return visited
---

DFS/BFS 문제를 풀려면 암기가 되고, 이해를 해야한다. 꼭 외워놓자