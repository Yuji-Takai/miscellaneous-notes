### Example 1
**[Problem]** Implement an inorder traversal without recursion
- Input: root of a binary tree (nodes do NOT contain parent references)
- Output: array containing the result of the inorder traversal without recursion

1. O(n) time, O(h) space
    ```
    def inorder_traversal(tree):
        s, result = [], []
        while s or tree:
            s.append(tree)
            tree = tree.left
        else:
            tree = s.pop()
            result.append(tree.data)
            tree = tree.right
        return result
    ```
    - simulate the function call stack by using an explicit stack
    - push the current node and not its right child
    - not using a visited field

### Example 2
**[Problem]** Implement a preorder traversal without recursion
- Input: root of a binary tree (nodes do NOT contain parent references)
- Output: array containing the result of the preorder traversal without recursion

1. O(n) time, O(h) space
    ```
    def preorder_traversal(tree):
        path, result = [tree], []
        while path:
            curr = path.pop()
            if curr:
                result.append(curr.data)
                path += [curr.right, curr.left]
        return result
    ```
    - preorder traversal visits nodes in a last in, first out order == can perform preorder traversal using a stack of tree nodes
    - stack initialized to contain the root
    - visit a node by popping it, adding first its right child, and then its left child to the stack
        + add the left child after the right child, since want to continue with the left child
    

### Example 3
**[Problem]** Compute the *k*th node in an inorder traversal
- Input: root of a b-tree (each node stores the number of nodes in the subtree rooted at that node)
- Output: kth node appearing in an inorder traversal

1. O(h) time
    ```
    def find_kth_node_binary_tree(tree, k):
        while tree:
            left_size = tree.left.size if tree.left else 0
            if left_size + 1 < k: 
                k -= left_size + 1
                tree = tree.right
            elif left_size == k - 1:
                return tree
            else:
                tree = tree.left
        return None # if 1 <= k <= n, this is unreachable
    ```
    - use the additional info given
    - if the left subtree has *L* nodes, then the *k*th node in the original tree is the (*k* - *L*)th node when skip the left subtree
        + if k <= L, the desired node lies in the left subtree



### Example 4
**[Problem]** Compute the successor
- Input: Node (each node in the tree stores its parent)
- Output: Successor of the input node
    + **Successor**: the node that appears immediately after the given node in an inorder traversal
1. O(h) time
    ```
    def find_successor(node):
        if node.right:
            node = node.right
            while node.left:
                node = node.left
            return node
        while node.parent and node.parent.right is node:
            node = node.parent
        return node.parent
    ```
    - if the given node has a nonempty right subtree, its successor must lie in that subtree, and the rest of the nodes are immaterial
    - when a node has a nonempty right subtree, its successor is the first node visited when performing an inorder traversal on that subtree == left-most node in that subtree which can be computed by following left children exclusively, stopping when there is no left child to continue from
    - if the given node does not have a right subtree:
        + if the node is its parent's left child, the parent is the successor
        + if the node is its parent's right child, the next visited node can be determined by iteratively following parents, stopping when move up from a left child
    - if the given node is the last node visited in an inorder traversal then root is reached without ever moving up from a left child 

### Example 5
**[Problem]** Implement an inorder traversal with O(1) space
- Input: root of a binary tree (each node has parent fields)
- Output: array resulting from nonrecursive inorder traversal with O(1) space

1. O(n) time, O(1) space
    ```
    def inorder_traversal(tree):
        prev, result = None, []
        while tree:
            if prev is tree.parent:
                if tree.left:
                    next = tree.left
                else:
                    result.append(tree.data)
                    next = tree.right or tree.parent
            elif tree.left is prev:
                result.append(tree.data)
                next = tree.right or tree.parent
            else:
                next = tree.parent
            prev, tree = tree, next
        return result
    ```
    - since each node stores its parent, possible to achieve space complexity of O(1)
    - need to know when returning to a prent if the just completed subtree was the parent's left subtree or a right subtree
        1. record the subtree's root before moving to the parent
        2. compare the subtree's root with the parent's left child 
