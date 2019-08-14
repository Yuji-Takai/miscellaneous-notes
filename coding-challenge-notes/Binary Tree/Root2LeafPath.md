### Example 1
**[Problem]** Sum the root-to-leaf paths in a binary tree
- Input: root of b-tree in which each node contains a binary digit (0 or 1)
- Output: sum of the binary numbers represented by the root-to-leaf paths

1. O(n) time, O(h) space
    ```
    def sum_root_to_leaf(tree, partial_path_sum=0):
        if not tree:
            return 0
        partial_path_sum = partial_path_sum * 2 + tree.data
        if not tree.left and not tree.right:
            return partial_path_sum
        return (sum_root_to_leaf(tree.left, partial_path_sum) +  sum_root_to_leaf(tree.right, partial_path_sum))
    ```
    - paths share nodes 
    - no need to repeat computations accross the shared nodes
    - to compute the integer for the path from the root to any node, take the integer for the node's parent, double it, and add the bit at that node
    - computation of the sum of all root to leaf node:
        + each time visiting a node, compute the integer it encodes using the number for its parent
        + if the node is a leaf, return its integer
        + if not a leaf, return the sum of the results from its left and right children
    
### Example 2
**[Problem]** Find a root-to-leaf path with specified sum
- Input: integer and root of a binary tree with integer node weight
- Output: True if there exists a leaf whose path weight equals the given integer
    + **Path weight**: the sum of the integers on the unique path from the root to a given node

1. O(n) time, O(h) space
    ```
    def has_path_sum(tree, remaining_weight):
        if not tree:
            return False
        if not tree.left and not tree.right:
            return remaining_weight == tree.data
        return has_path_sum(tree.left, remaining_weight - tree.data) or has_path_sum(tree.right, remaining_weight - tree.data)
    ```
    - traverse the tree, while keeping track of difference of the root-to-node path sum and the target value
    - as soon as a leaf is encountered and the remaining weight is equal to the leaf's weight, return true
    - short circuit evaluation of the check ensures not to process additional leaves