# Heap
## Python
### Heap
- Complete binary tree
- **Max Heap**: the key at each node is at least as great as the keys stored at its children
- heap can be implemented as an array
- the children of the node at index i are at indices 2i + 1 and 2i + 2
- O(log n) insertions
- O(1) lookup for the max/min element (max heap/min heap)
- O(log n) deletion of max/min element
- extract-max (-min): delete and return the maximum/minimum element
- O(n): search for arbitrary key
- heap referred as priority queue:
    + each element has priority == key, and deletion removes the element with the highest priority

- use heap when care about the **largest** or **smallest** elements and do not need to support fast lookup, delete, or search operations for arbitrary elements
- heap is a good choice when computing the *k* **largest** (min heap) or *k* **smallest** (max heap) elements in a collection 


### Example
- Input: a sequenc of strings presented in "streaming" fashion: cannot back up to read an earlier value
- Output: k longest strings in the sequence (no need to be in order)

1. O(n log k) time (n: number of strings in the input)
    ```
    def top_k(k, stream):
        min_heap = [(len(s), s) for s in itertools.islice(stream, k)]
        heapq.heapify(min_heap)
        for next_string in stream:
            heapq.heappushpop(min_heap, (len(next_string), next_string))
        return [p[1] for p in heapq.nsmallest(k, min_heap)]
    ```
    - as process the input, want to track the k lonest strings seen so far
    - out of these strings, the string to be evicted when a longer string is to be added is the shortest one 
        + a min heap (NOT max heap) is used - if max heap is used, need to remove the max k times, requiring O(k log n) time 
        + initialize min heap with size k
        + push a string
    - can improve best-case time complexity by first comparing the new string's length with the length of the string at the top of the heap and skipping the insert if the new string is too short to be in the set


### Heap libraries
- min heap functionality provided by `heapq` module
- Operations

    | function                  |                                                                    |
    |---------------------------|--------------------------------------------------------------------|
    | `heapq.heapify(L)`        | heapify elements in L in-place                                     |
    | `heapq.nlargest(k, L)`    | returns the *k* largest elements in L                              |
    | `heapq.nsmallest(k, L)`   | returns the *k* smallest elements in L                             |
    | `heapq.heappush(h, e)`    | pushes a new element on the heap                                   |
    | `heapq.heappop(h)`        | pops the smallest element from the heap                            |
    | `heappushpop(h, a)`       | pushes `a` on the heap, then pops and returns the smallest element |
    | `e = h[0]`                | returns the smallest element on the heap without popping it        |

- if need to build a max heap:
    + for integers or floats: insert their negative to get the effect of a max-heap using `heapq`
    + for objects: implement `__lt()__` appropriately