### Example 1
**[Problem]** Reconstruct a binary tree from traversal data
- Input: inorder and preorder traversal sequences of a b-tree
- Output: tree reconstructed from the 2 sequences
    + Given an inorder traversal and one of preorder and postorder traversals, there exists a unique binary tree that yields those orders, assuming *each node holds a distinct key*

1. O(n) time, O(n + h) = O(n) space
    ```
    def binary_tree_from_preorder_inorder(preorder, inorder):
        node_to_inorder_idx = {data: i for i, data in enumerate(inorder)}
        def binary_tree_from_preorder_inorder_helper(preorder_start, preorder_end, inorder_start, inorder_end):
            if preorder_end <= preorder_start or inorder_end <= inordedr_start:
                return None
            root_inorder_indx = node_to_inorder_idx[preorder[preorder_start]]
            left_subtree_size = root_inorder_idx - inorder_start
            return BinaryTreeNode(preorder[preorder_start], 
                binary_tree_from_preorder_inorder_helper(
                    preorder_start + 1, preorder_start + 1 + left_subtree_size, inorder_start, root_inorder_idx
                ),
                binary_tree_from_preorder_inorder_helper(
                    preorder_start + 1 + left_subtree_size, preorder_end, root_inorder_idx + 1, inorder_end))
        return binary_tree_from_preorder_inorder_helper(0, len(preorder), 0, len(inorder))
    ```
    - in preorder sequence, root node is the first in the sequence
        + allows to split the inorder sequence into an inorder sequence for the left subtree, followed by the root, followed by the right subtree
    - can use the left subtree inorder sequence to compute the preorder sequence for the left subtree from the preorder sequence for the entire tree
        + preorder traversal sequence consists of the root, followed by the preorder traversal sequence of the left subtree followed by the preorder traversal sequence of the right subtree
    - the number *k* of nodes in the left subtree is can be found from the location of the root in the inorder traversal sequence
        + the subsequence of *k* nodes after the root in the preorder traversal sequence is the preorder traversal sequence for the left subtree
    - finding the root within the inorder sequences takes O(n) time 
        + improve the time complexity by initially building a hash table from keys to their positions in the inorder sequence

    - Example:
        + inorder: [F, B, A, E, H, C, D, I, G] & preorder: [H, B, F, E, A, C, D, G, I]
        1. root = H --> first node in preorder
        2. root's subtree = [F, B, A, E] --> all nodes to left of root in inorder
        3. [B, F, E, A] == preorder traversal of the 4 nodes in the root's left subtree
        4. similar construction for root's right subtree
        5. recurse till reach the leaves
    
### Example 2
**[Problem]** Reconstruct a binary tree from a preorder traversal with markers
- Input: preorder traversal sequences of a b-tree with `null` to mark empty children
- Output: b-tree reconstructed from the traversal 
    + example input: `[H, B, F, null, null, E, A, null, null, null, C, null, D, null, G, I, null, null, null`

1. O(n) time
    ```
    def reconstruct_preorder(preorder):
        def reconstruct_preorder_helper(preorder_iter):
            subtree_key = next(preorder_iter)
            if subtree_key is None:
                return None
            # note that reconstruct_preorder_helper updates preorder_iter so the order of following calls are critical
            left_subtree = reconstruct_preorder_helper(preorder_iter)
            right_subtree = reconstruct_preorder_helper(preorder_iter)
            return BinaryTreeNode(subtree_key, left_subtree, right_subtree)
        return reconstruct_preorder_helper(iter(preorder))
    ```
    - first node in the sequence is the root
    - the sequence for the root's left subtree appears before all the nodes in the root's right subtree
