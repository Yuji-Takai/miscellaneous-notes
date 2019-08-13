### Example 1
**[Problem]** Check if a string is palindromic
- Input: String s 
- Output: If s is palindromic or not

1. O(n) time, O(1) space
  ```
  def is_palindromic(s):
    return all(s[i] == s[~i] for i in range(len(s) // 2))
  ```

### Example 4
**[Problem]** Longest Palindromic Substring
- Input: String s (max length == 1000)
- Output: longest palindromic substring in s

1. O(n<sup>3</sup>) Brute force
  - pick all possible starting and ending positions for a substring, and verify if it is a palindrome

2. O(n<sup>2</sup>) time, O(n<sup>2</sup>) space - Dynamic Programming 
  ```
  public int longestPalindromicSubstr(String s) {
      int n = s.length();
      boolean[][] p = new boolean[n][n];
      int maxLength = 1
      for (int i = 0; i < n; i++) {
          p[i][i] = true;
      }
      int start = 0;
      for (int i = 0; i < n - 1; i++) {

      }
  }
  ```
  - Define P(i, j) as:
    + P(i, j) = true if the substring S<sub>i</sub>...S<sub>j</sub> is  a palindrome
    + false otherwise
  - P(i, j) = (P(i + 1, j - 1) && S<sub>i</sub> == S<sub>j</sub>)
  - Base cases:
    + P(i, i) = true
    + P(i, i + 1) = (S<sub>i</sub> == S<sub>i + 1</sub>)

### Example 5
**[Problem]** Test palindromicity
- Input: string s
- Output: if s is palindromic
  + For this problem, palindromic string is defined to be a string which, when *all nonalphanumeric are removed*, reads the same front to back *ignoring cases*
  + `"Able was I, ere I saw Elba!" --> True`
1. O(n) time, O(1) space
  ```
  def is_palindromic(s):
    i, j = 0, len(s) - 1
    while i < j:
      while not s[i].isalnum() and i < j: 
        i += 1
      while not s[i].isalnum() and i < j:
        j -= 1
      if s[i].lower() != s[j].lower():
        return False
      i, j = i + 1, j + 1
    return True
  ```
  - use 2 indices to traverse the string, one forwards, the other backwards, skipping nonalphanumeric chars, performing case-insensitive comparison on the alphanumeric characters

2. O(n) time, O(1) space - Python way
  ```
  def is_palindromic(s):
    return all(a == b for a, b in zip(map(str.lower, filter(str.isalnum, s)), map(str.lower, filter(str.isalnum, reversed(s)))))
  ```