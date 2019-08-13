### Example 1
**[Problem]** Compute binary tree nodes in order of increasing depth
- Input: binary tree 
- Output: array consisting of the keys at the same level 
    + Keys should appear in the order of the corresponding nodes' depths, breaking ties from left to right

1. O(n) time, O(m) space (m: maximum number of nodes at any single depth)
    ```
    def binary_tree_depth_order(tree):
        result = []
        if not tree:
            return result
        curr_depth_nodes = [tree]
        while curr_depth_nodes:
            result.append([curr.data for curr in curr_depth_nodes])
            curr_depth_nodes = [child for curr in curr_depth_nodes for child in (curr.left, curr.right) if child]
        return result
    ```
    - use a queue of nodes to store nodes at depth i and a queue of nodes at depth i + 1
    - after all nodes at depth i are processed, done with that queue, and can start processing the queue with nodes at depth i + 1, putting the depth i + 2 nodes in a new queue