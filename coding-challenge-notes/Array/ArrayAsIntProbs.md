### Example 3
**[Problem]** Increment an arbitrary-precision integer
- Input: array of digits encoding a nonnegative decimal integer D
- Output: Updated array representing the integer D + 1
1. O(n) solution
  ```
  def plus_one(A):
    A[-1] += 1
    for i in reverse(range(1, len(A))):
      if A[i] != 10:
        break
      A[i] = 0
      A[i - 1] += 1
    if A[0] == 10:
      A[0] = 1
      A.append(0)
    return A
  ```
  - if A[0] == 10, there is a carry-out, so need to append a 0 at the end of the array (Ex: 999 -> 1000)

### Example 4
**[Problem]** Multiply 2 arbitrary-precision integers
- Input: 2 arrays representing integers
- Output: Array representing the product of the 2 integers
1. O(nm) solution
  ```
    def multiply(num1, num2):
      sign = -1 if (num1[0] < 0) ^ (num2[0] < 0) else 1
      num1[0], num2[0] = abs(num1[0]), abs(num2[0])
      result = [0] * (len(num1) + len(num2))
      for i in reversed(range(len(num1))):
        for j in reversed(range(len(num2))):
          result[i + j + 1] += num1[i] * num2[j]
          result[i + j] += result[i + j + 1]
          result[i + j + 1] %= 10
      
      # remove the leading 0's
      result = result[next((i for i, x in enumerate(result) if x != 0), len(result)):] or [0]
      return [sign * result[0]] + result[1:]  
  ```
  - multiply the first number by each digit of the second, and then add all the resulting terms
