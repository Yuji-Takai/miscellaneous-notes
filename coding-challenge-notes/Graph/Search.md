### Example 1
**[Problem]** Can team A beat team B
- Input: list of outcomes of matches between pairs of team, with each outcome being a win or loss, and teams A and B
- Output: Can A beat B?

1. O(|E|) time
    ```
    MatchResult = collections.namedtuple('MatchResult', ('winning_team', 'losing_team'))
    
    def can_team_a_beat_team_b(matches, team_a, team_b):
        def build_graph():
            graph = collections.defaultdict(set)
            for match in matches:
                graph[match.winning_team].add(match.losing_team)
            return graph

        def is_reachable_dfs(graph, curr, dest, visited=set()):
            if curr == dest:
                return True
            elif curr in visited or curr not in graph:
                return False
            visited.add(curr)
            return any(is_reachable_dfs(graph, team, dest) for team in graph[curr])
        return is_reachable_dfs(build_graph(), team_a, team_b)
    ```
    - model the problem using a graph
    - vertices: teams, edge from one team to another: team corresponding to the source vertex has beaten the team corresponding to the destination vertex 
    - use graph reachability (DFS or BFS) to perform the check

### Example 2
**[Problem]** Search a maze 
- Input: 2D array of black and white entries representing a maze (black: walls, white: open areas) with designated entrance and exit points
- Output: Path from the entrance to the exit, if one exists

1. O(|V| + |E|) time
    ```
    WHITE, BLACK = range(2)
    Coordinate = collections.namedtuple('Coordinate', ('x', 'y'))

    def search_maze(maze, s, e):
        def search_maze_helper(cur):
            if not (0 <= curr.x < len(maze) and 0 <= curr.y < len(maze[curr.x]) and maze[cur.x][cur.y] == WHITE):
                return False
            path.append(cur)
            maze[cur.x][cur.y] = BLACK
            if cur == e:
                return True
            if any(map(search_maze_helper, map(Coordinate, (cur.x - 1, cur.x + 1, cur.x, cur.x), (cur.y, cur.y, cur.y - 1, cur.y + 1)))):
                return True
            del path[-1]
            return False
        path = []
        search_maze_helper(s)
        return path
    ```
    - model the maze as a graph
    - vertex: white pixel
        + index the vertices based on the coordinates of the corresponding pixel
        + v<sub>i,j</sub> corresponds to the white entry at (i, j) in the 2D array
    - edge: adjacent white pixels
    - change color of the entry that has already been visited instead of storing a visited list to save space
    - BFS can be used but queue has to be explicity coded 
        + since the problem did NOT ask for a shortest path, better to use DFS

### Example 3
**[Problem]** Paint a Boolean matrix 
- Input: n x m Boolean array A and entry (x,y)
- Output: A with the color of region associated with (x, y) flipped
    + A(a,b): color at (a, b)
    + 2 entries adjacent: if one is to the left, right, above, or below the other
    + path from entry e<sub>0</sub> to entry e<sub>1</sub>: sequence of adjacent entries, starting at e<sub>0</sub>, ending e<sub>1</sub>, with successive entries being adjacent
    + region associated with a point (i, j): all points (i', j') such that there exists a path from (i, j) to (i', j') in which all entries are the same color (color must same at (i, j) and at (i', j'))

1.  O(mn) time, O(m + n) space
    ```
    def flip_color(x, y, image):
        color = image[x][y]
        q = collections.deque([(x, y)])
        image[x][y] = 1 - image[x][y] # flip color
        while q:
            x, y = q.popleft()
            for next_x, next_y in ((x, y + 1), (x, y - 1), (x + 1, y), (x - 1, y)):
                if (0 <= next_x < len(image) and 0 <= next_y < len(image[next_x]) and image[next_x][next_y] == color):
                    image[next_x][next_y] = 1 - image[next_x][next_y]
                    q.append((next_x, next_y))
    ```
    - entries as vertices, with vertices corresponding to adjacent entries begin connected by edges
    - search for all vertices whose color is the same as that of (x, y) that are reachable from (x, y)
        + BFS natural when starting with a set of vertices
        + use a queue to store such vertices
        + queue initialized to (x, y)
        + queue popped iteratively
            - let the popped point p
            1. record p's initial color
            2. flip p's color
            3. examine p's neighbors - any neighbor which is the same color as p's initial color is added to the queue
        + end when queue is empty
    - any point that is added to the queue is reachable from (x, y) via a path consisting of points of the same color, and all points reachable from (x, y) via points of the same color will eventually be added to the queue

2. O(mn) time
    ```
    def flip_color(x, y, image):
        color = image[x][y]
        image[x][y] = 1 - image[x][y]
        for next_x, next_y in ((x, y + 1), (x, y - 1), (x + 1, y), (x - 1, y)):
            if (0 <= next_x < len(image) and 0 <= next_y < len(image[next_x]) and image[next_x][next_y] == color):
                flip_color(next_x, next_y, image)
    ```
    - recursive solution using DFS
    - implicitly uses a stack == function call stack

### Example 4
**[Problem]** Compute enclosed regions 
- Input: m x n 2D array A whose entries are either W or B
- Output: A with all Ws that cannot reach the boundary replaced with a B
    + [EXAMPLE]
        ```
        [[B, B, B. B],
         [W, B, W, B],  -> Ws that cannot reach boundary
         [B, W, W, B],  -> (1, 2), (2, 1), (2, 2)
         [B, B, B, B]]
        ```
1. O(mn) time
    ```
    def fill_surrounded_regions(board):
        n, m = len(board), len(board[0])
        q = collections.deque([(i, j) for k in range(n) for i, j in ((k, 0), (k, m -1))] + [(i, j) for k in range(m) for i, j in ((0, k), (n - 1, k))])
        while q:
            x, y = q.popleft()
            if 0 <= x < n and 0 <= y < m and board[x][y] == 'W':
                board[x][y] = 'T'
                q.extend([(x - 1, y), (x + 1, y), (x, y - 1), (x, y ; 1)])
        board[:] = [['B' if c != 'T' else 'W' for c in row] for row in board]
    ```
    - focus on reverse problem = identify Ws that can reach the boundary
        + if a W adjacent to a W that can reach the boundary, then the first W can reach it too
        1. Ws on the boundary are the initial set
        2. subsequently find Ws neighboring the boundary Ws, and iteratively grow the set
        3. whenever find a new W that can reach the boundary, need to record it, and at some stage search for new Ws from it
        + queue used to track Ws to be processed
        + BFS starting with a set of vertices rather than a single vertex

### Example 5
**[Problem]** Deadlock detection 
- Input: directed graph
- Output: true if the graph contains a cycle
    + **Deadlock**: situtation in which 2 or more competing actions are each waiting for the other to finish, which precludes all these actions from progressing
    + Wait-for graph: 
        - track which other prcesses a process is currently blocking on
        - nodes: processes
        - edge from process P to Q: Q is holding a resource that P needs, and thus P is waiting for W to release its lock on that resource
        - cycle in this graph == deadlock

1. O(|V| + |E|) time, O(|V|) space (space: max stack depth)
    ```
    class GraphVertex:
        WHITE, GRAY, BLACK = range(3)
        def __init__(self):
            self.color = GraphVertex.WHITE
            self.edges = []
    
    def is_deadlocked(graph):
        def has_cycle(cur):
            if cur.color == GraphVertex.GRAY:
                return True
            cur.color = GraphVertex.GRAY
            if any(next.color != GraphVertex.BLACK and has_cycle(next) for next in cur.edges):
                return True
            cur.color = GraphVertex.BLACK
            return False

        return any(vertex.color == GraphVertex.WHITE and has_cycle(vertex) for vertex in graph)
    ```
    - check existence of a cycle in G by running DFS on G
        + DFS maintains a color for each vertex:
            1. initially all vertices are white
            2. when a vertex is first discovered, it is colored gray
            3. when DFS finishes processing a vertex, that vertex is colored black
        + as soon as discover an edge from a gray vertex back to a gray vertex, a cycle exists in G and can stop
        + if there exists a cycle, once first reach vertex in the cycle (*v*), will visit its predecessor in the cycle (*u*) before finishing processing *v* == find an edge from a gray to gray vertex
        + a cycle exists if and only if DFS discovers an edge from a gray vertex to a gray vertex
    - since the graph may not be strongly connected, must examine each vertex, and run DFS from it if it has not already been explored

### Example 6
**[Problem]** Clone a graph 
- Input: reference to a vertex u
- Output: copy of u (copy of graph on the vertices reachable from u)
    + consider vertex type for a directed graph in which there are 2 fields
        1. an integer label
        2. a list of references to other vertices 

1. O(|V|) space (from hashtable and BFS queue)
    ```
    class GraphVertex:
        def __init__(self, label):
            self.label = label
            self.edges = []
    
    def clone_graph(graph):
        if not graph:
            return None
        
        q = collection.deque([graph])
        vertex_map = {graph: GraphVertex(graph.label)}
        while q:
            v = q.popleft()
            for e in v.edges:
                if e not in vertex_map:
                    vertex_map[e] = GraphVertex(e.label)
                    q.append(e)
                vertex_map[v].edges.append(vertex_map[e])
        return vertex_map[graph]
    ```
    - traverse the graph starting from u
    - each time encounter a vertex or an edge that is not yet in the clone, add it to the clone
    - recognize new vertices by maintaining a hash table mapping vertices in the original graph to their counterparts in the new graph
    - any standard graph traversal algorithm works

### Example 7
**[Problem]** Making wired connections 
- Input: set of pins (vertices) and set of wires connecting pair of pins (edges) of a printed circuit board (PCB)
- Output: division if it is possible to place some pins on the left half of a PCB, and the remainder on the right half, such that each wire is between left and right halves

1. O(p + w) time, O(p) space (p: number of pins, w: number of wires)
    ```
    class GraphVertex:
        def __init__(self):
            self.d = -1
            self.edges = []
    
    def is_any_placement_feasible(graph):
        def bfs(s):
            s.d = 0
            q = collections.deque([s])
            while q:
                for t in q[0].edges:
                    if t.d == -1: # unvisited vertex
                        t.d = q[0].d + 1
                        q.append(t)
                    elif t.d == q[0].d:
                        return False
                del q[0]
            return True
        return all(bfs(v) for v in graph if v.d == -1)
    ```
    - use connectivity information to guide the partitioning
        + assume the pins are numbered from 0 to p - 1
        + create an undirected graph G whose vertices are the pins
        + add an edge between pairs of vertices if the corresponding pins are connected by a wire 
        + if G is not connected, the connected components can be analyzed independently
    1. Run BFS on G beginning with any vertex v<sub>0</sub>
    2. assign v<sub>0<sub> arbitrarily to lie on the left half
        + all vertices at an odd distance from v<sub>0</sub> are assigned to the right half
    
    - when performing BFS on an undirected graph, all newly discovered edges will either be:
        1. from vertices which are at a distance d from v<sub>0</sub> to undiscovered vertices (which will then be at a distance d + 1 from v<sub>0</sub>)
        2. from vertices which are at a distance d to vertices which are also at a distance d
    
    1. first assume never encounter an edge from a distance k vertex to a distance k vertex
        + in this case, each wire is from a distance k vertex to a distance k + 1 vertex == all wires between the left and right halves
    2. if any edge is from a distance k vertex to a distance k vertex, stop
        + pins cannot be partioned into left and right halves as desired
    
    - a cycle in which the vertices can be partitioned into 2 sets must have an even number of edges = has to go back and forth between the sets and terminate at the starting vertex, and each back and forth adds 2 edges
        + the vertices in an odd length cycle cannot be partioned into 2 sets such that all edges are between the sets
    - 2-colorable (bipartite) graph problem

### Example 8
**[Problem]** Transform one string to another 
- Input: dictionary D and 2 string s and t
- Output: length of a shortest production sequence if s does produce t; otherwise -1
    + s produces t: there exists a sequence of strings from the dictionary P = [s<sub>0</sub>, s<sub>1</sub>, ..., s<sub>n-1</sub>] such that the first string is s, the last string is t, and adjacent strings have the same length and differ in exactly one character
        - the sequence == production sequence

1. O(d<sup>2</sup>) time (d: number of words in dictionary)
    ```
    def transform_string(D, s, t):
        StringWithDistance = collections.namedtuple('StringWithDistance', ('candidate_string', 'distance'))
        q = collections.deque([StringWithDistance(s, 0)])
        D.remove(s)
        while q:
            f = q.popleft()
            if f.candidate_string == t:
                return f.distance
            
            for i in range(len(f.candidate_string)):
                for c in string.ascii_lowercase: # iterates through 'a' ~ 'z'
                    cand = f.candidate_string[:i] + c + f.candidate_string[i+1:]
                    if cand in D:
                        D.remove(cand)
                        q.append(StringWithDistance(cand, f.distance + 1))
        return -1
    ```
    - model problem using graphs
        + vertices: strings from the dictionary
        + edge (u, v): strings corresponding to u and v differ in exactly one character
            - "differs in one character" == symmetric relationship --> undirected graph!
    - production sequence == path in graph G
        + need to find the shortest path from s to t in G
        + shortest paths in an undirected graph can be computed using BFS
    - number of vertices == number of words in the dictionary == d
    - number of edges == d<sup>2</sup> in worst case
        + if the string length n is less than d, then the maximum number of edges out of a vertex is O(n) --> O(nd) time