### Example 1
**[Problem]** Test if a B-tree satisfies the BST property
- Input: binary tree
- Output: true if the binary tree satisfies the BST property

1. O(n) time, O(h) space (h: height of tree)
    ```
    def is_binary_tree_bst(tree, low_range=float('-inf'), high_range=float('inf')):
        if not tree:
            return True
        elif not low_range <= tree.data <= high_range:
            return False
        return (is_binary_tree_bst(tree.left, low_range, tree.data) 
                and is_binary_tree_bst(tree.right, tree.data, high_range))
    ```
    - checking constraints on the values for each subtree
    - if all nodes in a tree must have keys in the range [l, u] , and the key at the root is w, then all keys in the left subtree must be in the range [l, w], and all keys stored in the right subtree must be in the range [w, u]


2. O(n) time, O(n) space
    ```
    def is_binary_tree_bst(tree):
        QueueEntry = collections.namedtuple('QueueEntry', ('node', 'lower', 'upper'))

        bfs_queue = collections.deque([QueryEntry(tree, float('-inf'), float('inf'))])

        while bfs_queue:
            front = bfs_queue.popleft()
            if front.node:
                if not front.lower <= front.node.data <= front.upper:
                    return False
                bfs_queue += [QueueEntry(front.node.left, front.lower, front.node.data),
                                QueueEntry(front.node.right, front.node.data, front.upper)]
        return True
    ```
    - search violation in BFS manner to reduce the complexity when the property is violated at a node whose depth is small
        + use queue, where ech queue entry contains a node, an upper and a lower bound on the keys stored at the subtree rooted at that node
        + queue initialized to the root with lower bound -inf and upper bound inf
        + iteratively check the constraint on each node
            - if it violates the constraint, stop
            - otherwise, add its children along with the corresponding constraints

### Example 2
**[Problem]** Test if 3 BST nodes are totally ordered
- Input: 2 nodes in a BST and a third node ("middle" node)
- Output: true if one of the two nodes is a proper ancestor and the other a proper descedant of the middle
    + **Proper ancestor**: an ancestor that is not equal to the node
    + **Proper decendant**: a descendant that is not equal to the node

1. O(h) time
    ```
    def pair_includes_ancestor_and_descedant_of_m(possible_anc_or_desc_0, possible_anc_or_desc_1, middle):
        search_0, search_1 = possible_anc_or_desc_0, possible_anc_or_desc_1

        while (search_0 is not possible_anc_or_desc_1 
                and search_0 is not middle 
                and search_1 is not possible_anc_or_desc_0 
                and search_1 is not middle
                and (search_0 or search_1)):
            if search_0:
                search_0 = (search_0.left if search_0.data > middle.data else search_0.right)
            if search_1:
                search_1 = (search_1.left if search_1.data > middle.data else search_1.right)
        
        if ((search_0 is not middle and search_1 is not middle) or search_0 is possible_anc_or_desc_1 or search_1 is possible_anc_or_desc_0):
            return False
        
        def search_target(source, target):
            while source and source is not target:
                source = source.left if source.data > target.data else source.right
            return source is target
        
        return search_target(middle, possible_anc_or_desc_1 if search_0 is middle else possible_anc_or_desc_0)
    ```
    - perform the searches for the middle from both alternatives (assuming first node to be proper ancestor, second to be proper descendant and vice versa) in an interleaved fashion
    - if encounter the middle from one node, subsequently search for the second node from the middle
        + avoid performing an unsucessful search on a large subtree
    - when the middle node does have an ancestor and descendant pair == O(d) time (d: difference between the depths of the ancestor and descendant)
    - worst case O(h): middle node does not have an ancestor and descendant 

