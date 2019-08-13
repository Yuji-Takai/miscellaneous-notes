# Queue
## Python
### Queue
- FIFO
- basic operations
    + enqueue (add)
    + dequeue (removal)
- Implementations
    + Linked list implementation
        - O(1) operation for both enqueue and dequeue
    + Array implementation


- Implementation using composition 
    + add a private field that references a library queue object
    + O(1) operation for enqueue and dequeue
    + O(n) operation for max
    ```
    class Queue:
        def __init__(self):
            self._data = collections.deque()
        def enqueue(self, x):
            self._data.append(x)
        def dequeue(self):
            return self._data.popleft()
        def max(self):
            return max(self._data)
    ```


### Deque
- Double-ended queue
- Doubly linked list in which all insertions and deletions are from one of the two ends of the list (i.e. at the head or the tail)
- **push**: insertion to the front
- **inject**: insertion to the back
- **pop**: deletion from front
- **eject**: deletion from back


### Basic libraries of Queue
- Some questions require to implement own queue class
- For others, use `collections.deque`

| Queue operation | Equivalent python operation |
|-----------------|-----------------------------|
| **Enqueue**     | `q.append(e)`               | 
| **Peek**        | `q[0]`                      |
| **Dequeue**     | `s.popleft()`               |

[NOTE] dequeing or accessing the head/tail of an empty collection results in `IndexError` exception being raised



