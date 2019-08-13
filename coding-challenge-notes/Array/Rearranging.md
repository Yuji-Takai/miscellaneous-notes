## 1D Array
### Example 1
**[Problem]** Reorder entries so that the even entries appear first
- Space complexity must be O(1) == do not use additional storage
1. O(n) time, O(1) space
  ```
  def even_odd(A):
    next_even, next_odd = 0, len(A) - 1
    while next_even < next_ odd:
      if A[next_even] % 2 == 0:
        next_even += 1
      eles:
        A[next_even], A[next_odd] = A[next_odd], A[next_even]
        next_odd -= 1
  ```
  - partion array into 3 subarrays: even, unclassified, odd

### Example 2
**[Problem]**　Dutch national flag pattern
- Input: an array A and an index i into A 
- Output: A with the elements rearranged s.t. all elements less than A[i] (the pivot) appear first, followed by elements equal to the pivot, followed by elements greater than the pivot
1. O(n) time, O(1) space
  ```
  def dutch_flag_partition(A, i):
    pivot = A[i]
    # 1st pass: group elements smaller than pivot
    smaller = 0
    for i in range(len(A)):
      if A[i] < pivot:
        A[i], A[smaller] = A[smaller], A[i]
        smaller += 1
    # 2nd pass: group elements larger than pivot
    larger = len(A) - 1
    for i in reversed(range(len(A))):
      if A[i] > pivot:
        A[i], A[larger] = A[larger], A[i]
        larger -= 1
  ```
  - classification into elements less than, equal to, and greater than the pivot in two passes

2. O(n) time, O(1) space
  ```
  def dutch_flag_partition(A, i):
    pivot = A[i]
    smaller, equal, larger = 0, 0, len(A)
    while equal < larger:
      if A[equal] < pivot:
        A[smaller], A[equal] = A[equal], A[smaller]
        smaller, equal = smaller + 1, equal + 1
      elif A[equal] == pivot:
        equal += 1
      else:
        larger -= 1
        A[equal], A[larger] = A[larger], A[equal]
  ```
  - maintain 4 subarrays:
    + bottom - elements less than pivot: `A[:smaller]`
    + middle - elements equal to pivot: `A[smaller:equal]`
    + unclassified: `A[equal:larger]`
    + top - elements greater than pivot: `A[larger:]`

### Example 3
**[Problem]** Computing an alternation
- Input: Array of *n* numbers
- Output: Original array with elements rearranged s.t. A[0] <= A[1] >= A[2] <= A[3] >= A[4] <= A[5] >= ...

1. O(n) time
  ```
  def rearrange(A):
    for i in range(len(A)):
      A[i:i + 2] = sorted(A[i:i + 2], reverse=i % 2) # can condition sorting with the reverse field
  ``` 
  - Iterate through the array
  - Swap A[i] and A[i + 1] when i is even and A[i] > A[i + 1] OR i is odd and A[i] < A[i + 1]
  - Same time complexity as the approach based on median finding (Rearrange the elements around the median, and then perform the interleaving) BUT much easier to implement and operates in an *online* fashion == never needs to store more than two elements in memory or read a previous element

### Example 4
**[Problem]** Rotate Array
- Input: Array and integer k
- Output: Original array rotated to the right by k steps

1. O(n) time and space - using extra array
  ```
  def rotate(nums, k):
    if k == 0:
      return
    backup = [0] * len(nums)
    for i in range(len(nums)):
      backup[(i + k) % len(nums)] = nums[i]
    for i in range(len(nums)):
      nums[i] = backup[i]
  ```

2. O(2n) time and O(1) space 
  ```
  def rotate(nums, k):
    k = k % len(nums)
    if k == 0:
      return
    nums.reverse()
    nums[:k] = reversed(nums[:k])
    nums[k:] = reversed(nums[k:])
  ```
  - reverse the entire array
  - reverse the first k elements (the last k elements from the original array)
  - reverse the k + 1th element onwards
  [1, 2, 3, 4, 5, 6, 7] & k = 3
  [7, 6, 5, 4, 3, 2, 1] # reverse
  [5, 6, 7, 4, 3, 2, 1] # reverse first k elements
  [5, 6, 7, 1, 2, 3, 4] # reverse the latter elements

3. O(n) time and space (Python trick)
  ```
  def rotate(nums, k):
    k = k % len(nums)
    if k == 0:
      return
    nums[:k], nums[k:] = nums[-k:], nums[:-k]
  ```

4. O(n) time, O(1) space
  ```
  public void rotate(int[] nums, int k) {
    k = k % nums.length;
    int count = 0;
    for (int start = 0; count < nums.length; start++) {
      int current = start;
      int prev = nums[start];
      do {
        int next = (current + k) % nums.length;
        int temp = nums[next];
        nums[next] = prev;
        prev = temp;
        current = next;
        count++;
      } while (start != current);
    }
  }
  ```
  - shift the numbers only n times
  - by using `start`, can handle cases when length of `nums` is divisble by k

### Example 5
**[Problem]** Permute the elements of an array
- Input: Array A (size = n) and permutation P
- Output: Original A with P applied
- Example: A = [a, b, c, d] & P = [2, 0, 1, 3] --> A = [b, c, a, d]

1. O(n) time, O(n) space
  - Allocate a new arr B of the same length 
  - B[P[i]] = A[i] for each i
  - Copy B to A

2. O(n) time, O(1) space - can modify perm
  ```
  def appl_permutation(perm, A):
    for i in range(len(A)):
      next = i
      while perm[next] >= 0:
        A[i], A[perm[next]] = A[perm[next]], A[i]
        temp = perm[next]
        perm[next] -= len(perm)
        next = temp
    perm[:] = [a + len(perm) for a in perm]
  ```
  - [FACT] every permutation can be represented by a collection of independent cyclic permutations (i.e. moves all elements by a fixed offset, wrapping around)
    --> a single cyclic permutation can be performed with constant additional storage
    - permutation can be applied using a constant amount of additional storage by applying each cyclic permutation one-at-a-time
    --> need to identify disjoint cycles that constitute the permutation
  - To find and apply the cycle that includes entry i, keep going forward from i to P[i] till get back to i
  - After done with the cycle, need to find another cycle that has not yet been applied 
　
  - use sign bit in the entries in the permutation-array
    + subtract n from P[i] after applying P[i]
    + if an entry in P[i] is negative, already performed the corresponding move
  [EXAMPLE] perm = [3, 1, 2, 0]
  1. save value of A[3] and move A[0] to A[3]
  2. update perm as [-1, 1, 2, 0]
  3. move A[3] to A[0] --> since perm[0] is negative, done with with the cycle starting at 0
  4. update perm as [-1, 1, 2, -4]
  5. examine P[1] and continue

3. O(n<sup>2</sup>) time, O(1) space - cannot modify perm
  ```
  def apply_permutation(perm, A):
    def cyclic_permutation(start, perm, A):
      i, temp = start, A[start]
      while True:
        next_i = perm[i]
        next_temp = A[next_i]
        A[next_i] = temp
        i, temp = next_i, next_temp
        if i == start:
          break

    for i in range(len(A)):
      j = perm[i]
      while j != i:
        if j < i:
          break
        j = perm[j]
      cyclic_permutation(i, perm, A)
  ```
  - go from left to right
  - apply the cycle only if the current position is the leftmost position in the cycle

### Example 6
**[Problem]** Compute the next permutation
- Input: Permutation array
- Output: Next permutation under dictionary ordering
  + Dictionary ordering: permutation *p* appear before *q* if 
    1. *p* and *q* differ in their array representations
    2. starting from index 0, the corresponding entry for *p* is less than that for *q*
  + if the permutation is the last permutation, return the empty array

1. O(n) time, O(1) space
  ```
  def next_permutation(perm):
    inversion_point = len(perm) - 2
    while (inversion_point >= 0 and perm[inversion_point] >= perm[inversion_point + 1]):
      inversion_point -= 1
    if inversion_point == -1:
      return []
    for i in reversed(range(inversion_point + 1, len(perm))):
      if perm[i] > perm[inversion_point]:
        perm[inversion_point], perm[i] = perm[i], perm[inversion_point]
        break
    return perm
  ```
  - Need to increase the permutation by as little as possible 
    + start from the right, and look at the longest decreasing suffix
    + look at the entry *e* that appears just before the longest decreasing suffix
      - *e* must be less than some entries in the suffix
    + swap *e* with the smallest entry *s* in the suffix which is larger than *e* to minimize the change to the prefix
      - new *prefix* is smallest possible for all permutations greater than the initial permutation, but the new *suffix* may not be the smallest 
    + get smallest suffix by sorting the entries in the suffix from smallest to largest
      - the suffix was originally *decreasing*, and after replacing *s* by *e*, it remains decreasing 
      --> just reverse the suffix to sort the suffix from smallest to largest

## 2D Array
### Example 1
**[Problem]** Compute the spiral ordering of a 2D array
- Input: nxn 2D array
- Output: spiral ordering of the array
  + [EXAMPLE]
  ```
  [[ 1,  2,  3,  4],
   [ 5,  6,  7,  8],
   [ 9, 10, 11, 12],
   [13, 14, 15, 16]]
  --> [1, 2, 3, 4, 8, 12, 16, 15, 14, 13, 9, 5, 6, 7, 11, 10]
  ```

1. O(n<sup>2</sup>) time 
  ```
  def matrix_in_spiral_order(A):
    def matrix_layer_in_clockwise(offset):
      if offset == len(A) - offset - 1:
        # Reached center (when A has odd dimension)
        spiral_ordering.append(A[offset][offset])
        return
      spiral_ordering.extend(A[offset][offset:-1 - offset])
      spiral_ordering.extend(list(zip(*A))[-1 - offset][offset:-1 - offset])
      spiral_ordering.extend(A[-1 - offset][-1 -offset:offset:-1])
      spiral_ordering.extend(list(zip(*A))[offset][-1 - offset:offset:-1])
    spiral_ordering = []
    for offset in range((len(A) + 1) // 2):
      matrix_layer_in_clockwise(offset)
    return spiral_ordering
  ```
  1. Add first n - 1 elements of the first row
  2. Add the first n - 1 elements of the last column
  3. Add the last n - 1 elements of the last row in reverse order
  4. Add the last n - 1 elements of the first column in reverse order
  - Repeat 1 - 4 for n x n, (n - 2) x (n - 2), (n - 4) x (n - 4), ... 2D arrays

2. O(n<sup>2</sup>) time - Optimized
  ```
  def matrix_in_spiral_order(A):
    SHIFT = ((0, 1), (1, 0), (0, -1), (-1, 0))
    direction = x = y = 0
    spiral_ordering = []
    for _ in range(len(A) ** 2):
      spiral_ordering.append(A[x][y])
      A[x][y] = 0
      next_x, next_y = x + SHIFT[direction][0], y + SHIFT[direction][direction][1]
      if (next_x not in range(len(A)) or next_y not in range(len(A)) or A[next_x][next_y] == 0):
        direction = (direction + 1) & 3
        next_x, next_y = x + SHIFT[direction][0], y + SHIFT[direction][1]
      x, y = next_x, next_y
    return spiral_ordering
  ```

### Example 2
**[Problem]** Rotate 2D array
- Input: nxn 2D array
- Output: original array rotated by 90 degrees clockwise
  + [EXAMPLE]
  ```
  [[1,   2,  3,  4],                   [[13,  9, 5, 1],
   [5,   6,  7,  8],               -->  [14, 10, 6, 2], 
   [9,  10, 11, 12],                    [15, 11, 7, 3],
   [13, 14, 15, 16]]                    [16, 12, 8, 4]]
  ```
1. O(n<sup>2</sup>) time, O(1) space
  ```
  def rotate_matrix(A):
    matrix_size = len(A) - 1:
    for i in range(len(A) // 2):
      for j in range(i, matrix_size - i):
        # [NOTE] A[~i] for i in [0, len(A) - 1] is A[-(i + 1)]
        (A[i][j], A[~j][i], A[~i][~j], A[j][~i]) = (A[~j][i], A[~i][~j], A[j][~i], A[i][j])
  ```
  - perform rotation in a layer-by-layer fashion (different layer can be processed independently)
  - within a layer, can exchange groups of 4 elements at a time to perform the rotation
    + 1 --> 4, 4 --> 16, 16 --> 13, 13 --> 1, 2 --> 8, ....
  - from outermost layer to the center of the array, perform exchange within a layer iteratively using the four-way swap
