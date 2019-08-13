# Stack
## Python
### Stack
- LIFO 
    + useful for creating reverse iterators for sequences which are stored in a way that would make it difficult or impossible to step back from a given element
    + **[EXAMPLE]** Printing the elements of a SLL in reversed order (O(n) time, O(n) space)
        ```
        def print_linked_list_in_reverse(head):
            nodes = []
            while head:
                nodes.append(head.data)
                head = head.next
            while nodes:
                print(nodes.pop())
        ```

- basic operations
    + push (add)
    + pop (removal)
    + peek (optional, returns the top of the stack without popping it)
- Implementations
    + Linked list implementation
        - O(1) operation for both push and pop
    + Array implementation
        - O(1) operation for both push and pop
        - maximum number of entries that it can have
    + Dynamically resized array implementation
        - amortized O(1) operation for both push and pop

### Basic libraries of Stack
- Some questions require to implement own stack class
- For others, use `list`

| Stack operation | Equivalent python operation |
|-----------------|-----------------------------|
| **Push**        | `s.append(e)`               | 
| **Peek**        | `s[-1]`                     |
| **Pop**         | `s.pop()`                   |
| **isEmpty**     | `len(s) == 0`               |

[NOTE] calling `s[-1]` and `s.pop()` when `s` is an empty list will raise `IndexError` exception


