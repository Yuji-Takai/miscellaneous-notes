### Example 1 
**[Problem]** Test if a binary tree is height-balanced
- Input: root of a b-tree
- Output: true if the tree is height-balanced otherwise false
    + **height-balanced**: if for each node in the tree, the difference in the height of its left and right subtrees is at most one
    + *Perfect B-tree* and *Complete B-tree* are *height-balanced* but a *height-balanced* B-tree does NOT have to be perfect or complete

1. O(n) time, O(h) space
    ```
    def is_balanced_binary_tree(tree):
        BalancedStatusWithHeight = collections.namedtuple('BalancedStatusWithHeight', ('balanced', 'height'))
        def check_balanced(tree):
            if not tree:
                return BalancedStatusWithHeight(True, -1)
            left_result = check_balanced(tree.left)
            if not left_result.balanced:
                return BalancedStatusWithHeight(False, 0)
            right_result = check_balanced(tree.right)
            if not right_result.balanced:
                return BalancedStatusWithHeight(False, 0)
            is_balanced = abs(left_result.height - right_result.height) <= 1
            height = max(left_result.height, right_result.height) + 1
            return BalancedStatusWithHeight(is_balanced, height)
        return check_balance(tree).balanced
    ```
    - No need to store the heights of all nodes at the same time (which requires O(n) space)
    - Once done with a subtree, just need to know whether it is height-balanced, and if so, what its height is
    - No need of any information about the descendants of the subtree's root
    - Implementation of postorder traversal with some calls possibly being eliminated because of early termination
        + If any left subtree is not height-balanced, do not need to visit the corresponding right subtree
    - O(h) space
        + function call stack corresponds to a sequence of calls from the root through the unique path to the current node
        + stack height is therefore bounded by the height of tree
    - O(n) time (same as postorder traversal)


### Example 2 
**[Problem]** Test if a binary tree is symmetric
- Input: root of a b-tree
- Output: true if the tree is symmetric otherwise false
    + **Symmetric**: if the left subtree is a mirror image of the right subtree when
    + not just structurally symmetric, requires that corresponding nodes have the same keys

1. O(n) time, O(h) space
    ```
    def is_symmetric(tree):
        def check_symmetric(subtree_0, subtree_1):
            if not subtree_0 and not subtree_1:
                return True
            elif subtree_0 and subtree_1:
                return (subtree_0.data == subtree_1.data and check_symmetric(subtree_0.left, subtree_1.right) and check_symmetric(subtree_0.right, subtree_1.left))
            return False
        return not tree or check_symmetric(tree.left, tree.right)
    ```
    - no need to construct the mirrored subtrees
    - just need to check whether a pair of subtrees are mirror images
    - as soon as a pair fails the test, can short circuit the check to false