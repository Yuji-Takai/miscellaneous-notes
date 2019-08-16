### Example 1
**[Problem]** Compute the intersection of 2 sorted arrays 
- Input: 2 sorted arrays (may have duplicate entries)
- Output: an array containing elements that are present in both of the input arrays (should be duplicate free)
    + [EXAMPLE] `[2, 3, 3, 5, 5, 6, 7, 7, 8, 12] & [5, 5, 6, 8, 8, 9, 10, 10] --> [5, 6, 8]`

1. O(mn) time
    ```
    def intersect_two_sorted_arrays(A, B):
        return [a for i, a in enumerate(A) if (i == 0 or a != A[i-1]) and a in B]
    ```
    - traversing through all the elements of one array and comparing them to the elements of the other array

2. O(m log n) time
    ```
    def intersect_two_sorted_arrays(A, B):
        def is_present(k):
            i = bisect.bisect_left(B, k)
            return i < len(B) and B[i] == k
        
        return [a for i, a in enumerate(A) if (i == 0 or a != A[i - 1]) and is_present(a)]
    ```
    - iterate through the first array and use binary search to test if the element in the first array is present in the second array
    - improve runtime by choosing the shorter array for the outer loop
    - best solution if one set is much smaller than the other
    - not the best when the array lengths are similar because not exploiting the fact that both are sorted

3. O(n + m) time
    ```
    def intersect_two_sorted_arrays(A, B):
        i, j, intersection_A_B = 0, 0, []
        while i < len(A) and j < len(B):
            if A[i] == B[j]:
                if i == 0 or A[i] != A[i - 1]:
                    intersection_A_B.append(A[i])
                i, j = i + 1, j + 1
            elif A[i] < B[j:
                i += 1
            else:
                j += 1
        return intersection_A_B
    ```
    - simultaneously advancing through the 2 input arrays in increasing order
    - at each iteration:
        1. if the array elements differ, the smaller one can be eliminated
        2. if the array elements are equal, add that value to the intersection and advance both (handle duplicates by comparing the current element with previous one)

### Example 2
**[Problem]** Merge 2 sorted arrays 
- Input: 2 sorted arrays of integers
- Output: update the first array to the combined entries of the 2 arrays in sorted order

1. O(m + n) time, O(1) space
    ```
    def merge_two_sorted_arrays(A, m, B, n):
        a, b, write_idx = m - 1, n - 1, m + n - 1
        while a >= 0 and b >= 0:
            if A[a] > B[b]:
                A[write_idx] = A[a]
                a -= 1
            else:
                A[write_idx] = B[b]
                b -= 1
            write_idx -= 1
        while b >= 0:
            A[write_idx] = B[b]
            write_idx, b = write_idx - 1, b - 1
    ```
    - take advantage of the spare space at the end of the first array by filling the first array from its end
    - the last element in the result will be written to index m + n - 1
    - will never overwrite an entry in the first array that has not already been processed
        + even if every entry of the second array is larger than each element of the first array, all elements of the second array will fill up indices m to m + n - 1 inclusive, which does not conflict with entries stored in the first array