## 1D Array
### Example 1
**[Problem]** Enumerate all primes to *n*
- Input: Integer *n*
- Output: Array with all the primes between 1 and *n*

1. O(n<sup>3/2</sup>) time, O(1) space
  - iterate over all i from 2 to *n* (O(n))
  - for each i, test if i is prime:
    + divide i by each integer from 2 to the square root of i and check if the remainder is 0 (O(sqrt(n)))
    + [NOTE] no need to test beyond the square root of i, since if i has a divisor other than 1 and itself, it must also have a divisor that is no greater than its square root
  - NOT exploiting the facts of whether previous integers were prime or not

2. O(n log(log n)) time, O(n) space - Seiving
  ```
  def generate_primes(n):
    primes = []
    is_prime = [False, False] + [True] * (n - 1)
    for p in range(2, n + 1)
  ```
  - Use Boolean array (`is_prime`) to encode the candidates (if `is_prime[i] == True`, then i is potentially a prime)
  - Initially, every number >= 2 is a candidate (`is_prime[i] == True` for all i >= 2)
  - Whenever determine a number is a prime, add it to the result (`prime`)
    + None of its multiples can be primes, so remove all its multiples from the candidate set by writing false in the corresponding locations
  - Continue till the end

  [Analysis]
  - time to sift out the multiples of p is proportional to n/p
    + overall time complexity = O(n/2 + n/3 + n/5 + n/7 + ...) -> O(n log(log n))



3. O(n log(log n)) time, O(n) space - More optimized seiving
  ```
  def generate_primes(n):
    if n < 2:
      return []
    size = (n - 3) // 2 + 1
    primes = [2]
    is_prime = [True] * size
    for i in range(size):
      if is_prime[i]:
        p = i * 2 + 3
        primes.append(p)
        for j in range(2 * i ** 2 + 6 * i + 3, size, p):
          is_prime[j] = False
  ``` 
  - `is_prime[i]`: whether 2i + 3 is prime or not
  - seiving from p<sup>2</sup> = (4i<sup>2</sup> + 12i + 9) 
    + the index in is_prime is (2i<sup>2</sup> + 6i + 3) because `is_prime[i]` represents 2i + 3
  - [NOTE] need to use long for j because p<sup>2</sup> might overflow


## 2D Array
### Example 1
**[Problem]** Sudoku checker
- Input: 9x9 array representing a partially completed Sudoku
- Output: validity
  + 0 = entry is blank, every other entry is in [1, 9]

1. O(n<sup>2</sup>) - list comprehension way
  ```
  def is_valid_sudoku(partial_assignment):
    def has_duplicate(block):
      block = list(fliter(lambda x: x != 0, block))
      return len(block) != len(set(block))
    n = len(partial_assignment)

    # check row and column constraints
    if any(has_duplicate([partial_assignment[i][j] for j in range(n)]) or has_duplicate([partial_assignment[j][i] for j in range(n)]) for i in range(n)):
      return false
    
    # check region constraints
    region_size = int(math.sqrt(n))
    return all(not has_duplicate([partial_assignment[a][b] for a in range(region_size * I, region_size * (I + 1)) for b in range(region_size * J, region_size * (J + 1))]) for I in range(region_size) for J in range(region_size))
  ```

2. O(n<sup>2</sup>) - Python way 
  ```
  def is_valid_sudoku(partial_assignment):
    region_size = int(math.sqrt(len(partial_assignment)))
    return max(collections.Counter(k for i, row in enumerate(partial_assignment) for j, c in enumerate(row) if c != 0 for k in ((i, str(c)), (str(c), j), (i / region_size, j / region_size, str(c)))).values(), default=0) <= 1
  ```

### Example 1
**[Problem]** Compute rows in Pascal's triangle
- Input: non-negative integer *n*
- Output: first *n* rows of Pascal's triangle

1. O(n<sup>2</sup>) time, O(n<sup>2</sup>) space
  ```
  def generate(n):
    result = [[1] * (i + 1) for i in range(n)]
    for i in range(n):
      for j in range(1, i):
        result[i][j] = result[i - 1][j - 1] + result[i - 1][j]
    return result
  ```
  - keep the arrays left-aligned (1<sup>st</sup> is at location 0)
  - j<sup>th</sup> entry in i<sup>th</sup> row is:
    + 1 if `j = 0` or `j = i`
    + otherwise, sum of (j - 1)<sup>th</sup> and j<sup>th</sup> entries in the (i - 1)<sup>th</sup> row

2. O(n<sup>2</sup>) time, O(n) space
