## 1D array
### Example 1 
**[Problem]** Search a sorted array for first occurrence of k
- Input: a sorted array and a key
- Output: index of the first occurrence of that key in the array

1. O(log n) time
    ```
    def search_first_of_k(A, k):
        left, right, result = 0, len(A) - 1, -1
        while left <= right:
            mid = left + (right - left) // 2
            if A[mid] > k:
                right = mid - 1
            elif A[mid] == k:
                result = mid
                right = mid - 1
            else:
                left = mid + 1
        return result
    ```
    - naive approach: 
        + find the index of any element equal to the key *k*
        + after finding such an element, traverse backwards from it to find the first occurrence of that element 
        + O(n) worst case time: traversing backwards when all entries equal to k
    - fundamental idea of binary search is to maintain set of candidate solutions
        + if the element at index *i* equals *k*, do not know whether *i* is the first element equal to *k*, but do know that no subsequent elements can be the first one --> remove all elements with index *i* + 1 or more
    
### Example 2
**[Problem]** Search a sorted array for entry equal to its index 
- Input: sorted array of distinct integers
- Output: index such that the element at index *i* equals *i*

1. O(log n) time
    ```
    def search_entry_equal_to_its_index(A):
        left, right = 0, len(A) - 1
        while left <= right:
            mid = left + (right - left) // 2
            difference = A[mid] - mid
            if difference == 0:
                return mid
            elif difference > 0:
                right = mid - 1
            else:
                left = mid + 1
        return -1
    ```
    - the difference between an entry and its index increases by at least 1 as iterate through the array
    - if A[j] > j, then no entry after *j* can satisfy the given criterion
        + because each element in the array is at least 1 greater than the previous element
    - if A[j] < j, then no entry before *j* can satisfy the given criterion (same reason)

### Example 3 
**[Problem]** Search a cyclically sorted array
- Input: cyclically sorted array (all elements are distinct)
- Output: smallest element in the array
    + **Cyclically sorted**: possible to cyclically shift the array's entries so that it becomes sorted
    + [Example] `[378, 478, 550, 631, 103, 203, 220, 234, 279, 368]` 

1. O(log n) time
    ```
    def search_smallest(A):
        left, right = 0, len(A) - 1
        while left < right:
            mid = left + (right - left) // 2
            if A[mid] > A[right]:
                left = mid + 1
            else:
                right = mid
        # loop ends when left == right
        return left
    ```
    - for any *m*, if A[m] > A[n - 1], then the minimum value must be an index in the range [m + 1, n - 1]
    - if A[m] < A[n - 1], then no index in the range [m + 1, n - 1] can be the index of the minimum value
    - not possible for A[m] = A[n - 1] since it is given that all elements are distinct
    - [NOTE] this problem cannot be solved in less than linear time when elements may be repeated

## 2D array
### Example 1
**[Problem]** Search in a 2D sorted array 
- Input: 2D sorted array (m x n) and number
- Output: true if the number exists in the array otherwise false
    + **2D sorted array**: array's rows and columns are nondecreasing
    + [Example]
        ```
        [[-1,  2,  4,  4,  6],
         [ 1,  5,  5,  9, 21],
         [ 3,  6,  6,  9, 22],
         [ 3,  6,  8, 10, 24],
         [ 6,  8,  9, 12, 25],
         [ 8, 10, 12, 13, 40]]
        ```

1. O(m + n) time (inspect at most m + n - 1 elements)
    ```
    def matrix_search(A, x):
        row, col = 0, len(A[0]) - 1
        while row < len(A) and col >= 0:
            if A[row][col] == x:
                return True
            elif A[row][col] < x:
                row += 1
            else:
                col -= 1
        return False
    ```
    - take advantage of the fact that both rows and columns are sorted
        + compare *x* with A[0][n - 1]
            1. x = A[0][n - 1] --> found
            2. x > A[0][n - 1] --> x > all elements in Row 0
            3. x < A[0][n - 1] --> x < all elements in Column n - 1
            + in either case of 2 or 3, have a 2D array with one fewer row or column to search
        
