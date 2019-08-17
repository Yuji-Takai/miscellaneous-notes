### Example 1 
**[Problem]** Fibonacci number 
- Input: int n
- Output: nth fibonacci number
    + F(n) = F(n - 1) + F(n - 2), F(0) = 0, F(1) = 1
1. using recursion --> exponential time due to repeatedly calling some F(i)s
2. O(n) time, O(n) space - DP
    ```
    def fibonacci(n, cache={}):
        if n <= 1:
            return n
        elif n not in cache:
            cache[n] = fibonacci(n - 1) + fibonacci(n - 2)
        return cache[n]
    ```

3. O(n) time, O(1) space - DP
    ```
    def fibonacci(n):
        if n <= 1:
            return n
        f_minus_2, f_minus_1 = 0, 1
        for _ in range(1, n):
            f = f_minus_2 + f_minus_1
            f_minus_2, f_minus_1 = f_minus_1, f
        return f_minus_1
    ```
    - iteratively fill the cache in a bottom-up fashion
        + allows to reuse cache storage to reduce the space complexity of the cache

### Example 2 
**[Problem]** Find the maximum sum over all subarrays 
- Input: array of integers
- Output: maximum sum over all subarrays of the array

1. O(n) time, O(1) space
    ```
    def find_maximum_subarray(A):
        min_sum = max_sum = 0
        for running_sum in itertools.accumulate(A):
            min_sum = min(min_sum, running_sum)
            max_sum = max(max_sum, running_sum - min_sum)
        return max_sum
    ```
    - in order to solve the problem for A[0, n - 1], need to know the subarray amonst all subarrays A[0,i], i < n - 1, with the smallest subarray sum --> desired value is S[n - 1] minus this subarray's sum 
        + for each index j, the maximum subarray sum ending at j is equal to S[j] - min<sub>k<=j</sub>S[k]
        + during the iteration, track the minimum S[k] seen so far and compute the maximum subarray sum for each index

### Example 3
**[Problem]** Count the number of score combinations
- Input: final score (int) and scores for individual plays (ints)
- Output: number of combinations of plays that result in the final score

1. O(sn) time, O(sn) space (s: number of individual play scores)
    ```
    def num_combinations_for_final_score(final_score, individual_play_scores):
        num_combinations_for_score = [[1] + [0] * final_score for _ in individual_play_scores]
        for i in range(len(individual_play_scores)):
            for j in range(1, final_score + 1):
                without_this_play = (num_combinations_for_score[i - 1][j] if i >= 1 else 0)
                with_this_play = (num_combinations_for_score[i][j - individual_play_scores[i]] if j >= individual_play_scores[i] else 0)
                num_combinations_for_score = (without_this_play + with_this_play)
        return num_combinations_for_score[-1][-1]
    ```
    - A[i][j]: 2D array storing the number of score combinations that result in a total of j, using individual plays of scores W[0], W[1], ..., W[i - 1]
        + A[i + 1][j] = A[i][j] + sum(k:0 to i)A[k][j - W[i]]

### Example 4
**[Problem]** Compute the Levenshtein distance
- Input: 2 strings
- Output: minimum number of edits needed to transform the first string into the second one
    + **Levenshtein Distance**: minimum number of edits (1 edit == insertion, deletion, substitution of a single character) it would take to transform the misspelled word into a correct word 

1. O(ab) time, O(ab) space (a: length of string A, b: length of string B)
    ```
    def levenshtein_distance(A, B):
        def compute_distance_between_prefixes(A_idx, B_idx):
            if A_idx < 0:
                return B_idx + 1
            elif B_idx < 0:
                return A_idx + 1
            if distance_between_prefixes[A_idx][B_idx] == -1:
                if A[A_idx] == B[B_idx]:
                    distance_between_prefixes[A_idx][B_idx] = (compute_distance_between_prefixes(A_idx - 1, B_idx - 1))
                else:
                    substitute_last = compute_distance_between_prefixes(A_idx - 1, B_idx - 1)
                    add_last = compute_distance_between_prefixes(A_idx - 1, B_idx)
                    delete_last = compute_distance_between_prefixes(A_idx, B_idx - 1)
                    distance_between_prefixes[A_idx][B_idx] = 1 + min(substitute_last, add_last, delete_last)
            return distance_between_prefixes[A_idx][B_idx]
        
        distance_between_prefixes = [[-1] * len(B) for _ in A]
        return compute_distance_between_prefixes(len(A) - 1, len(B) - 1)
    ```
    - E(A[0, a - 1], B[0, b - 1]): Levenshtein distance between A and B
        1. if the last character of A equals the last character of B, then E(A[0, a - 1], B[0, b - 1]) = E(A[0, a - 2], B[0, b - 2])
        2. if the last character of A is not equal to the last character of B, then E(A[0,a-1], B[0,b-1]) = 1 + min(E(A[0,a-2], B[0,b-2]), E(A[0,a-1], B[0,b-2]), E(A[0,a-2], B[0,b-1]))
            1. transforming A[0,a-1] to B[0,b-1] by transforming A[0,a-2] to B[0,b-2] and then substituting A's last character with B's last characer
            2. transforming A[0,a-1] to B[0,b-1] by transforming A[0,a-1] to B[0,b-2] and then adding B's last character at the end
            3. transforming A[0,a-1] to B[0,b-1] by transforming A[0,a-2] to B[0,b-1] and then deleting A's last character
        

### Example 5
**[Problem]** Count the number of ways to traverse a 2D array
- Input: 2D array
- Output: number of ways to go from top-left to the bottom-right of the 2D array
    + all moves must either go right or down

1. O(nm) time, O(nm) space
    ```
    def number_of_ways(n, m):
        def compute_number_of_ways_to_xy(x, y):
            if x == y == 0:
                return 1
            if number_of_ways[x][y] == 0:
                ways_top = 0 if x == 0 else compute_number_of_ways_to_xy(x - 1, y)
                ways_left = 0 if y == 0 else compute_number_of_ways_to_xy(x, y - 1)
                number_of_ways[x][y] == ways_top + ways_left
            return number_of_ways[x][y]
        number_of_ways = [[0] * m for _ in range(n)]
        return compute_number_of_ways_to_xy(n - 1, m - 1)
    ```
    - since paths must advance down or right, the number of ways to get to the bottom-right entry is the number of ways to get to the entry immediately above it, plus the number of ways to get to the entry immediately to its left

2. analytical solution <sub>n - 1</sub>C<sub>n + m - 2</sub>

### Example 6
**[Problem]** Compute the binomial coefficients
- Input: int n and k
- Output: <sub>k</sub>C<sub>n</sub>
    + the algorithm should never overflow if the final result fits in the integer word size

1. O(nk) time, O(nk) space
    ```
    def compute_binomial_coeffcient(n, k):
        def compute_x_choose_y(x, y):
            if y in (0, x):
                return 1
            if x_choose_y[x][y] = 0:
                without_y = compute_x_choose_y(x - 1, y)
                with_y = compute_x_choose_y(x - 1, y - 1)
                x_choose_y[x][y] = without_y + with_y
            return x_choose_y[x][y]
        
        x_choose_y = [[0] * (k + 1) for _ in range(n + 1)]
        return compute_x_choose_y(n, k)
    ```
    - binomial coefficient counts the number of subsets of size k in a set of size n
    - consider the nth element in the initial set
        + a subset of size k will either contain this element or not contain it == basis for recursive enumeration
        + find all subsets of size k - 1 amongst the first n - 1 elements and add the nth element into these sets, and then find all subsets of size k amongst the first n - 1 elements
        + union of these 2 subsets == all subsets of size k
        + <sub>k</sub>C<sub>n</sub> = <sub>k</sub>C<sub>n - 1</sub> + <sub>k - 1</sub>C<sub>n - 1</sub>

### Example 7
**[Problem]** Search for a sequence in a 2D array
- Input: 2D and 1D array
- Output: true if the 1D array occurs in the 2D array
    + the 1D array occurs in the 2D array if it is possible to start from some entry in the 2D array and traverse adjacenet entries in the order specified by the 1D array till all entries in the 1D array have been visited
    + adjacent entries == above, below, to the left, to the right assuming they exist

1. O(nm|S|) time
    ```
    def is_pattern_contained_in_grid(grid, S):
        def is_pattern_suffix_contained_starting_at_xy(x, y, offset):
            if len(S) == offset:
                return True
            if (0 <= x < len(grid) and 0 <= y < len(grid[x]) and grid[x][y] == S[offset] and (x, y, offset) not in previous_attempts and any(is_pattern_suffix_contained_startin_at_xy(x + a, y + b, offset + 1) for a, b in ((-1, 0), (1, 0), (0, -1), (0, 1)))):
                return True
            previous_attempts.add((x, y, offset))
            return False

        previous_attempts = set()
        return any(is_pattern_suffix_contained_starting_at_xy(i, j, 0) for i in range(len(grid)) for j in range(len(grid[i])))
    ```
    - A: m x n 2D array, S: 1D array
    - suffix of S to match and a starting point to match from:
        1. if the suffix is empty, done
        2. otherwise, for the suffix to occur from the starting point, the first entry of the suffix must equal the entry at the starting point, and the remainder of the suffix must occur starting at a point adjacent to the starting point
    - cache intermediate results to avoid repeated calls to the recursion
    - [EXAMPLE]
        ```
            [[1, 2, 3],
        A =  [3, 4, 5],     S = [1, 3, 4, 6]
             [5, 6, 7]] 
        ```
        1. A[0][0] == S[0] 
        2. A[1][0] != S[1] but A[1][0] == S[1]
        3. A[2][0] != S[2] but A[1][1] == S[2] ...

### Example 8
**[Problem]** The knapsack problem
- Input: array of items each with weight and value, and weight constraint
- Output: total value of the subset of items that has maximum value and satisfies the weight constraint
    + all weights and values are in integers

1. O(nw) time, O(nw) space
    ```
    Item = collections.namedtuple('Item', ('weight', 'value'))

    def optimum_subject_to_capacity(items, capacity):
        def optimum_subject_to_item_and_capacity(k, available_capacity):
            if k < 0:
                return 0
            if V[k][available_capacity] == -1:
                without_curr_item = optimum_subject_to_item_and_capacity(k - 1, available_capcity)
                with_curr_item = (0 if available_capacity < items[k].weight else (items[k].value + optimum_subject_to_item_and_capacity(k - 1, available_capacity - items[k].weight)))
                V[k][available_capacity] = max(without_curr_item, with_curr_item)
            return V[k][available_capacity]
        
        V = [[-1] * (capacity + 1) for _ in items]
        return optimum_subject_to_item_and_capacity(len(items) - 1, capacity)
    ```
    - let the items be numbered from 0 to n - 1, with the weight and the value of teh ith clock denoted by w<sub>i</sub> and v<sub>i</sub>
    - let V[i][w] be the optimum solution when restricted to clocks 0, 1, 2, ..., i - 1 and can carry w weight
    - then V[i][w] is:
        1. max(V[i - 1][w], V[i - 1][w - w<sub>i</sub>] + v<sub>i</sub>), if w<sub>i</sub> <= w
        2. V[i - 1][w], otherwise
    - base cases: i = 0 or w = 0 


### Example 9
**[Problem]** The bedbathandbeyond.com problem
- Input: dictionary (set of strings) and name (string)
- Output: true if the name is the concatenation of a sequence of dictionary words

1. O(n<sup>3</sup>) time (n: length of the input string)
    ```
    def decompose_into_dictionary_words(domain, dictionary):
        last_length = [-1] * len(domain)
        for i in range(len(domain)):
            if domain[:i + 1] in dictionary:
                last_length[i] = i + 1
            if last_length[i] == -1:
                for j in range(i):
                    if last_length[j] != -1 and domain[j + 1:i + 1] in dictionary:
                        last_length[i] = i - j
                        break
        decompositions = []
        if last_length[-1] != -1:
            idx = len(domain) - 1
            while idx >= 0:
                decompositions.append(domain[idx + 1 - last_length[idx]:idx + 1])
                idx -= last_length[idx]
            decompositions = decompositions[::-1]
        return decompositions
    ```
    - cache intermediate results 
    - cache keys: prefixes of the string, value: Boolean denoting wether the prefix can be decomposed into a sequence of valid words
    - store dictionary in a hash table so it is easy to determine if a string is a valid word
    - a prefix of the given string can be decomposed into a sequence of dictionary words exactly if:
        1. it is a dictionary word, or
        2. there exists a shorter prefix which can be decomposed into a sequence of dictionary words and the difference of the shorter prefix and the current prefix is a dictionary word

### Example 10
**[Problem]** Find the minimum weight path in a triangle
- Input: triangle
- Output: weight of a minimum weight path
    - Path in the triangle: 
        + sequence of entries in the triangle in which adjacent entries in the sequence correspond to entries that are adjacent in the triangle
        + must start at the top, descend the triangle continuously, and end with an entry on the bottom row
        + weight of path == sum of the entries
    - [EXAMPLE]
        ```
            2
           4 4
          8 5 6        -->  15 (2 -> 4 -> 5 -> 2 -> 2)
         4 2 6 2
        1 5 2 3 4
        ```
1.  O(n<sup>2</sup>) time, O(n) space
    ```
    def minimum_path_weight(triangle):
        min_weight_to_curr_row = [0]
        for row in triangle:
            min_weight_to_curr_row = [row[j] + min(min_weight_to_curr_row[max(j - 1, 0)], min_weight_to_curr_row[min(j, len(min_weight_to_curr_row) - 1)]) for j in range(len(row))]
        return min(min_weight_to_curr_row)
    ```
    - for any entry in the ith row, looking at the minimum weight path ending at it, the part of the path that ends at the previous row must also be a minimum weight path
    - iteratively compute the minimum weight of a path ending at each entry in Row i
    - since after processing Row i, not need the results for Row i - 1 to process Row i + 1 , can reuse storage

### Example 11
**[Problem]** Pick up coins for maximum gain
- Input: array of coins
- Output: maximum total value for the starting player in the game
    + pick-up-coins game:
        - even number of coins placed in a line
        - 2 players take turns at choosing one coin each - they can only choose from the 2 coins at the ends of the line
        - game ends when all the coins have been picked up
        - the player whose coins have the higher total value wins
        - no passing turns

1. O(n<sup>2</sup>) time (n: number of coins)
    ```
    def maximum_revenue(coins):
        def compute_maximum_revenue_for_range(a, b):
            if a > b:
                return 0
            if maximum_revenue_for_range[a][b] == 0:
                max_revenue_a = coins[a] + min(compute_maximum_revenue_for_range(a + 2, b), compute_maximum_revenue_for_range(a + 1, b - 1))
                max_revenue_b = coins[b] + min(compute_maximum_revenue_for_range(a + 1, b - 1), compute_maximum_revenue_for_range(a, b - 2))
                maximum_revenue_for_range[a][b] = max(max_revenue_a, max_revenue_b)
            return maximum_revenue_for_range[a][b]
        
        maximum_revenue_for_range = [[0] * len(coins) for _ in coins]
        return compute_maximum_revenue_for_range(0, len(coins) - 1)
    ```
    - first player wants to balance selecting high coins with minimizing the coins available to the second player
    - second player assumed to play the best move he possibly can == always choose the coin that maximizes his revenue
    - R[a][b]: the maximum revenue a player can get when it is his turn to play, adn the coins remaining on the table are at indices a to b, inclusive
    - C: an array representing the line of coins
    - since the second player seeks to maximize his revenue, and the total revenue is a constant, it is equivalent for the second player to move so as to minimize the first player's revenue
        + R(a, b) is:
            1. max(C[a] + min(R(a + 2, b), R(a + 1, b - 1)), C[b] + min(R(a + 1, b - 1), R(a, b - 2))) if a <= b
            2. 0, otherwise
    - minimize the maximum revenue the opponent can gain --> min-max 

### Example 12
**[Problem]** Count the number of moves to climb stairs
- Input: int n and k
- Output: number of ways to get to the destination (n steps up)
    - climbing stairs
    - can advance 1 to k steps at a time

1. O(kn) time, O(n) space
    ```
    def number_of_ways_to_top(top, maximum_step):
        def compute_number_of_ways_to_h(h):
            if h <= 1:
                return 1
            if number_of_ways_to_h[h] == 0:
                number_of_ways_to_h[h] = sum(compute_number_of_ways_to_h(h - i) for i in range(1, min(maximum_step, h) + 1))
            return number_of_ways_to_h[h]
        
        number_of_ways_to_h = [0] * (top + 1)
        return compute_number_of_ways_to_h(top)
    ```
    - F(n, k) = sum (i = 1 to k) F(n - i, k)
    - cache F(i, k), 0 <= i <= n for efficiency

### Example 13
**[Problem]** The pretty printing problem
- Input: array of words separated and length of a line
- Output: value of minimum messiness that could be achieved by decomposing the array to fit the lines 
    + no word is split accross lines
    + messiness of end-of-line whitespace: 
        1. messiness of a single line ending with b blank chars == b<sup>2</sup>
        2. total messiness of a sequence of lines == sum of the messiness of all lines

1. O(nL) time, O(n) space (L: line length, n: number of words)
    ```
    def minimum_messiness(words, line_length):
        num_remaining_blanks = line_length - len(words[0])
        min_messiness = [num_remaining_blanks**2] + [float('inf)] * (len(words) - 1)
        for i in range(1, len(words)):
            num_remaining_blanks = line_length - len(words[i])
            min_messiness[i] = min_messiness[i - 1] + num_remaining_blanks ** 2
            for j in reversed(range(i)):
                num_remaining_blanks -= len(words[j]) + 1
                if num_remaining_blanks < 0:
                    break
                first_j_messiness = 0 if j - 1 < 0 else min_messiness[j - 1]
                current_line_messiness = num_remaining_blanks ** 2
                min_messiness[i] = min(min_messiness[i], first_j_messiness + current_line_messiness)
        retrun min_messiness[-1]
    ```
    - the placement that results by removing the ith word from an optimum placement for the first i words is not always an optimum placement for the first i - 1 words
    - if in the optimum placement for the ith word the last line consists of words j, j + 1, ..., i, then in this placement, the first j - words must be placed optimally
    1. in an optimum placement of the first i words, the last line consists of some subset of words ending in the ith word
    2. since the first i words are assumed to be optimally placed, the placement of words on the lines prior to the last one must be optimum
    - minimum messiness M(i) when placing the first i words = min<sub>j<=i</sub>f(j, i) + M(j - 1)
        + f(j, i): messiness of a single line consisting of words j to i inclusive
    - cache values for M to improve time complexity

### Example 14
**[Problem]** Find the longest nondecreasing subsequence
- Input: array of numbers
- Output: length of a longest nondecreasing subsequence in the array

1.  O(n<sup>2</sup>) time, O(n) space
    ```
    def longest_nondecreasing_subsequence_length(A):
        max_length = [1] * len(A)
        for i in range(1, len(A)):
            max_lengt[i] = max(1 + max((max_length[j] for j in range(i) if A[i] >= A[j]), default=0), max_length[i])
        return max(max_length)
    ```
    - L[i]: length of the longest nondecreasing subsequence of A that ends at and includes Index i
    - the longest nondecreasing subsequence that ends at Index i:
        1. is of length 1 (if A[i] is smaller than all preceding entries) or 
        2. has some element, say at Index j, as its penultimate entry, in which case teh subsequence restricted to A[0, j] must be the longest subsequence ending at Index j
        + L[i] is
            1. 1 (if A[i] is less than all previous entries)
            2. 1 + max{L[j]} j < i and A[j] <= A[i]


