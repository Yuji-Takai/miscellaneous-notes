### Example 1
**[Problem]** Compute the k closest stars 
- Input: list of stars and int *k*
- Output: list of *k* closest stars to Earth (coordinate = (0, 0, 0))

1. O(n log k) time, O(k) space
    ```
    class Star:
        def __init__(self, x, y, z):
            self.x, self.y, self.z = x, y, z
        @property
        def distance(self):
            return math.sqrt(self.x**2 + self.y**2 + self.z**2)
        def __lt__(self, rhs):
            return self.distance < rhs.distance
        
    
    def find_closest_k_stars(stars, k):
        max_heap = []
        for star in stars:
            heapq.heappush(max_heap, (-star.distance, star))
            if len(max_heap) == k + 1:
                heapq.heappop(max_heap)
        return [s[1] for s in heapq.nlargest(k, max_heap)]
    ```
    - keep a set of candidates and iteratively update the candidate set
        + candidates = *k* closest stars have seen so far
        + when examine a new start, want to see if it should be added to the candidates == compare the candidate that is furthest from Earth with the new star
        + thus, use max-heap
    - since python has only min-heap, insert tuple (negative of distance, star) to sort in reversed distance order

### Example 2
**[Problem]** Compute the median of online data 
- Input: sequence of numbers presented in a streaming fashion (cannot back up to read an earlier value)
- Output: median of the sequence (need to output the median after reading in each new element)
1. O(log n) time
    ```
    def online_median(sequence):
        min_heap = []
        max_heap = []
        result = []
        for x in sequence:
            heapq.heappush(max_heap, -heapq.heappushpop(min_heap, x))
            if len(max_heap) > len(min_heap):
                heappq.heappush(min_heap, -heapq.heappop(max_heap))
            result.append(0.5 * (min_heap[0] + (-max_heap[0])) if len(min_heap) == len(max_heap) else min_heap[0])
        return result
    ```
    - the median of a collection divides the collection into 2 equal parts
    - when a new element is added to the collection, the parts can change by at most one element, and the element to be moved is the largest of the smaller half or the smallest of the larger half
    - use 2 heaps:
        + max-heap for the smaller half
        + min-heap for the larger half 
        + keep these heaps balanced in size
    - Example: 1, 0, 3, 5, 2, 0, 1
        + L: min-heap, H: max-heap
        1. read in 1: L = [1],          H = [],         median = 1
        2. read in 0: L = [1],          H = [0],        median = (1 + 0) / 2 = 0.5
        3. read in 3: L = [1, 3]        H = [0],        median = 1
        4. read in 5: L = [3, 5]        H = [1, 0]      median = (3 + 1) / 2 = 2
        5. read in 2: L = [2, 3, 5]     H = [1, 0]      median = 2
        6. read in 0: L = [2, 3, 5]     H = [1, 0, 0]   median = (2 + 1) / 2 = 1.5
        7. read in 1: L = [1, 2, 3, 5]  H = [1, 0, 0]   median = 1

### Example 3 
**[Problem]** Compute the k largest elements in a max-heap
- Input: array A representing a max-heap
- Output: k largest elements stored in the max-heap (order does not matter)
    + cannot modify the heap

1. O(k log k) time, O(k) space
    ```
    def k_largest_in_binary_heap(A, k):
        if k <= 0:
            return []
        candidate_max_heap = []
        candidate_max_heap.append((-A[0], 0))
        result = []
        for _ in range(k):
            candidate_idx = candidate_max_heap[0][1]
            result.append(-heapq.heappop(candidate_max_heap)[0])
            left_child_idx = 2 * candidate_idx + 1
            if left_child_idx < len(A):
                heapq.heappush(candidate_max_heap, (-A[left_child_idx], left_child_idx))
            right_child_idx = 2 * candidate_idx + 2
            if right_child_idx < len(A):
                heapq.heappush(candidate_max_heap, (-A[right_child_idx], right_child_idx))
        return result
    ```
    - [Fact] heap has partial order information = a parent node always stores value greater than or equal to the values stored at its children
        + root == largest element 
        + second largest element == larger of the root's children
    1. create a max-heap of candidates, initialized to hold the index 0, which serves as a reference to A[0] i.e. the root
        + the indices in the max-heap are ordered according to corresponding value in A
    2. iteratively perform k extract-max operations from the max-heap
        + each extraction of an index i is followed by inserting the indices of i's left child (2i + 1) and right child (2i + 2) to the max-heap, assuming these children exist


