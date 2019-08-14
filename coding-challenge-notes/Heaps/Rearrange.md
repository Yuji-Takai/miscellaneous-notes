### Example 1
**[Problem]** Merge sorted files 
- Input: a set of sorted sequences
- Output: union of the sequences as a sorted sequence

1. O(n log k) time, O(k) space (k = number of input sequences)
    ```
    def merge_sorted_arrays(sorted_arrays):
        min_heap = []
        sorted_arrays_iters = [iter(x) for x in sorted_arrays]
        for i, it in enumerate(sorted_arrays_iters):
            first_element = next(it, None)
            if first_element is not None:
                heapq.heappush(min_heap, (first_element, i))
        
        result = []
        while min_heap:
            smallest_entry, smallest_array_i = heapq.heappop(min_heap)
            smallest_array_iter = sorted_arrays_iters[smallest_array_i]
            result.append(smallest_entry)
            next_element = next(smallest_array_iter, None)
            if next_element is not None:
                heapq.heappush(min_heap, (next_element, smallest_array_i))
        return result
    ```
    - repeatedly pick the smallest element amongst the firsst element of each of the remaining part of each of the sequences
    - min-heap is ideal for maintaining a collection of elements when need to add arbitrary values and extract the smallest element
    1. the min-heap is initialized to the first entry of each array 
    2. extract the smallest entry and add it to the output
    3. add the next entry of the array whose element was just extracted from the min-heap
    4. continue until the min-heap becomes empty
    - if the data comes form files and is written to a file, instead of arrays, need only O(k) additional storage    

2. O(n log k) time, O(k) space - Python 
    ```
    def merge_sorted_arrays(sorted_arrays):
        return list(heapq.merge(*sorted_arrays))
    ```

3. O(n log k) time, O(n) space
    - recursively merge the k files, 2 at a time using the merge step from merge sort
    - log k stages, and each has time complexity O(n) --> O(n log k) time
    - merge sort space == O(n) --> considerably worse than the heap based approach when k << n

### Example 2
**[Problem]** Sort an increasing-decreasing array 
- Input: k-increasing-decreasing array
- Output: input array sorted
    + **k-increasing-decreasing array**: elements repeatedly increase up to a certain index after which they decrease, then again increase, a total of k times
    + [Example] `A = [57, 131, 493, 294, 221, 339, 418, 452, 442, 190] --> k = 4`

1. O(n log k) time
    ```
    def sort_k_increasing_decreasing_array(A):
        sorted_subarrays = []
        INCREASING, DECREASING = range(2)
        subarray_type = INCREASING
        start_idx = 0
        for i in range(1, len(A) + 1):
            if (i == len(A) or (A[i - 1] < A[i] and subarray_type == DECREASING) or (A[i - 1] >= A[i] and subarray_type == INCREASING)):
                sorted_subarrays.append(A[start_idx:i] if subarray_type == INCREASING else A[i - 1:start_idx - 1:-1])
                start_idx = i
                subarray_type = (DECREASING if subarray_type == INCREASING else INCREASING)
        return merge_sorted_arrays(sorted_subarrays)
    ```
    - first reverse the order of each of the decreasing subarrays
    - use the merge sorted arrays method from example 1 to merge these subarrays

2. O(n log k) time - Python
    ```
    def sort_k_increasing_decreasing_array(A):
        class Monotonic:
            def __init__(self):
                self._last = float('inf')
            def __call__(self, curr):
                result = curr < self._last
                self._last = curr
                return result
        return merge_sorted_arrays([list(group)[::-1 if is_decreasing else 1] for is_decreasing, group in itertools.groupby(A, Monotonic())])
    ```

### Example 3
**[Problem]** Sort an almost-sorted array 
- Input: a very long sequence of numbers (*k-sorted array*) and int *k* (each number is at most *k* away from its correctly sorted position)
- Output: numbers in sorted order

1. O(n log k) time, O(k) space
    ```
    def sort_approximately_sorted_array(sequence, k):
        min_heap = []
        for x in itertools.islice(sequence, k):
            heapq.heappush(min_heap, x)
        result = []
        for x in sequence:
            smallest = heapq.heappushpop(min_heap, x)
            result.append(smallest)
        while min_heap:
            smallest = heapq.heappop(min_heap)
            result.append(smallest)
        return result
    ```
    - after reading k + 1 numbers, the smallest number in that group must be smaller than all following numbers 
    - use min-heap:
        + need to store k + 1 numbers
        + want to be able to efficiently extract the minimum number and add a new number
    1. add k numbers to a min-heap
    2. add additional numbers to the min-heap and extract the minimum from the heap
    3. when the numbers run out, just perform extraction