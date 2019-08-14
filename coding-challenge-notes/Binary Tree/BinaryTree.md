# Array 
## Python
### Binary Tree
- **Binary tree**: 
    + either empty, or a *root* node *r* together with a left binary tree and a right binary tree
    + appropriate when dealing with *hierarchies*

- Prototype
    ```
    class BinaryTreeNode:
        def __init__(self, data=None, left=None, right=None):
            self.data = data
            self.left = left
            self.right = right
    ```
- Each node, except the root, is itself the root of a left subtree or a right subtree
- with the exception of the root, every node has a unique parent

### Definitions
- **Search path**: a unique sequence of nodes from the root to a node with each node in the sequence being a child of the previous node
- **ancestor**: a node that lies on the search path from the root to a node *d*
- **decendant**: if d1 is an ancestor of d2, then d2 is a descendant of d1
- node is an ancestor and descendant of itself
- **leaf**: a node that has no descendants except for itself
- **depth**: the number of nodes on the search path from the root to node *n*, not including *n* itself
- **height**: the maximum depth of any node in that tree
- **level**: all nodes at the same depth

### Traversals
- **Inorder**: traverse the left subtree, visit the root, then the right subtree
- **Preorder**: visit the root, traverse the left subtree, then traverse the right subtree
- **Postorder**: traverse the left subtree, traverse the right subtree, and then visit the root 
- Implemented recursively, traversals have O(n) time complexity (*n* number of nodes in tree)and O(h) additional space complexity (*h* height of the tree)
    + space complexity is dictated by the maximum depth of the function call stack
- if each node has a parent field, the traversals can be done with O(1) additional space complexity

- Implementation:
    ```
    def tree_traversal(root):
        if root:
            print('Preorder: %d' % root.data)
            tree_traversal(root.left)
            print('Inorder: %d' % root.data)
            tree_traversal(root.right)
            print('Postorder: %d' % root.data)
    ```
    + O(n) time complexity
    + O(h) space complexity
        - no memory is explicity allocated
        - BUT function call stack reaches a maximum depth of *h* (height of tree)
        - (complete B-Tree) *log n* <= *h* <= *n* (skewed tree)

### Types of Binary Tree
- **full binary tree**: binary tree in which every node other than leaves has 2 children
- **perfect binary tree**: 
    + full binary tree in which all leaves are at the same depth, and in which every parent has 2 children
    + a perfect binary tree of height *h* contains exactly 2<sup>h+1</sup> - 1 nodes, of which 2<sup>h</sup> are leaves
- **complete binary tree**: 
    + binary tree in which every level, except possibly the last, is completely filled, and all are as far left as possible 
    + A complete binary treee on *n* nodes has height `⌊log n⌋`
- **Left-skewed tree**: a tree in which no node has a right child
- **Right-skewed tree**: a tree in which no node has a left child

