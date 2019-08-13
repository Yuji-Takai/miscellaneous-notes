### Example 1
**[Problem]** Test a string over "[,],(,),{,}" for well-formedness
- Input: str made up of characters "[" , "]", "(", ")", "{", "}"
- Output: if the str is well-formed

1. O(n) time, O(n) space
    ```
    def is_well_formed(s):
        left_chars, LOOKUP = [], {'(': ')', '{': '}', '[': ']'}
        for c in s:
            if c in LOOKUP:
                left_chars.append(c)
            elif not left_chars or LOOKUP[left_chars.pop()] != c:
                return False
        return not left_chars
    ```
    - use stack to record the unmatched left characters with the most recent one at the top
    - if encounter a right character and the stack is empty or the top of the stack is a different type of left character, the right character is not matched, implying the string is not matched
    - if all characters have been processed and the stack is nonempty, there are unmatched left characters so the string is not matched
