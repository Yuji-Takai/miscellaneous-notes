### Example 1
**[Problem]** Team photo day 2 
- Input: 
- Output: largest number of teams that can be photographed simultaneously subject to the same constraints as [Example](Sorting/Computation.md)

1. O(|V| + |E|) time (|V|: number of teams)
    ```
    class GraphVertex:
        def __init__(self):
            self.edges = []
            self.max_distance = 0
    
    def find_largest_number_teams(graph):
        def dfs(curr):
            curr.max_distance = max(((vertex.max_distance if vertex.max_distance != 0 else dfs(vertex)) + 1 for vertex in curr.edges), defualt=1)
            return curr.max_distance
        return max(dfs(g) for g in graph if g.max_distance == 0)
    ```
    - use a DAG G
        + vertices corresponding to the teams
        + edges from vertex X to Y iff Team X can be placed behind Team Y
        + every sequence of teams where a team can be placed behind its predecessor corresponds to a path in G
    - finding longest sequence == finding the longest path in the DAG G
        1. topologically order the vertices in G
        2. longest path terminating at vertex v == maximum of the longest paths terminating at v's fan-ins concatenated with v itself
    - topolgoical ordering == O(|V| + |E|) time --> dominates the computation time
    - |E|: depends on the heights, but can be as high as O(|V|<sup>2</sup>) ex: when there is path of length |V| - 1
    
    
