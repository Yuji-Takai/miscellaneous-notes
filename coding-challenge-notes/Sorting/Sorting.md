# Sorting
## Python
### Sorting

- O(n log n) sorts
| Name          | in-place     | stability | worst case         |
|---------------|--------------|-----------|--------------------|
| heapsort      | yes          | no        | O(n log n)         |
| merge sort    | no           | yes       | O(n log n)         |
| quicksort     | yes          |           | O(n<sup>2</sup>)   |

- in-place: O(1) space
- stability: entries which are equal appear in their original order

- *short arrays* (10 or fewer elements) --> **insertion sort** is easier and faster than asymptotically superior sorting algos
- *every element is known to be at most k places from its final location* --> **min-heap** can be used for O(n log k) algo
- *a small number of distinct keys* (ex: int in [0, 255]) --> **counting sort** 
    + records, for each element, the number of elements less than it
    + the count can be can be kept in *an array* (if the largest number is comparable in value to the size of the set being sorted) or *BST* (keys = numbers & values = their frequencies)
- *multiple duplicate keys* --> add the keys to BST, with linked lists for elements which have the same keys --> sorted result can be derived from an *in-order* traversal of the BST

- most sorting algos are NOT stable 
    + **merge sort** can be made stable
    + Solution: add the index as an integer rank to the keys to break ties

- most sorting algos are based on a compare function
    + possible to use numerical attributes directly like in **radix sort**

- Heaps can be helpful in sorting 
    + O(log n) inserts
    + O(1) lookup for max (min) element
    + O(log n) deletion of max (min) element


### Sorting in Python
- when sorting a custom class, define the compare function in the class by `__lt__(self, other)` or define the lambda function inside the `sort` function

```
class Student(object):
    def __init__(self, name, gpa):
        self.name = name
        self.gpa = gpa
    def __lt__(self, other):
        return self.name < other.name

students = [
    Student('A', 4.0),
    Student('B', 3.0),
    Student('C', 2.0),
    Student('D', 1.0)
]

# sorting according to __lt__ defined in Student. students remain unchanged
students_sort_by_name = sorted(students)

# sort students in-place by gpa
students.sort(key=lambda student: student.gpa)
```

### Python Library
- `sort()`: sort a list in-place
    + implements a stable in-place sort for list objects
    + returns `None` == calling list itself is updated
    + 2 optional args: 
        1. `key=None`: if not None, assumes key to be a function which takes list elements and maps them to obejcts which are comparable == this function defines the sort order
        2. `reverse=False`: if True, sort in descending order; otherwise ascending order
- `sorted()`: sort an iterable
    + input = iterable, output = new list containing all items from teh iterable in ascending order 
    + original list is unchanged
    + 2 optional args like `sort()`

