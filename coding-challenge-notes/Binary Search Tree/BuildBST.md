### Example 1
**[Problem]** Reconstruct a BST from traversal data
- Input: preorder traversal of a BST (all keys are distinct)
- Output: BST reconstructed from the traversal sequence
    + [NOTE] sequence provided by inorder traversal is not sufficient to reconstruct the tree
    + [NOTE] algo for reconstructing from postorder traversal is similar 

1. O(n<sup>2</sup>) time (worst == left skewed == O(n<sup>2</sup>, best == right skewed == O(n), balanced == O(n log n)))
    ```
    def rebuild_bst_from_preorder(preorder_sequence):
        if not preoder_sequence:
            return None
        transition_point = next((i for i, a in enumerate(preorder_sequence) if a > preorder_sequence[0]), len(preorder_sequence))
        return BstNode(preorder_sequence[0], 
                rebuild_bst_from_preorder(preorder_sequence[1:transition_point]), 
                rebuild_bst_from_preorder(preorder_sequence[transition_point:]))
    ```

    - in any preorder traversal sequence, first key corresponds to the root
    - the subsequence which begins at the second element and ends at the last key less than the root, corresponds to the preorder traversal of the root's left subtree
    - the final subsequence consisting of keys greater than the root corresponds to the preorder traversal of the root's right subtree
    - recursively reconstruct the left and right subtrees from the 2 subsequences then add them to the root

2. O(n) time
    ```
    def rebuild_bst_from_preorder(preorder_sequence):
        def rebuild_bst_from_preorder_on_value_range(lower_bound, upper_bound):
            if root_idx[0] == len(preorder_sequence):
                return None
            root = preorder_sequence[root_idx[0]]
            if not lower_bound <= root <= upper_bound:
                return None
            root_idx += 1
            left_subtree = rebuild_bst_from_preorder_on_value_range(lower_bound, root)
            right_subtree = rebuild_bst_from_preorder_on_value_range(root, upper_bound)
            return BstNode(root, left_subtree, right_subtree)
        
        root_idx = [0]
        return rebuild_bst_from_preorder_on_value_range(float('-inf'), float('inf'))
    ```
    - reconstruct the left subtree in the same iteration as identifying the nodes which lie in it
    - do not want to iterate from first entry after the root to the last entry smaller than the root, only to go back and partially repeat this process for the root's left subtree
    - can avoid repeated passes over nodes by including the range of keys wanted to reconstruct the subtrees over

### Example 2 
**[Problem]** Build a minimum height BST from a sorted array
- Input: sorted array
- Output: BST of minimum possible height

1. O(n) time
    ```
    def build_min_height_bst_from_sorted_array(A):
        def build_min_height_bst_from_sorted_subarray(start, end):
            if start >= end:
                return None
            mid = start + (end - start) // 2
            return BstNode(A[mid], build_min_height_bst_from_sorted_subarray(start, mid), build_min_height_bst_from_sorted_subarray(mid + 1, end))
        
        return build_min_height_bst_from_sorted_subarray(0, len(A))
    ```
    - to make minimum height BST, need the subtrees to be as balanced as possible
        + balanced can be achieved by keeping the number of nodes in both subtrees as close as possible
    - let n = length of the array
        1. make the element in the middle of the array (n//2) the root to achieve optimum balance
        2. recursively compute minimum height BSTs for the subarrays on either side of this entry 
    
