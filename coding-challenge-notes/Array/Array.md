# Array 
## Python
### Array 
- Retrieve and updating: O(1)
- Insertion at last index: O(1)* 
  +  O(1) if not full
  +  O(n) if full -> need to create a new array and copy over all the elements
- Deletion/Insertion at i<sup>th</sup> index: O(n)
  + Need to shift the successive elements to the left for deletion and to the right for insetion
- Arrays in Python are provided by the `list` type

### Instantiating list
- `[3, 5, 7, 11]`: literal 
- `[1] + [0] * 4  # [1, 0, 0, 0, 0]`: use * to repeat element n times and + to concatenate arrays
- `list(range(10))`: cast range to list
- list comprehension
  1. an input sequence
  2. an iterator over the input sequence
  3. (optional) a logical condition over the iterator
  4. an expression that yields the elements of the derived list
  + Example 1
    `[x**2 for x in range(6) if x % 2 == 0] == [0, 4, 16]`
  + Example 2
    ```
    A = [1, 3, 5]
    B = ['a', 'b']
    [(x, y) for x in A for y in B]
    ```
  + Example 3 (2D to 1D)
    ```
    M = [['a', 'b', 'c'], ['d', 'e', 'f']]
    x for row in M for x in row == ['a', 'b', 'c', 'd', 'e', 'f']
    ```
  + Example 4
    ```
    A = [[1, 2, 3], [4, 5, 6]]
    [[x**2 for x in row] for row in A] == [[1, 4, 9], [16, 25, 36]] 
    ```

### Basic operations
- `len(A)`: length of array
- `A.remove(a)`: remove 1<sup>st</sup> occurrence of `a`
- `A.insert(index, value)`: insert at new value at the index
  + if the index >= len(A), inserts at the end
- `a in A`: checking if a value is present in an array ([NOTE] O(n))

### Copying
#### 1D array
- `b = a`: shallow copy == whenever either array is altered, the other one is also altered
- `b = list(a), b = copy.copy(a), b = copy.deepcopy(a)`: deep copy == whenever one array is altered, the other is not affected 
#### 2D array
- `b = a, b = list(a), b = copy.copy(a)`: shallow copy
  + copy(a) becomes shallow copy because it copies all the elements within the a (all the arrays in a)
  + that means the elements in a and b are the same object (same object id) so altering the element of array in a or b will alter the other too
- `b = copy.deepcopy(a)`: deep copy 

### Key methods for arrays
| Method                                             | Description     
|----------------------------------------------------|
| `min(A)`, `max(A)`                                 | min/max value in A                 
| `bisect.bisect(A, x)` & `bisect.bisect_right(A, x)`| A: sorted array, x: value to insert, returns index i that maintains the order when x is inserted to A (when x already exists in A, returns the index to the left of any x already existing in A)
| `bisect.bisect_left(A, x)`                         | same as right but inserts to the index to the right of any x in A when x already exists in A
| `A.reverse()`                                      | in place reverse sort
| `reversed(A)`                                      | returns an iterator that sorts A in reverse order 
| `A.sort()`                                         | in place sort
| `sorted(A)`                                        | returns a copy of A which is sorted
| `del A[i]`                                         | deletes the i<sup>th</sup> element
| `del A[i:j]`                                       | removes the slice


### Slicing
#### Example
```
A = [1, 6, 3, 4, 5, 2, 7]
A[2:4] == [3, 4]
A[2:] == [3, 4, 5, 2, 7]
A[:4] == [1, 6, 3, 4]
A[:-1] == [1, 6, 3, 4, 5, 2]
A[-3:] == [5, 2, 7]
A[-3:-1] == [5, 2]
A[1:5:2] == [6, 4]
A[5:1:-2] == [2, 4]
A[::-1] == [7, 2, 5, 4, 3, 6, 1] # reverses list
A[k:] + A[:k] # rotates A by k to the left
B = A[:] # shallow copy of A into B
```


