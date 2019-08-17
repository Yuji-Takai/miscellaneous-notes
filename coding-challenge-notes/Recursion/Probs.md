### Example 1
**[Problem]** The Towers of Hanoi problem 
- Input: int n
- Output: sequence of operations that result in the transfer of n rings from one pef to another 
    + each operation should be encoded as a pair (from, to)
    + a peg contains rings in sorted order, with the largest ring being the lowest
    + transfer these rings to another peg, which is initially empty

1. O(2<sup>n</sup>) time
    ```
    NUM_PEGS = 3

    def compute_tower_hanoi(num_rings):
        def compute_tower_hanoi_steps(num_rings_to_move, from_peg, to_peg, use_peg):
            if num_rings_to_move > 0:
                compute_tower_hanoi_steps(num_rings_to_move - 1, from_peg, to_peg, use_peg)
                pegs[to_peg].append(pegs[from_peg].pop())
                result.append([from_peg, to_peg])
                compute_tower_hanoi_steps(num_rings_to_move - 1, use_peg, to_peg, from_peg)
        
        result = []
        pegs = [list(reversed(range(1, num_rings + 1)))] + [[] for _ in range(1, NUM_PEGS)]
        compute_tower_hanoi_steps(num_rings, 0, 1, 2)
        return result
    ```
    - Recurrence T(n) = T(n - 1) + 1 + T(n - 1) --> O(2<sup>n</sup>)


### Example 2
**[Problem]** Generate all nonattacking placements of n-Queens 
- Input: int n
- Output: all distinct nonattacking placements of n queens on an n x n chessboard
    + Nonattacking placement of queens: no 2 queens are in the same row, column or diagonal

1. O(???) time
    ```
    def n_queens(n):
        def solve_n_queens(row):
            if row == n:
                result.append(list(col_placement))
                return
            for col in range(n):
                if all(abs(c - col) not in (0, row - i) for i, c in enumerate(col_placement[:row])):
                    col_placement[row] = col
                    solve_n_queens(row + 1)
            result, col_placement = [], [0] * n
            solve_n_queens(0)
            return result
    ```
    - enumerate placements that use distinct rows (since no 2 queens can be on the same row)
        + such a placement cannot lead to conflicts on rows, but it may lead to conflicts on columns and diagonals
        + placement can be represented by an array of length n, where the ith entry is the location of the queen on Row i
        + Example: n = 4
            1. start with first row at column 0 --> [0, _, _, _]
            2. second row --> cannot be column 0 or 1 --> either column 2 or 3 --> [0, 2, _, _] or [0, 3, _, _]
            3. third row --> cannot be column 0 or 1 or 3 when second row is 2 & cannot be column 0 or 2 or 3 when second row is 3 --> [0, 3, 1, _]
            4. fourth row --> cannot put column 2 because violation --> no placement possible with a queen plaed at Row 0 and Column 0
            5. [1, 3, 0, 2] and [2, 0, 3, 1] are the only nonattacking placements

### Example 3
**[Problem]** Generate permutations 
- Input: array of distinct integers
- Output: all permutations of that array

1. O(n x n!) time
    ```
    def permutations(A):
        def directed_permutations(i):
            if i == len(A) - 1:
                result.append(A.copy())
            for j in range(i, len(A)):
                A[i], A[j] = A[j], A[i]
                directed_permutations(i + 1)
                A[i], A[j] = A[j], A[i]
        
        result = []
        directed_permutations(0)
        return result
    ```
    - once a value has been chosen for an entry, do not want to repeat it
        + every permutation of A begins with one of A[0], A[1], ..., A[n - 1]
        + generate all permutations that begin with A[0], then all permutations that begin with A[1], ...
        + computing all permutations that begin with A[0] == computing all permutations of A[1, n - 1] ==> recursion

2. O(n x n!) time
    ```
    def permutations(A):
        result = []
        while True:
            result.append(A.copy())
            A = next_permutation(A)
            if not A:
                break
        return result
    ```
    - n distinct entries in the array can be mapped to 1, 2, 3, ... with 1 corresponding to the smallest entry
    - keep on computing the next permutations
    - [EXAMPLE] [7, 3, 5] --> first sort to be [3, 5, 7] --> next is [3, 7, 5] --> [5, 3, 7] --> [5, 7, 3] --> [7, 3, 5] --> [7, 5, 3]

### Example 4
**[Problem]** Generate powerset 
- Input: a set
- Output: its power set
    + **Power set**: set of all subsets of set S, includin both the empty set and S itself

1. O(n2<sup>n</sup>) time, O(n2<sup>n</sup>) space 
    ```
    def generate_power_set(input_set):
        def directed_power_set(to_be_selected, selected_so_far):
            if to_be_selected == len(input_set):
                power_set.append(list(selected_so_far))
                return
            directed_power_set(to_be_selected + 1, selected_so_far)
            directed_power_set(to_be_selected + 1, selected_so_far + [input_set[to_be_selected]])
        power_set = []
        directed_power_set(0, [])
        return power_set
    ```
    - compute all subsets *U* that do not include a particular element (could be any single element)
    - then compute all subsets *V* that do include that element
    - each subset must appear in *U* or in *V*, so the final result is just *U* U *V*

2. O(n2<sup>n</sup>)
    ```
    def generate_power_set(S):
        power_set = []
        for int_for_subset in range(1 << len(S)):
            bit_array = int_for_subset
            subset = []
            while bit_array:
                subset.append(int(math.log2(bit_array & ~(bit_array - 1))))
                bit_array &= bit_array - 1
            power_set.append(subset)
        return power_set
    ```
    - for a given ordering of the elements of S, there exists a 1-to-1 correspondence between the 2<sup>n</sup> bit arrays of length n and the set of all subsets of S -- the 1s in the n-length bit array *v* indicate the elements of S in the subset corresponding to *v*
        + [Example] [a, b, c, d] & [1, 0, 1, 1] --> [a, c, d]
        + derive a nonrecursive algorithm for enumerating subsets
    - when n is less than or equal to teh width of an integer on the architecture working on, can enumerate bit arrays by enumerating integers in [0, 2<sup>n</sup> - 1] and examining the indices of bits set in these integers
        + these indices are determined by first isolating the lowest set bit by computing y = x & ~(x - 1), and then getting the index by computing log y
    

### Example 5 
**[Problem]** Generate all subsets of size k
- Input: ints n and k
- Output: all size k subsets of {1, 2, ..., n}
1. O(n (k C n)) time
    ```
    def combinations(n, k):
        def directed_combinations(offset, partial_combination):
            if len(partial_combination) == k:
                result.append(list(partial_combination))
                return
            
            num_remaining = k - len(partial_combination)
            i = offset
            while i <= n and num_remaining <= n - i + 1:
                directed_combinations(i + 1, partial_combination + [i])
                i += 1
        result = []
        directed_combinations(1, [])
        return result
    ```
    - 2 possibilities for a subset
        1. it does not contain 1 --> return all subsets of size k of {2, 3, ..., n}
        2. it does contain 1 --> compute all k - 1 sized subsets of {2, 3, ..., n} and add 1 to each of them
    

### Example 6
**[Problem]** Generate strings of matched parens 
- Input: int k
- Output: all the strings with that number of matched pairs of parens
    + Strings in which parens are matched:
        1. the empty string, "", is a string in which parens are matched
        2. the addition of a leading left parens and a trailing right parens to a string in which parens are matched results in a string in which parens are matched 
            - "(())()" is a matched paren --> "((())())" is as well
        3. the concatenation of 2 strings in which parens are matched is itself a string in which parens are matched
            - "(())()" + "()" --> "(())()()" is also matched parens
    
1. O((2k)!/(k!(k + 1)!)) time
    ```
    def generate_balanced_parentheses(num_pairs):
        def directed_generate_balanced_parentheses(num_left_parens_needed, num_right_parens_needed, valid_prefix, result=[]):
            if num_left_parens_needed > 0:
                directed_generate_balanced_parentheses(num_left_parens_needed - 1, num_right_parens_needed, valid_prefix + '(')
            if num_left_parens_needed < num_right_parens_needed:
                directed_generate_balanced_parentheses(num_left_parens_needed, num_right_parens_needed - 1, valid_prefix + ')')
            if not num_right_parens_needed:
                result.append(valid_prefix)
            return result
        return directed_generate_balanced_parentheses(num_pairs, num_pairs, '')
    ```
    - some strings can never be completed to a string with k pairs of matched parens --> ex: string beginning with )
    - build strings incrementally
        + ensure that as each additional character is added, the resulting string has the potential to be completed to a string with k pairs of matched parens
    - suppose have a string whose length < 2k and know that string can be completed to a string with k pairs of matched parens --> 2 possibilities to extend that string: add a left parens or a right parens
        1. if add a left parens, and still want to complete the string to a string with k pairs of matched parens --> the number of left parens needed is greater than 0
        2. if add a right parens, and still want to complete the string to a string with k pairs of matched parens --> the number of left parens needed must be less than the number of right parens
    
### Example 7
**[Problem]** Generate palindromic decompositions 
- Input: str
- Output: all palindromic decompositions of a given string
    + **Palindromic string**: reads the same backwards and forwards
    + **Decomposition of a string**: a set of strings whose concatenation is the string
    + [Example] `"0204451881" --> "020", "44", "5", "1881"`

1. O(n2<sup>n</sup>)
    ```
    def palindrome_decompositions(input):
        def directed_palindrome_decompositions(offset, partial_partition):
            if offset == len(input):
                result.append(list(partial_partition))
                return
            
            for i in range(offset + 1, len(input) + 1):
                prefix = input[offset:i]
                if prefix == prefix[::-1]:
                    directed_palindrome_decompositions(i, partial_partition + [prefix])
        result = []
        directed_palindrome_decompositions(0, [])
        return result
    ```
    - recursively compute palindromic sequences 

2. O(n2<sup>n</sup>) - Python
    ```
    def palindrome_decompositions(text):
        return ([[text[:i]] + right for i in range(1, len(text) + 1) if text[:i] == text[:i][::-1] for right in palindrome_decompositions(text[i:])] or [[]])
    ```

### Example 8
**[Problem]** Generate binary trees 
- Input: int 
- Output: all distinct binary trees with a specified number of nodes

1. O((2n)!/(n!(n+1)!)) 
    ```
    def generate_all_binary_trees(num_nodes):
        if num_nodes == 0:
            return [None]
        result = []
        for num_left_tree_nodes in range(num_nodes):
            num_right_tree_nodes = num_nodes - 1 - num_left_tree_nodes
            left_subtrees = generate_all_binary_trees(num_left_tree_nodes)
            right_subtrees = generate_all_binary_trees(num_right_tree_nodes)
            result += [BinaryTreeNode(0, left, right) for left in left_subtrees for right in right_subtrees]
        return result
    ```
    - if the left child has k nodes, only use right child with n - 1 - k nodes, to get binary trees with n nodes that have that left child
        + get all binary trees on n nodes by getting all left subtrees on i nodes, and right subtrees on n - 1 - i nodes, for i between 0 and n - 1
    
### Example 9
**[Problem]** Implement a Sudoku solver 
- Input: partial assignment of sudoku
- Output: complete assignment

1. O(exponential) --> if generalized to n x n grids
    ```
    def solve_sudoku(partial_assignment):
        def solve_partial_sudoku(i, j):
            if i == len(partial_assignment):
                i = 0
                j += 1
                if j == len(partial_assignment):
                    return True
            if partial_assignment[i][j] != EMPTY_ENTRY:
                return solve_partial_sudoku(i + 1, j)
            
            def valid_to_add(i, j, val):
                if any(val == partial_assignment[k][j] for k in range(len(partial_assignment))):
                    return False
                if val in partial_assignment[i]
                    return False
                region_size = int(math.sqrt(len(partial_assignment)))
                I = i // region_size
                J = j // region_size
                return not any(val == partial_assignment[region_size * I + a][region_size + j + b] for a, b in itertools.product(range(region_size), repeat=2))
            
            for val in range(1, len(partial_assignment) + 1):
                if valid_to_add(i, j, val):
                    partial_assignment[i][j] = val
                    if solve_partial_sudoku(i + 1, j):
                        return True
            
            partial_assignment[i][j] = EMPTY_ENTRY # undo assignment
            return False
        
        EMPTY_ENTRY = 0
        return solve_partial_sudoku(0, 0)
    ```
    - traverse the 2D array entries one at a time
        1. if the entry is empty, try each value for the entry, and see if the updated 2D array is still valid; if it is, recurse --> if all the entries have been filled, the search is successful
        2. no need to test validity of the entire entry (too costly)
            - use that fact that adding a value to an array that already satisfies the constraints == need to check just the row, column, and subgrid of the added entry

### Example 10
**[Problem]** Compute a Gray code
- Input: int n
- Output: n-bit Gray code
    + **n-bit Gray code**: a permutation of {0, 1, 2, ..., 2<sup>n</sup> - 1} such that the binary representations of successive integers in the sequence differ in only one place

1. O(2<sup>n</sup>)
    ```
    def gray_code(num_bits):
        def directed_gray_code(history):
            def differs_by_one_bit(x, y):
                bit_difference = x ^ y
                return bit_difference and not (bit_difference & (bit_difference - 1))
            
            if len(result) == 1 << num_bits:
                return differs_by_one_bit(result[0], result[-1])
            
            for i in range(num_bits):
                previous_code = result[-1]
                candidate_next_code = previous_code ^ (1 << i)
                if candidate_next_code not in history:
                    history.add(candidate_next_code)
                    result.append(candidate_next_code)
                    if directed_gray_code(history):
                        return True
            return False
        
        result = [0]
        directed_gray_code(set([0]))
        return result
    ```
    - build the sequence incrementally, adding a value only if it is distinct from all values currently in the sequence, and differs in exactly one place with the previous value (for the last value, have to check that it differs in one place from the first value)

2. O(2<sup>n</sup>)
    ```
    def gray_code(num_bits):
        if num_bits == 0:
            return [0]
        gray_code_num_bits_minus_1 = gray_code(num_bits - 1)
        leading_bit_one = 1 << (num_bits - 1)
        return gray_code_num_bits_minus_1 + [leading_bit_one | i for i in reversed(gray_code_num_bits_minus_1)]
    ```

    ```
    def gray_code(num_bits):
        result = [0]
        for i in range(num_bits):
            result += [x + 2 ** i for x in reversed(result)]
        return result
    ```
    - from n bit Gray codes to n + 1 bit Gray code, prepending 0 to the n-bit Gray codes and prepending 1 to the reverse of the n bit Gray codes and concatenating the 2 will result in n + 1 bit Gray code, since Gray codes differ in one place on wrapping around
