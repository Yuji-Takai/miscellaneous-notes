### Example 1
**[Problem]** Find the min and max simultaneously 
- Input: array
- Output: min and max elements of array
    + compute the min and max with less than 2(n - 1) comparision required to compute the min and max independently

1. O(n) time, O(1) space
    ```
    MinMax = collections.namedtuple('MinMax', ('smallest', 'largest'))

    def find_min_max(A):
        def min_max(a, b):
            return MinMax(a, b) if a < b else MinMax(b, a)
        if len(A) <= 1:
            return MinMax(A[0], A[0])
        global_min_max = min_max(A[0], A[1])
        for i in range(2, len(A) - 1, 2):
            local_min_max = min_max(A[i], A[i + 1])
            global_min_max = MinMax(min(global_min_max.smallest, local_min_max.smallest), max(global_min_max.largest, local_min_max.largest))
        if len(A) % 2:
            global_min_max = MinMax(min(global_min_max.smallest, A[-1]), max(global_min_max.largest, A[-1]))
        return global_min_max
    ```
    - partition the array into min candidates and max candidates by comparing successive pairs
        + n/2 candidates for min and n/2 candidates for max at the cost of n/2 comparisons
    - takes n/2 - 1 comparisons to find the min from the min candidates and n/2 - 1 comparisons to find the max from max candidates
    - total of 3n/2 - 2 comparisons
    - by maintaining candidate min and max as process successive pairs, O(1) space is achieved

### Example 2 
**[Problem]** Find the kth largest element
- Input: array with distinct elements and int k
- Output: kth largest element in the array

1. Avg O(n), Worst O(n<sup>2</sup>) time, O(1) space
    ```
    def find_kth_largest(k, A):
        def find_kth(comp):
            def partition_around_pivot(left, right, pivot_idx):
                pivot_value = A[pivot_idx]
                new_pivot_idx = left
                A[pivot_idx], A[right] = A[right], A[pivot_idx]
                for i in range(left, right):
                    if comp(A[i], pivot_value):
                        A[i], A[new_pivot_idx] = A[new_pivot_idx], A[i]
                        new_pivot_idx += 1
                A[right], A[new_pivot_idx] = A[new_pivot_idx], A[right]
                return new_pivot_idx
            
            left, right = 0, len(A) - 1
            while left <= right:
                pivot_idx = random.randint(left, right)
                new_pivot_idx = partition_around_pivot(left, right, pivot_idx)
                if new_pivot_idx == k - 1:
                    return A[new_pivot_idx]
                elif new_pivot_idx > k - 1:
                    right = new_pivot_idx - 1
                else:
                    left = new_pivot_idx + 1
        return find_kth(operator.gt)
    ```
    - sorting then accessing element at index k - 1 --> O(n log n) time but overkill --> if want to find the first largest, just need to traverse once i.e. O(n) 
    - store a candidate set of k elements in a min-heap --> O(n log k) time and O(k) space --> faster than sorting but not in-place + overkill --> computes the k largest elements in sorted order, but all that is asked for is the kth largest element
    - quick select:
        + kth largest element in-place without completely sorting the array
        + select an element at random (pivot), and partition the remaining entries into those greater than the pivot and those less than the pivot
        1. if there are exactly k - 1 elements greater than the pivot, the pivot mustbe the kth largest element
        2. if there are more than k - 1 elements greater than the pivot, can discard elements less than or equal to the pivot --> kth largest must be greater than the pivot
        3. if there are less than k - 1 elements greater than the pivot, can discard elements greater than or equal to the pivot
    - O(1) space by using the array itself to record the partitioning 
    - Worst of O(n<sup>2</sup>) if the randomly selected pivot is the smallest or largest element in the current subarray

### Example 3
**[Problem]** Find the missing IP address 
- Input: list containing roughly one billion IP addresses (each of which is a 32 bit quantity)
- Output: an IP address that is not in the file
    + unlimited drive space but only a few megabytes of RAM at disposal

1. O(1) space (2<sup>16</sup> integer entries)
    ```
    def find_missing_element(stream):
        NUM_BUCKET = 1 << 16
        counter = [0] * NUM_BUCKET
        stream, stream_copy = itertools.tee(stream)
        for x in stream:
            upper_part_x = x >> 16
            counter[upper_part_x] += 1
        
        BUCKET_CAPACITY = 1 << 16
        candidate_bucket = next(i for i, c in enumerate(counter) if c < BUCKET_CAPCITY)

        candidates = [0] * BUCKET_CAPACITY
        stream = stream_copy
        for x in stream_copy:
            upper_part_x = x >> 16
            if candidate_bucket == upper_part_x:
                lower_part_x = ((1 << 16) - 1) & x
                candidates[lower_part_x] = 1
        
        for i, v in enumerate(candidates):
            if v == 0:
                return (candidate_bucket << 16) | i
    ```
    - make multiple passes
        + a pass through the bit array to count the number of IP addresses present whose leading bit is a 1, and the number of addresses whose leading bit is a 0
            - at least one IP address must exist which is not present in the array, so at least one of these two counts is below 2<sup>31</sup>
    
    - count on groups of bits
        + count the number of IP addresses in the array that begin with 0, 1, 2, ..., 2<sup>16</sup> - 1 using an array of 2<sup>16</sup> integers that can be represented with 32 bits
        + for every IP address in the file, make its 16 MSBs to index into this array and increment the count of that number
        + since the bit array contains fewer than 2<sup>32</sup> numbers, there must be one entry in the array that is less than 2<sup>16</sup>
        + there is at least one IP address which has those upperbits and is not in the bit array
        + in the second pass, focus only on the addresses whose leading 16 bits match the one found, and use a bit array of size 2<sup>16</sup> to identify a missing address

### Example 4
**[Problem]** Find the duplicate and missing elements 
- Input: array of n integers, each between 0 and n - 1, inclusive and exactly one element appears twice, implying that exactly one number between 0 and n - 1 is missing from the array
- Output: duplicate and missing numbers

1. O(n) time, O(1) space
    ```
    DuplicateAndMissing = collections.namedtuple('DuplicateAndMissing', ('duplicate', 'missing'))

    def find_duplicate_missing(A):
        # XOR of all numbers from 0 to n - 1 and all entries in A
        miss_XOR_dup = functools.reduce(lambda v, i: v ^ i[0] ^ i[1], enumerate(A), 0)

        # sets all of bits in differ_bit to 0 except for the LSB in miss_XOR_dup that's 1
        differ_bit, miss_or_dup = miss_XOR_dup & (~(miss_XOR_dup - 1)), 0
        for i, a in enumerate(A):
            if i & differ_bit:
                miss_or_dup ^= i
            if a & differ_bit:
                miss_or_dup ^= a
        return (DuplicateAndMissing(miss_or_dup, miss_or_dup ^ miss_XOR_dup) if miss_or_dup in A else DuplicateAndMissing(miss_or_dup ^ miss_XOR_dup, miss_or_dup))
    ```
    - 2 unknowns *t* (duplicate) and *m* (missing)
        + equation 1: sum of the elements in the array is exactly (n-1)n/2 + t - m
        + equation 2: XOR of all the numbers from 0 to n - 1 XORed with the XOR of all entries in the array yields m XOR t
            - the 1s in the result are exactly where t and m differ
    - if m and t differ in the kth bit:
        1. compute the XOR of the numbers from 0 to n - 1 in which kth bit is 1, and the entries in the array in which the kth bit is 1 --> let this result be h --> h must be one of m or t
        2. make another pass through A to determine if h is the duplicate or the missing element
        
### Example 5
**[Problem]** Find the missing element
- Input: array of n - 1 integers, each between 0 and n - 1, inclusive, and all numbers in the array are distinct
- Output: the missing number
1. O(n) time, O(1) space
    - subtract the sum of the numbers in the array from (n - 1)n/2 to get the missing number

2. O(n) time, O(1) space
    1. compute the XOR of all the integers from 0 to n - 1 inclusive,
    2. XOR the result from 1 with the XOR of all the elements in the array 
    + every element in the array, except for the missing element, cancels out with an integer from the first set --> resulting XOR == missing element

 ### Example 6
**[Problem]** Find the duplicate element
- Input: array of n + 1 integers, each between 0 and n - 1, inclusive, and with exactly one element appearing twice
- Output: the duplicate number
1. O(n) time, O(1) space
    - subtract (n - 1)n/2 from the sum of the numbers in the array to get the missing number

2. O(n) time, O(1) space
    1. compute the XOR of all the integers from 0 to n - 1 inclusive,
    2. XOR the result from 1 with the XOR of all the elements in the array 
    + every element in the array, except for the duplicate element, cancels out with an integer from the first set --> resulting XOR == duplicate element

