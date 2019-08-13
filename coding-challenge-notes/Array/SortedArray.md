### Example 6
**[Problem]** Delete duplicates from a sorted array
- Input: sorted array 
- Output: original array with all duplicates removed and remaining elements being shifted left to fill the emptied indices & number of valid elements
1. O(n) time, O(1) space
  ```
  def delete_duplicates(A):
    if not A:
      return 0
    write_index = 1
    for i in range(1, len(A)):
      if A[write_index - 1] != A[i]:
        A[write_index] = A[i]
        write_index += 1
    return write_index
  ```