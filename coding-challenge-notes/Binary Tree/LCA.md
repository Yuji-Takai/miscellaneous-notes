### Example 1
**[Problem]** Compute the lowest common ancestor in a binary tree 
- Input: root of a b-tree (nodes do NOT have a parent field) and 2 nodes 
- Output: LCA of the 2 nodes
    + any 2 nodes in a b-tree have a common ancestor == root
    + **Lowest Common Ancestor (LCA)**: the node furthest from the root that is an ancestor of 2 nodes
    + Example application: when computing CSS that is applicable to a particular DOM element

1. O(n) time and O(h) space
    ```
    def lca(tree, node0, node1):
        Status = collections.namedtuple('Status', ('num_target_nodes', 'ancestor'))

        def lca_helper(tree, node0, node1):
            if not tree:
                return Status(0, None)
            left_result = lca_helper(tree.left, node0, node1)
            if left_result.num_target_nodes == 2:
                return left_result
            right_result = lca_helper(tree.right, node0, node1)
            if right_result.num_target_nodes == 2:
                return right_result
            num_target_nodes = (left_result.num_target_nodes + right_result.num_target_nodes + (node0, node1).count(tree))
            # list.count(val) == count the number of instances of val in list/tuple
            return Status(num_target_nodes, tree if num_target_nodes == 2 else None)
        
        return lca_helper(tree, node0, node1).ancestor
    ```
    - instead of simply returning a Boolean indicating that both nodes are in that subtree, compute the LCA directly
    - returns an object with 2 fields - the first is an integer indicating how many of the 2 ndoes were present in that subtree, and the second is their LCA, if both nodes were present 
    - structurally similar to postorder traversal

### Example 2
**[Problem]** Compute the LCA when nodes have parent pointers
- Input: root of a b-tree (each node has a parent pointer) and 2 nodes
- Output: LCA of the 2 nodes

1. O(h) time, O(1) space
    ```
    def lca(node0, node1):
        def get_depth(node):
            depth = 0
            while node:
                depth += 1
                node = node.parent
            return depth
        
        depth0, depth1 = map(get_depth, (node0, node1))
        if depth1 > depth0:
            node0, node1 = node1, node0
        
        depth_diff = abs(depth0 - depth1)
        while depth_diff:
            node0 = node0.parent
            depth_diff -= 1
        while node0 is not node1:
            node0, node1 = node0.parent, node1.parent
        return node0
    ```
    - fact that the 2 nodes have a common ancestor, namely the root
    - if the nodes are at the same depth, can move up the tree in tandem from both nodes, stopping at the first common node, which is the LCA
    - in order to avoid having to keep the set of nodes if the nodes are not at the same depth, ascend from the deeper node to get the same depth as the shallow node, and perform the tandem upward movement
    - computing depth is straightforward since the nodes have parent field = O(h) time and O(1) space