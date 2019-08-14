### Example 1
**[Problem]** Form a linked list from the leaves of a binary tree 
- Input: root of a binary tree
- Output: linked list from the leaves (appearing in left-to-right order) of the binary tree

1. O(n) time 
    ```
    def create_list_of_leaves(tree):
        if not tree:
            return []
        if not tree.left and not tree.right:
            return [tree]
        return create_list_of_leaves(tree.left) + create_list_of_leaves(tree.right)
    ```
    - if process the tree from left to right, the leaves occur in the desired order, so can incrementally add them to the result

### Example 2
**[Problem]** Compute the exterior of a binary tree
- Input: root of a b-tree
- Output: array representing the exterior of the b-tree
    + **Exterior**: the nodes from the root to the leftmost leaf (leaf that appears first in inorder), followed by the leaves in left-to-right order, followed by nodes from the rightmost leaf (leaf that appears last in inorder) to the root 

1. O(n) time
    ```
    def exterior_binary_tree(tree):
        def is_leaf(node):
            return not node.left and not node.right
        
        def left_boundary_and_leaves(subtree, is_boundary):
            if not subtree:
                return []
            return (([subtree] if is_bounary or is_leaf(subtree) else []) + 
                left_boundary_and_leaves(subtree.left, is_boundary) +
                left_boundary_and_leaves(subtree.right, is_boundary and not subtree.left))

        def right_boundary_and_leaves(subtree, is_boundary):
            if not subtree:
                return []
            return (right_boundary_and_leaves(subtree.left, is_boundary and not subtree.right) +
                right_boundary_and_leaves(subtree.right, is_boundary) +
                ([subtree] if is_boundary or is_leaf(subtree) else []))
        
        return ([tree] + left_boundary_and_leaves(tree.left, True) + right_boundary_and_leaves(tree.right, True) if tree else [])
    ```
    1. compute the nodes on the path from the root to the leftmost leaf and the leaves in the left subtree in one traversal 
    2. find the leaves in the right subtree followed by the nodes from the rightmost leaf to the root with another traversal

### Example 3
**[Problem]** Compute the right sibling tree
- Input: root of a perfect binary tree (each has an extra field called level-next == a node that will be used to compute a map from nodes to their right siblings)
- Output: original b-tree with each node's level-next field set to the node on its right, if one exists

1. O(n) time, O(1) space
    ```
    def construct_right_sibling(tree):
        def populate_children_next_field(start_node):
            while start_node and start_node.left:
                start_node.left.next = start_node.right
                start_node.right.next = start_node.next and start_node.next.left
                start_node = start_node.next
        
        while tree and tree.left:
            populate_children_next_field(tree)
            tree = tree.left
    ```
    - since the b-tree is perfect, for a node which is a left child, its right sibling is just its parent's right child
    - for a node which is a right child, its right sibling is its parent's right sibling's left child
    - process nodes level-by-level, left-to-right
    - when completing the level, the next level is the starting node's left child
    