### Example 1
**[Problem]** Test for palindromic permutations
- Input: string
- Output: true if letters forming the string can be permuted to form a palindrome

1. O(n) time, O(c) space (c: number of distinct characters appearing in the string)
    ```
    def can_form_palindrome(s):
        return sum(v % 2 for v in collections.Counter(s).values()) <= 1
    ```
    - all characters must occur in pairs for a string to be permutable into a palindrome, with one exception == when the string is of odd length
        + if the string is of even length, a necessary and sufficient condition for it to be a palindrome is that each character in the string appears an even number of times
        + if the length is odd, all but one character should appear an even number of times
            - both cases can be covered by testing that *at most* one character appears an odd number of times, which can be checked using a hash table mapping characters to frequencies

### Example 2
**[Problem]** Is an anonymous letter constructible? 
- Input: text for an anonymous letter and text for a magazine
- Output: true it it is possible to write the anonymous letter using the magazine
    + the anonymous letter can be written using the magazine if for each character in the anonymous letter, the number of times it appears in the anonymous letter is no more than the number of times it appears in the magazine

1. O(n + m) time, O(L) space (L: number of distinct characters appearing in the letter)
    ```
    def is_letter_constructible_from_magazine(letter_text, magazine_text):
        char_frequency_for_letter = collections.Counter(letter_text)
        for c in magazine_text:
            if c in char_frequency_for_letter:
                char_frequency_for_letter[c] -= 1
                if char_frequency_for_letter[c] == 0:
                    del char_frequency_for_letter[c]
                    if not char_frequency_for_letter:
                        return True
        return not char_frequency_for_letter
    ```
    1. make a single pass over the letter, storing the character counts for the letter in a single hash table 
        + keys = characters & values = number of times that character appears
    2. make a pass over the magazine
        + when processing a character *c*, if *c* appears in the hash table, reduce its count by 1
        + remove the character from the hash when its count goes to 0
        + if the hash becomes empty, return true
    3. if reach the end of the magazine and the hash is nonempty, return false == each of the characters remaining in the hash occurs more times in the letter than the magazine

2. O(n + m) time - Python
    ```
    def is_letter_constructible_from_magazine(letter_text, magazine_text):
        return (not collections.Counter(letter_text) - collections.Counter(magazine_text))
    ```

- [NOTE] if the characters are coded in ASCII, can use a 256 entry integer array A, with A[i] being set to the number of times the character i appears in the letter

### Example 3
**[Problem]** Test the Collatz conjecture 
- Input: int n
- Output: true if all first n positive integers satisfy Collatz conjecture otherwise false 
    + **Collatz conjecture**: 
        1. take any natural number
            - if it is odd, triple it and add one
            - if it is even, halve it
        2. repeat the process indefinitely --> no matter what number, will eventually arrive at 1

1. O(???) time
    ```
    def test_collatz_conjecture(n):
        verified_numbers = set()

        for i in range(3, n + 1):
            sequence = set()
            test_i = i
            while test_i >= i:
                if test_i in sequence:
                    return False
                sequence.add(test_i)
                if test_i % 2:  # odd number
                    if test_i in verified_numbers:
                        break
                    verified_numbers.add(test_i)
                    test_i = 3 * test_i + 1
                else:
                    test_i // 2
        return True
    ```
    - Collatz hypothesis can fail in 2 ways = a sequence returns to a previous number in the sequence
        1. implying loop forever OR
        1. sequence goes to infinity (overflows)
    - iterate through all numbers and for each number repeatedly apply the rules till reach 1
    - to accelerate the check:
        1. reuse computation by storing all the numbers proven to converge to 1; that way, as soon as such a number is reached, can assume it would reach 1
        2. to save time, skip even numbers (since they are immediately halved, and the resulting number must have already been checked)
        3. if have tested every number up to *k*, can stop the chain as soon as reach a number that is less than or equal to *k* --> do NOT need to store the numbers below *k* in the hash table
        4. if multiplication and division are expensive, use bit shifting and addition
        5. partition the search set and use many computers in parallel to explore the subsets

