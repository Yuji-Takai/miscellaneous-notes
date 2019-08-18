# Graph
## Python
### Graph
- **Directed Graph**: set V of vertices and a set E ⊂ V x V of edges 
    + edge e = (u, v) --> u: source, v: sink
    + **Path** from u to v: sequence of vertices [v<sub>0</sub>, v<sub>1</sub>, ..., v<sub>n-1</sub>] where v<sub>0</sub> = u, v<sub>n-1</sub> = v and each (v<sub>i</sub>, v<sub>i+1</sub>) is an edge
        - **Length**: number of edges traversed by the path
        - v is **reachable** from u: there exists a path from u to v
    + **Weakly Connected**: replacing all of its directed edges with undirected edges produces an undirected graph that is connected
    + **Connected**: the directed graph contains a directed path from u to v or a directed path from v to u for every pair of vertices u and v
    + **Strongly Connected**: the directed graph contains a directed path from u to v and a directed path from v to u for every pair of vertices u and v
- **Directed Acyclic Graph (DAG)**: directed graph in which there are no cycles (path which contain one ore more edges adn which begin and end at the same vertex)
    + **Source**: vertex which have no incoming edges
    + **Sink**: vertex which have no outgoing edges
    + **Topological ordering**: an ordering of vertices in which each edge is from a vertex earlier in the ordering to a vertex later in the ordering 
- **Undirected Graph**: tuple (V, E), with E as a set of unordered pairs of vertices
    + u and v are **connected**: undirected graph G contains a path from u to v (otherwise **disconnected**)
    + **Connected Graph**: every pair of vertices in the graph is connected 
    + **Connected component**: maximal set of vertices C such that each piar of vertices in C is connected in G
        - every vertex belongs to exactly one connected component
- **Tree (Free Tree)**: 
    + undirected graph that is connected but has no cycles
    + graph is free tree if and only if there exists a unique path between every pair of vertices
    + **Rooted Tree**: designated verticx is called the root, which leads to a parent-child relationship on the nodes
    + **Ordered Tree**: rooted tree in which each vertex has an ordering on its children 
        - Ordered Tree vs **Binary Tree**:
            1. Binary tree: node may have only one child, but that node may be a left or a right child, there are position and order associated with the children of nodes
            2. Ordered tree: no analogous notion exists for a node with a single child 
- **Spanning tree**: given a graph G = (V, E), if graph G' = (V, E') where E' ⊂ E is a tree, then G' is the spanning tree of G

- Use graph:
    + when problem involves *spatially connected* objects (ex: road segments between cities)
    + for analyzing any *binary relationship* between objects (ex: interlinked webpages, followers in a social graph) --> problems can be reduced to graph problem
+ Analyzing structure of a graph (ex: looking for cycles or connected components) --> use DFS
+ Optimization problems --> use BFS, Dijkstra's shorted path algorithm, and minimum spanning tree

### Graph Implementation
1. adjacency lists
    - each vertex v has a list of vertices to which it has an edge 
2. adjacency matrix
    - |V| x |V| Boolean-valued matrix indexed by vertices, with a 1 indicating the presence of an edge

### Graph search
- Depth-first search (DFS) and Breadth-first search (BFS)
    + Time Complexity: both linear time O(|V| + |E|)
    + Space Complexity:
        1. DFS: O(|V|), worst case = there is a path from the initial vertex covering all vertices without any repeats, and the DFS edges selected correspond to this path
        2. BFS: O(|V|), worst case = there is an edge from the initial vertex to all remaining vertices, implying that they will all be in the BFS queue simultaneously at some point
    + DFS: 
        + can be used to check for the presence of cycles 
        + include the concept of discovery time and finishing time for vertices
        + maintains a color for each vertex:
            1. initially all vertices are white
            2. when a vertex is first discovered, it is colored gray
            3. when DFS finishes processing a vertex, that vertex is colored black
            + a cycle exists if and only if DFS discovers an edge from a gray vertex to a gray vertex
    + BFS: can be used to compute distances from the start vertex

### Advanced graph problems
- 4 classes of complex graph problems that can be solved in polynomial time
1. **Shortest path**: given a graph, directed or undirected, with costs on the edges, find the minimum cost path from a given vertex to all vertices
    + variants: the shortest paths for all pairs of vertices, case where costs are all nonnegative
2. **Minimum spanning tree**: given an undirected graph G = (V, E), assumed to be connected, with weights on each edge, find a subset E' of the edges with minimum total weight such that the subgraph G' = (V, E') is connected
3. **Matching** given an undirected graph, find a maximum collection of edges subject to the constraint that every vertex is incident to a most one edge
    + ex: checking if bipartite graph 
    + variant: maximum weighted matching problem in which edges have weights and a maximum weight edge set is sought, subject to the matching constraint
4. **Maximum flow**: given a directed graph with a capacity for each edge, find the maximum flow from a given source to a given sink, where a flow is a function mapping edges to numbers satisfying conservation (flow into a vertex equals the flow out of it) and the edge capacities
    + variant: minimum cost circulation problem - generalizing maximum flow problem by adding lower bounds on edge capacities, and for each edge, a cost per unit flow
