### Example 1
**[Problem]** Reverse all the words in a sentence
- Input: string s containing a set of words separated by whitespace
- Output: original string in which the words appear in the reverse order
  + `"Alice likes Bob" --> "Bob likes Alice"`

1. O(n) time, O(1) space
  ```
  def reverse_words(s):
    s.reverse()
    
    def reverse_range(s, start, finish):
      while start < finish:
        s[start], s[finish] = s[finish], s[start]
        start, finish = start + 1, finish - 1
    
    start = 0
    while True:
      finish = s.find(b' ', start)
      if finish < 0:
        break
      reverse_range(s, start, finish - 1)
      start = finish + 1
    reverse_range(s, start, len(s) - 1)
  ```
  - special case where each word is a single character --> desired result is simply the reverse of s
  - with the special case in mind, reverse s gets the words to their correct relative positions, but for words that are longer than one character, their letters appear in reverse order
    + situation can be corrected by reversing the individual words  