# Invariants
## Python
### Invariants
- **Invariant**: a condition that is true during execution of a program
    + may be on the values of the variables of the program, or on the control logic
    + well-chosen one can be used to rule out potential solutions that are suboptimal or dominated by other solutions

- often, the invariant is a subset of the set of input space (ex: subarray)

- Examples:
    + Binary search: maintains the invariant that the space of candidate solutions contains all possible solutions as the algorithm executes
    + selection sort: work with successively larger subarrays beginning at index 0, and preserve the invariant that these subarrays are sorted, their elements are less than or equal to the remaining elements, and the entire array remains a permutation of the original array