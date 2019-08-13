### Example 5
**[Problem]** Advancing through an array
- Input: array of n integers, where A[i] denotes the maximum advance from index i
- Output: whether it is possible to advance to the last index starting from 1st index of the array
- Example:
  + A = [3, 3, 1, 0, 2, 0, 1] -> True
    1. take 1 step from A[0] to A[1]
    2. take 3 steps from A[1] to A[4]
    3. take 2 steps from A[4] to A[6]

1. O(n) time, O(1) space
  ```
  def can_reach_end(A):
    furthest_reach_so_far, last_index = 0, len(A) - 1
    i = 0
    while i <= furthest_reach_so_far and furthest_reach_so_far < last index:
      furthest_reach_so_far = max(furthest_reach_so_far, A[i] + i)
      i += 1
    return furthest_reach_so_far >= last_index 
  ```
  - Iterate through the array & track the index furthest possible to advance to
  - Furthest possible to advance from index i == i + A[i]
  - If, for some i before the end of the array, i is the furthest index that is reachable, CANNOT reach the last index
  - Otherwise, reach the end
  - Example 1: A = [3, 3, 1, 0, 2, 0, 1] -> [0, 3, 4, 4, 4, 6, 6, 7] -> 7 >= 6 -> True
  - Example 2: A = [3, 2, 0, 0, 2, 0, 1] -> [0, 3, 3, 3, 3] -> not possible -> False
  - This implementation is robust wrt negative entries