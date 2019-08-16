# Binary Search Tree
## Python 
### BST
- stored values (keys) are stored in a sorted order
- BST: b-tree in which:
    + the nodes store keys that are comparable
    + the keys stored at nodes have to respect the BST property:
        - the key stored at a node is greater than or equal to the keys stored at the nodes of its left subtree and less than or equal to the keys stored at the nodes of its right subtree
    + key lookup, insertion, and deletion: 
        - take proportional time to the height of the tree
        - worst case O(n)
        - implementations that guarantee the tree to have height log(n) like **Red-black trees**  will have O(log n) operations
    + avoid putting mutable objects in a BST since the object that's present in a BST is not updated
        - when need to update mutable object, first remove it from the tree, then update it, and then add it back
- BST can find min and max elements and find the next largest and next smallest elements
- BST takes O(n) space
- with a BST can iterate through elements in sorted order in time O(n) (regardless of balanced)

### Prototype
```
class BSTNode:
    def __init__(self, data=None, left=None, right=None):
        self.data, self.left, self.right = data, left, right
```

### Recursion in BST
- Checking if a given value is present in a BST
- O(h) (h: height of tree)
```
def search_bst(tree, key):
    return (tree if not tree or tree.data == key else search_bst(tree.left, key) if key < tree.data else searcg_bst(tree.right, key))
```

### Augmented BSTs
- additional fields to the nodes other than key, left child, right child and possible parent
- speed up certain queries
- Example: need a datastructure with efficient insertion, deletion, lookup of interger keys and range queries (determine the number of keys that lie in an interval)
    1. O(h + m) time (h: height of tree, m: number of nodes with keys within the interval)
        - use BST
        - search for the first node with a key greater than or equal to the lower bound, and then call successor operation until reach a node whose key is greater than the upper bound
    2. O(h) time
        - use an augmented BST with a size field (number of nodes in the BST rooted at that node) in each node
        - simplified version: count all entries less than *v*
            1. intialize count to 0
            2. search for the first occurrence of v in an inorder traversal (since there can be duplicate keys in the tree)
                - if *v* is not present, stop when determined this
                1. each time take left child, leave count unchanged
                2. each time take right child, add one plus the size of the corresponding left child
                3. if *v* is present, and when first occurrence of *v* is reached, add the size of *v*'s left child
        - same approach for counting all entries greater than *v* case
        - O(h) time because the search always descends the tree
            + if m is large, this approach is much faster than repeating calling successor
        - find the number of keys in the interval [U, L]:
            1. compute number of keys < U
            2. compute number of keys > L
            3. subtract the two numbers from total number of nodes
        - size field can be updated on insert and delete without changing the O(h) time complexity of both operations
            + only nodes whose size field change are those on the search path to the added/deleted node
        