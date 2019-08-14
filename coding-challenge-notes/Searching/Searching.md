# Searching 
## Python
### Binary Search
- eliminate half the keys from consideration by keeping the keys in sorted order
- if the search key is not equal to the middle element of the array, one of the 2 sets of keys to the left and to the right of the middle element can be eliminated from further consideration
- applicable to searching in sorted arrays and to searching an interval of real numbers or integers

- iterative implementation of binary search
    ```
    def bsearch(t, A):
        L, U = 0, len(A) - 1
        while L <= U:
            M = L + (U - L) // 2 # not M = (L + U)/2 because it is possible to overflow
            if A[M] < t:
                L = M + 1
            elif A[M] == t:
                return M:
            else:
                U = M - 1
        return -1
    ```

- ADV: O(log n) time
- DIS: need a sorted array and sorting an array takes O(n log n) time

- Example
- Input: array of students, sorted by descending gpA, with ties broken on name
- Use library binary search routine to perform fast searches in this array
1. O(log n) time
    ```
    Student = collections.namedtuple('Student', ('name', 'grade_point_average'))

    # comparator 
    def comp_gpa(student):
        return (-student.grade_point_average, student.name)
    
    def search_student(students, target, comp_gpa):
        i = bisect.bisect_left([comp_gpa(s) for s in students], comp_gpa(target))
        return 0 <= i < len(students) and students[i] == target
    ```

### Python libraries
- `bisect` module for binary search 
- operations (assume `a` is a sorted array):

    | function                      | description                                                       |
    |-------------------------------|-------------------------------------------------------------------|
    | `bisect.bisect_left(a,x)`     | find the first element that is not less than a target value <br> returns the index of the first entry that is *greater than or equal to* the targeted value <br> if all elements in the list are less than *x*, returns `len(a)`
    | `bisect.bisect_right(a,x)`    | find the first element that is greater than a targeted value <br> returns the index of the first entry that is *greater than* the targeted value <br> if all elements in the list are less than or equal to *x*, returns `len(a)`