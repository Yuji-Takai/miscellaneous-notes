### Example 1
**[Problem]** Find the first key greater than a given value in a BST
- Input: a BST and a value
- Output: first key that would appear in an inorder traversal which is greater than the input value

1. O(h) time, O(1) space
    ```
    def find_first_greater_than_k(tree, k):
        subtree, first_so_far = tree, None
        while subtree:
            if subtree.data > k:
                first_so_far, subtree = subtree, subtree.left
            else:
                subtree = subtree.right
        return first_so_far
    ```
    - store the best candidate for the result and update that candidate as iteratively descend the tree, eliminating subtrees by comparing the keys stored at nodes with the input value
        + if the current subtree's root stores a key less than or equal to the input value, search the right subtree
        + if the current subtree's root stores a key greater than the input value, search in the left subtree, updating the candidate to the current root

### Example 2
**[Problem]** Find the k largest elements in a BST
- Input: a BST and an integer k
- Output: kth largest elements in the BST in decreasing order

1. O(h + k) time
    ```
    def find_k_largest_in_bst(tree, k):
        def find_k_largest_in_bst_helper(tree):
            if tree and len(k_largest_elements) < k:
                find_k_largest_in_bst_helper(tree.right)
                if len(k_largest_elements) < k:
                    k_largest_elements.append(tree.data)
                    find_k_largest_in_bst_helper(tree.left)
        k_largest_elements = []
        find_k_largest_in_bst_helper(tree)
        return k_largest_elements
    ```
    - reverse inorder traversal
        + begin with the desired nodes, and work backwards
        + recurse first on the right subtree and then on the left subtree
        + as soon as k nodes are visited, halt
        + since the prob states decreasing order, appending the newer nodes is fine

### Example 3
**[Problem]** Compute the LCA in a BST
- Input: a BST (all keys are distinct, nodes do NOT have references to their parents) and 2 nodes 
- Output: LCA of the 2 nodes

1. O(h) time (h: height of the tree)
    ```
    def find_LCA(tree, s, b):
        while tree.data < s.data or tree.data > b.data:
            while tree.data < s.data:
                tree = tree.right
            while tree.data > b.data:
                tree = tree.left
        return tree
    ```
    - let s (smaller key) and b the 2 nodes whose LCA will be computed 
        1. if the root's key is the same as that stored at s or at b, done --> root is the LCA
        2. if the key at s is smaller than the key at the root, and the key at b is greater than the key at the root --> root is the LCA
        3. if both keys are smaller than that at the root --> LCA must lie in the left subtree
        4. if both keys are larger than that at the root --> LCA must lie in the right subtree

### Example 4
**[Problem]** Find the closest entries in 3 sorted arrays
- Input: 3 sorted arrays
- Output: one entry from each such that the minimum interval containing these three entries is as small as possible
    + [EXAMPLE] `[5, 10, 15], [3, 6, 9, 12, 15], [8, 16, 24] --> [15, 15, 16]`

1. O(n log k): (n: total number of elements in the k arrays)
    ```
    def find_closest_elements_in_sorted_arrays(sorted_arrays):
        min_distance_so_far = float('inf')
        iters = bintrees.RBTree()
        for idx, sorted_array in enumerate(sorted_arrays):
            it = iter(sorted_array)
            first_min = next(it, None)
            if first_min is not None:
                iters.insert((first_min, idx), it)
        
        while True:
            min_value, min_idx = iters.min_key()
            max_value = iters.max_key()[0]
            min_distance_so_far = min(max_value - min_value, min_distance_so_far)
            it = iters.pop_min()[1]
            next_min = next(it, None)
            if next_min is None:
                return min_distance_so_far
            iters.insert((next_min, min_indx), it)
    ```
    1. begin with the triple consisting of the smallest entries in each array
        + s: min value in the triple, t: max value in the triple
        + smallest interval = [s, t]
    2. remove s from the triple and bring the next smallest element from the array it belongs to into the triple
        + s': new min value in the triple, t': new max value in the triple
        + smallest interval = [s', t']
    3. by iteratively examining and removing the smallest element from the triple. compute the minimum interval starting at that element
        + since the minimum interval containing elements from each array must being with the element of some array, guaranteed to encounter the minimum element
