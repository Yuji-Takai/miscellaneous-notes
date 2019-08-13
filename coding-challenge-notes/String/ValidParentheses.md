**[Problem]** Valid Parentheses (Single Type)
- Input: String containing '(' and ')'
- Output: True if valid, else False

1. O(n) time, O(1) space
  ```
  def isValid(s):
    left = 0
    for character in s:
      if character == '(':
        left += 1
      elif character == ')' and left > 0:
        left -= 1
      else:
        return False
    return left == 0
  ```
  - when encounter '(', do not know if the expression is valid -> keep track of the unmatched '(' by incrementing `left`
  - when encounter ')', if `left > 0` then there is a matching open bracket so decrement `left`, else means invalid so return `False`
  - return if there remains unmatched left parenthesis 

### Example 2
**[Problem]** Valid Parenthesis (Mulitple Type)
- Input: String containing '(', ')', '{', '}', '[', ']'
- Output: True if valid, else False

1. O(n) time, O(n) space - Stack
  ```
  def isValid(s):
    stack = []
    mapping = {')': '(', '}': '{', ']': '['} 
    openPar = set(['(', '{', '['])
    for char in s:
      if char in openPar:
        stack.append(char)
      elif stack and stack[-1] == mapping[char]:
        stack.pop()
      else 
        return False
    return not stack
  ```
  - Utilize the property of valid parenthesis => if the entire expression is valid, its subexpression should also be valid
  - Remove the expression whenever encounter a matching pair of parenthesis 
  - Process from outside to inwards using stack
  - mapping: keeps the code clean and makes the code scalable for more types of parameter

### Example 3
**[Problem]** Longest Valid Parenthesis
- Input: string containing just '(' and ')'
- Output: length of the longest valid parentheses substring
1. O(n<sup>3</sup>) time, O(n) space
  ```
  public int longestValidParenthesis(String s) {
      int maxlen = 0;
      for (int i = 0; i < s.length(); i++) {
          for (int j = i + 2; j <=s.length(); j+=2) {
              if (isValid(s.substring(i, j))) {
                  maxlen = Math.max(maxlen, j - i);
              }
          }
      }
  }
  public boolean isValid(String s) {
      Stack<Character> stack = new Stack<Character>();
      for (int i = 0; i < s.length(); i++) {
          if (s.charAt(i) == '(') {
              stack.push('(');
          } else if (!stack.empty() && stack.peek() == '(') {
              stack.pop();
          } else {
              return false;
          }
      }
      return stack.empty();
  }
  ```
  - consider every possible non-empty even length substring from the given string and check the validity
  - use stack to checkn the validity:
    + for every '(', push it onto the stack 
    + for every ')', pop from stack
      - if '(' is not available on the stack for popping at anytime OR if stack contains some elements after processing complete substring, the substring if invalid

2. O(n) time, O(n) space - Dynamic Programming
  ```
  public int longestValidParenthesis(String s) {
      int maxans = 0;
      int[] dp = new int[s.length()];
      for (int i = 1; i < s.length(); i++) {
          if (s.charAt(i) == ')') {
              if (s.charAt(i - 1) == '(') {
                  dp[i] = (i >= 2 ? dp[i - 2] : 0) + 2;
              } else if ((i - dp[i - 1]) > 0 && s.charAt(i - dp[i - 1] - 1) == '(') {
                  dp[i] = dp[i - 1] + ((i - dp[i - 1] >= 2) ? dp [i - dp[i - 1] - 2] : 0) + 2;
              }
              maxans = Math.max(maxans, dp[i]);

          }
      }
      return maxans;
  }
  ```
  - `dp`: i<sup>th</sup> element of dp represents the length of the longest valid substring ending at i<sup>th</sup> index
  - [FACT] valid substrings MUSt end with ')' 
    + substrings ending with '(' always contain 0 at their corresponding dp indices
    + update dp array only when ')' is encountered
  - to fill dp arary, check every 2 consecutive chars of the string:
    + if s[i] = ')' && s[i - 1] = '(' => dp[i] = dp[i - 2] + 2
      - ending '()' portion is valid substring anyhow, leading to an increment of 2 in the length of the just previous valid substring's length
    + if s[i] = ')' && s[i - 1] = ')' => if s[i - dp[i - 1] - 1] = '(' => dp[i] = dp[i - 1] + dp[i - dp[i - 1] - 2] + 2
      - if the 2<sup>nd</sup> last ')' was a part of a valid substring (sub<sub>s</sub>), for the last ')' to be a part of a larger substring, there must be a corresponding starting '(' which lies before the valid substring of which the 2nd last ')' is a part (sub<sub>s</sub>) => if the character before sub<sub>s</sub> is '(', update dp[i] as an addition of 2 in the length of sub<sub>s</sub> (i.e. dp[i - 1])and add the length of the valid substring just before sub<sub>s</sub> (i.e. dp[i - dp[i - 1] - 2])

3. O(n) time, O(n) space - Stack
  ```
  public int longestValidParentheses(String s) {
      int maxans = 0;
      Stack<Integer> stack = new Stack<>();
      stack.push(-1);
      for (int = 0; i < s.length(); i++) {
          if (s.charAt(i) == '(') {
              stack.push(i);
          } else {
              stack.pop();
              if (stack.empty()) {
                  stack.push(i);
              } else {
                  maxans = Math.max(maxans, i - stack.peek());
              }
          }
      }
      return maxans;
  }
  ```
  - Use stack while scanning the given string to check if the string scanned so far is valid and to obtain the length of the longest valid string
  - Start by pushing -1 onto the stack
  - For every '(', push its index onto the stack
  - For every ')',
    1. Pop the topmost element
    2. Subtract the current element's index from the top element of the stack == length of the currently encountered valid string of parenthesese
    3. If while popping the element, the stack becomes empty, push the current element's index onto the stack 
    + can keep on calculating the lengths of the valid substrings and return the length of the longest valid string at the end

4. O(2n) time, O(1) space 
  ```
  public int longestValidParentheses(String s) {
      int left = 0, right = 0, maxlength = 0;
      for (int i = 0; i < s.length(); i++) {
          if (s.charAt(i) == '(') {
              left++;
          } else {
              right++;
          }
          if (left == right) {
              maxlength = Math.max(maxlength, 2 * right);
          } else if (right > left) {
              left = right = 0;
          }
      }
      left = right = 0;
      for (int i = s.length() - 1; i >= 0; i++) {
          if (s.charAt(i) == '(') {
              left++;
          } else {
              right++;
          }
          if (left == right) {
              maxlength = Math.max(maxlength, 2 * left);
          } else if (left > right) {
              left = right = 0;
          }
      }
      return maxlength;
  }
  ```
  - Use 2 counters `left` and `right`
  - Traverse from left to right:
    + For every '(', increment `left`
    + For every ')', increment `right`
      - Whenever `left` == `right`, calculate the length of the current valid string and keep track of max length substring so far
      - If `right` > `left`, reset `left` and `right` to 0
  - Traverse string from right to left with similar procedure