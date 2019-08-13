### Example 1
**[Problem]** Convert string to int
- Input: String s
- Output: int of s
1. O(n) time, O(n) space
  ```
  def int_to_string(x):
    is_negative = False
    if x < 0:
      x, is_negative = -x, True
    s = []
    while True:
      s.append(chr(ord('0') + x % 10))
      x //= 10
      if x == 0:
        break
    return ('-' if is_negative else '') + ''.join(reversed(s))
  ```
  - start from least significant digit to most significant digit
  - adding a digit to the beginning of a string is expensive, since all digits have to be moved
    + more time efficient approach is to add each computed digit to the end, and then reverse the computed sequence

### Example 2
**[Problem]** Convert int to string
- Input: int *n*
- Output: String representation of *n*
1. O(n) time, O(n) space
  ```
  def string_to_int(s):
    return functools.reduce(lambda running_sum, c: running_sum * 10 + string.digits.index(c), s[s[0] == '-':], 0) * (-1, if s[0] == '-' else 1)
  ```
  
### Example 3
**[Problem]** Base conversion
- Input: s, b1, b2 (string s represents an integer in base b1)
- Output: string representing the same integer in base b2
1. O(n(1 + log<sub>b<sub>2</sub></sub> b<sub>1</sub>)) time (n == len(s))
  ```
  def convert_base(num_as_str, b1, b2):
    def construct_from_base(nums_as_int, base):
      return ('' if num_as_int == 0 else construct_from_base(num_as_int // base, base) + string.hexdigits[nums_as_int % base].upper())
    
    is_negative = num_as_string[0] == '-'
    num_as_int = functools.reduce(lambda x, c: x * b1 + string.hexdigits.index(c.lower()), num_as_string[is_negative:], 0)
    return ('-' if is_negative else '') + ('0' if num_as_int == 0 else construct_from_base(num_as_int, b2))
  ```
  1. convert a string in base b<sub>1</sub> to integer type using a sequence of multiply and adds
  2. convert that integer type to a string in base b<sub>2</sub> using a sequence of modulus and division operations
  - since the conversion algorithm is naturally expressed in terms of smaller similar subproblems, it is natural to implement it using recursion


### Example 4
**[Problem]** Compute the spreadsheet column encoding 
- Input: Spread column id (str)
- Output: Corresponding integer
  + `"A" --> 1, "AA" --> 27, "ZZ" --> 702`
1. O(n) time
  ```
  def ss_decode_col_id(col):
    return functools.reduce(lambda result, c: result * 26 + ord(c) - ord('A') + 1, col, 0)
  ```
  - just convert a string representing a base-26 number to the corresponding integer (except A corresponds to 1 not 0)

### Example 5
**[Problem]** Replace and remove
- Input: array of characters and int *n* denoting the number of entries of the array that the operation is to be applied to 
- Output: the last index of the array that results from removing each 'b' and replacing each 'a' by 2 'd''s in the original array
  + can assume that there is enough space in the array to hold the final result

1. O(n) time, O(1) space
  ```
  def replace_and_remove(size, s):
    # forward iteration to remove b's and count the number of a's
    write_idx, a_count = 0, 0
    for i in range(size):
      if s[i] != 'b':
        s[write_idx] = s[i]
        write_idx += 1
      if s[i] == 'a':
        a_count += 1
    
    # backward iteration to replace a's with dd's starting from the end
    cur_idx = write_idx - 1
    write_idx += a_count - 1
    final_size = write_idx + 1
    while cur_idx >= 0:
      if s[cur_idx] == 'a':
        s[write_idx - 1 : write_idx + 1] = 'dd'
        write_idx -= 2
      else:
        s[write_idx] = s[cur_idx]
        write_idx -= 1
      cur_idx -= 1
    return final_size
  ```
  1. delete 'b's and compute the final number of valid characters of the string, with a forward iteration through the string
  2. replace each 'a' by 2 'd's, iterating backwards from the end of the resulting string
    + if there are more 'b's than 'a's, the number of valid entries will decrease
    + if there are more 'a's than 'b's, the number will increase
  - if used library implementation of insert and remove while traversing the arrary, the complexity will be O(n<sup>2</sup>) because both are O(n) operations
  - if used a new array, O(n) time but O(n) space as well
  
### Example 6
**[Problem]** Look-and-say problem
- Input: int *n*
- Output: n<sup>th</sup> integer in the look-and-say sequence representing as a string
  + **Look-and-say sequence**: 
    1. starts with 1
    2. subsequence numbers are derived by describing the previous number in terms of consecutive digits
    `1, 11, 21, 1211, 111221, 312211, 13112221, 1113213211`

1. O(n2<sup>n</sup>) time
  ```
  def look_and_say(n):
    def next_number(s):
      result, i = [], 0
      while i < len(s):
        count = 1
        while i + 1 < len(s) and s[i + 1]:
          i += 1
          count += 1
        result.append(str(count) + s[i])
        i += 1
      return ''.join(result)
    s = '1'
    for _ in range(1, n):
      s = next_number(s)
    return s
  ```
  - compute the nth number by iteratively applying the rule n - 1 times
  - since counting digits, natural to use strings to represent the integers in the sequence
    + going from ith number to the (i + 1)th number entails scanning the digits from most significant to least significant, counting the number of consecutive equal digits, and writing these counts
  [Time complexity analysis]
  1. each successive number can have at most twice as many digits as the previous number (happens when all digits are different)
    - maximum length number has length no more than 2<sup>n</sup>
  2. there are n iterations and the work in each iteration is proportional to the length of the number computed in the iteration
  3. thus simple bound on the time complexity = O(n2<sup>n</sup>)

2. O(n2<sup>n</sup>) time - Python way
  ```
  def look_and_say(n):
    s = '1'
    for _ in range(n - 1):
      s = ''.join(str(len(list(group))) + key for key, group in itertools.groupby(s))
    return s
  ```
  
### Example 7
**[Problem]** Convert from Roman to Decimal
- Input: valid Roman number string s
- Output: integer s corresponds to 
  + `I == 1, V == 5, X == 10, L == 50, C == 100, D == 500, M == 1000`
  + Roman number string is valid if symbols appear in *nonincreasing* order, with the following exceptions allowed:
    - *I* can immediately precede *V* and *X*
    - *X* can immediately precede *L* and *C*
    - *C* can immediately precede *D* and *M*

1. O(n) time
  ```
  def roman_to_int(s):
    T = {'I': 1, 'V': 5, 'X': 10, 'L': 50, 'C': 100, 'D': 500, 'M': 1000}
    return functools.reduce(lambda val, i: val + (-T[s[i]] if T[s[i]] < T[s[i + 1]] else T[s[i]]), reversed(range(len(s) - 1)), T[s[-1]])
  ```
  - right to left scan
  - if the symbol after the current one is greater than it, subtract the current symbol
  [NOTE]
  - it does NOT check that when a smaller symbol appears to the left of a larger one that it is one of the 6 allowed exceptions
    + so will return 99 for 'IC' for example

### Example 8
**[Problem]** Write a String Sinusoidally
- Input: string s
- Output: snakestring of s
  + snakestring: left-right top-to-bottom sequence in which characters appear when s is written in sinusoidal fashion
  + [Example] 
  ```
  'Hello World!' --> 'e lHloWrdlo!'

    e              l 
  H   l   o  W   r   d 
        l      o       !
  ```

1. O(3n) time
  ```
  def snake_string(s):
    result = []
    for i in range(1, len(s), 4):
      result.append(s[i])
    for i in range(0, len(s), 2):
      result.append(s[i])
    for i in range(3, len(s), 4):
      result.append(s[i])
    return result
  ```

2. O(n) time - Python way
  ```
  def snake_string(s):
    return s[1::4] + s[2::4] + s[3::4]
  ```

### Example 9
**[Problem]** Implement Run-Length Encoding (RLE)
- Input: 
  1. Encoding: uncompressed raw string
  2. Decoding: RLE compressed string
- Output:
  1. Encoding: RLE compressed string
  2. Decoding: uncompressed string
  + **Run-Length Encoding Compression**: encode successive repeated characters by the repetition count and the character
  + [Example] `'aaaabcccaa' <-> '4a1b3c2a'`

1. O(n) time 
  ```
  def decoding(s):
    count, result = 0, []
    for c in s:
      if c.isdigit():
        count = count * 10 + int(c) # handling case when two digits are consecutive
      else:
        result.append(c * count)
        count = 0
    return ''.join(result)
  
  def encoding(s):
    result, count = [], 1
    for i in range(1, len(s) + 1):
      if i == len(s) or s[i] != s[i - 1]:
        result.append(str(count) + s[i - 1])
        count = 1
      else:
        count += 1
    return ''.join(result)

