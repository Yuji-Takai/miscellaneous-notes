### Example 1
**[Problem]** Enumerate numbers of the form a + b sqrt(2)
- Input: nonnegative integers a and b
- Output: k smallest numbers of the form a + b sqrt(2)

1. O(k log k) time, O(k) space
    ```
    class Number:
        def __init__(self, a, b):
            self.a, self.b = a, b
            self.val = a + b * sqrt(2)
        def __lt__(self, other):
            return self.val < other.val
        def __eq__(self, other):
            return self.val == other.val
    
    def generate_first_k_a_b_sqrt2(k):
        candidates = bintrees.RBTree([Number(0, 0), None])
        result = []
        while len(result) < k:
            next_smallest = candidates.pop_min()[0]
            result.append(next_smallest.val)
            candidates[Number(next_smallest.a + 1, next_smallest.b)] = None
            candidates[Number(next_smallest.a, next_smallest.b + 1)] = None
        return result
    ```
    - since sqrt(2) is irrational --> if x + y sqrt(2) = x' + y' sqrt(2) --> x = x' & y = y'
    1. maintain a collection of real numbers, initialized to 0 + 0 sqrt(2)
    2. perform k extractions of the smallest element (a + b sqrt(2)), followed by insertion of (a + 1) + b sqrt(2) and a + (b + 1) sqrt(2)
        - the smallest number is 0 + 0 sqrt(2)
        - candidates for next smallest number are 1 + 0 sqrt(2) and 0 + 1 sqrt(2)
    - since it is possible that the same number may be inserted more than once, need to ensure the collection does not create duplicates when the same item is inserted twice

2. O(n) time
    ```
    def generate_first_k_a_b_sqrt2(k):
        cand = [Number(0, 0)]
        i = j = 0
        for _ in range(1, k):
            cand_i_plus_1 = Number(cand[i].a + 1, cand[i].b)
            cand_j_plus_sqrt2 = Number(cand[j].a, cand[i].b + 1)
            cand.append(min(cand_i_plus_1, cand_j_plus_sqrt2))
            if cand_i_plus_1.val == cand[-1].val:
                i += 1
            if cand_j_plus_sqrt2.val == cand[-1].val:
                j += 1
        return [a.val for a in cand]
    ```
    - (n + 1)th value will be the sum of 1 or sqrt(2) with a previous value
    - computing (n + 1)th value:
        1. store the result in an array A
        2. keep track of just 2 entries:
            1. i: the smallest index such that A[i] + 1 > A[n - 1]
            2. j: the smallest index such that A[j] + sqrt(2) > A[n - 1]
        3. (n + 1)th entry will be the smaller of A[i] + 1 and A[j] + sqrt(2)
        4. after obtaining the (n + 1)th entry:
            1. if it is A[i] + 1, increment i
            2. if it is A[j] + sqrt(2), increment j
            3. if A[i] + 1 == A[j] + sqrt(2), imcrement both i and j

### Example 2
**[Problem]** The range lookup problem
- Input: a BST and an interval
- Output: the BST keys that lie in the interval

1. O(m + h) time
    ```
    Interval = collections.namedtuple('Interval', ('left', 'right'))

    def range_lookup_in_bst(tree, interval):
        def range_lookup_in_bst_helper(tree):
            if tree is None:
                return
            if interval.left <= tree.data <= interval.right:
                range_lookup_in_bst_helper(tree.left)
                result.append(tree.data)
                range_lookup_in_bst_helper(tree.right)
            elif interval.left > tree.data:
                range_lookup_in_bst_helper(tree.right)
            else:
                range_lookup_in_bst_helper(tree.left)
        
        result = []
        range_lookup_in_bst_helper(tree)
        return result
    ```
    - use BST property to prune the traversal:
        1. if the root of the tree holds a key that is less than the left endpoint of the interval, the left subtree cannot contain any node whose key lies in the interval
        2. if the root of the tree holds a key that is more than the right endpoint of the interval, the right subtree cannot contain any node whose key lies in the interval
        3. otherwise, the root of the tree holds a key that lies within the interval, and it is possible for both the left and right subtrees to contain nodes whose keys lie in the interval
