### Example 1
**[Problem]** Compute all mnemonics for a phone number
- Input: 7-digit phone number
- Output: all possible character sequences that correspond to the phone number
  + `2276696 --> ACRONYM, ABPOMZN, ...`

1. O(4<sup>n</sup>n) time
  ```
  MAPPING = ('0', '1', 'ABC', 'DEF', 'GHI', 'JKL', 'MNO', 'PQRS', 'TUV', 'WXYZ')

  def phone_mnemonic(phone_number):
    def phone_mnemonic_helper(digit):
      if digit == len(phone_number):
        mnemonics.append(''.join(partial_mnemonic))
      else:
        for c in MAPPING[int(phone_number[digit])]:
          partial_mnemonic[digit] = c
          phone_mnemonic_helper(digit + 1)
    
    mnemonics, partial_mnemonic = [], [0] * len(phone_number)
    phone_mnemonic_helper(0)
    return mnemonics
  ```
  - brute force approach --> 7 nested for loops where the iteration variables correspond to the 7 ranges to enumerate all possible mnemonics 
    + repetitive and inflexible
  - [GENERAL RULE] any such enumeration is best computed using **recursion**
  [Time complexity analysis]
  1. no more than 4 possible characters for each digit == number of recursive calls T(n) <= 4T(n - 1) where n = number of digits in the number
    + T(n) = O(4<sup>n</sup>)
  2. for the function calls that entail recursion, the time spent within the function, not including the recursive calls, is O(1)
  3. each base case entails making a copy of a string and adding it to the result 
    + since each such string has length *n*, each base case takes time O(n)
  4. Thus time complexity = O(4<sup>n</sup>n)

### Example 2
**[Problem]** Compute all valid IP addresses
- Input: mangled string (IP address which has all the periods vanished)
- Output: all possible IP address corresponding to the string
  + **IP address**: 4 decimal strings separated by periods

1. O(1) time complexity (b/c total number of IP addresses is a constant 2<sup>32</sup>)
  ```
  def get_valid_ip_address(s):
    def is_valid_part(s):
      return len(s) == 1 or (s[0] != '0' and int(s) <= 255)
    
    result, parts = [], [None] * 4
    for i in range(1, min(4, len(s))):
      parts[0] = s[:i]
      if is_valid_part(parts[0]):
        for j in range(1, min(len(s) - i, 4)):
          parts[1] = s[i:i + j]
          if is_valid_part(parts[1]):
            for k in range(1, min(len(s) - i - j, 4)):
              parts[2], parts[3] = s[i + j:i + j + k], s[i + j + k:]
              if is_valid_part(parts[2]) and is_valid_part(parts[3]):
                result.append('.'.join(parts))
    return results 
  ```