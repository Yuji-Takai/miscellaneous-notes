### Example 1
**[Problem]** Implement a circular queue
- Implement queue using a fixed size array
- API should have a constructor (args: initial capacity of the queue), enqueue, dequeue, and size functions
- Dynamically resize to support storing an arbitrarily large number of elements

```
class Queue:
    SCALE_FACTOR = 2
    def __init__(self, capacity):
        self._entries = [None] * capacity
        self._head = self._tail = self._num_queue_elements = 0
    
    def enqueue(self, x):
        # need resizing
        if self._num_queue_elements == len(self._entries):
            self._entries = (self._entries[self._head:] + self._entries[:self_head])
            self._head, self._tail = 0, self._num_queue_elements
            self._entries += [None] * (len(self._entries) * Queue.SCALE_FACTOR - len(self._entries))
        self._entries[self._tail] = x
        self._tail = (self._tail + 1) % len(self._entries)
        self._num_queue_elements += 1
    
    def dequeue(self):
        if not self._num_queue_elements:
            raise IndexError('empty queue')
        self._num_queue_elements -= 1
        result = self._entries[self._head]
        self._head = (self._head + 1) % len(self._entries)
        return result
    
    def size(self):
        return self._num_queue_elements
```
- Use 2 indices (instead of having head always at index 0) to have time complexity of O(1) for dequeue
- Reorder the elements so that they appear contiguously and then resize when performing enqueue on full array
- O(1) dequeue, amortized O(1) enqueue

### Example 2
**[Problem]** Implement a queue using stacks
- Implement a queue given a library implementing stacks

```
class Queue:
    def __init__(self):
        self._enq, self._deq = [], []
    
    def enqueue(self, x):
        self._enq.append(x)
    
    def dequeue(self):
        if not self._deq:
            while self._enq:
                self._deq.append(self._enq.pop())
        if not self._deq:
            raise IndexError('empty queue')
        return self._deq.pop()
```

- enqueue: push the element to be enqueued onto one stack
- dequeue:
    + the element to be dequeued is the element at the bottom of the enqueue stack, which can be accessed by first popping all the elements in enqueue stack and pushing them to another stack, then popping the top of the second stack
    + if we pop the remaining elements back to the first stack, then dequeue will have O(n) time complexity every time
    + solve this problem by keeping all the elements moved to the dequeue stack, pop until it becomes empty and move all the elements from the enqueue stack again 
- O(m) time for m operations

### Example 3
**[Problem]** Implement a Queue with max API
- Implement a queue with enqueue, dequeue, and max operations
- Max operation returns the maximum element currently stored in the queue

1. 
    ```
    def QueueWithMax:
        def __init__(self):
            self._entries = collections.deque()
            self._candidates_for_max = collections.deque()
        
        def enqueue(self, x):
            self._entries.append(x)
            while self._candidates_for_max and self._candidates_for_max[-1] < x:
            self._candidates_for_max.pop()
            self._candidates_for_max.append(x)
        
        def dequeue(self):
            if self._entries:
                result = self._entries.popleft()
                if result == self._candidates_for_max[0]:
                    self._candidates_for_max.popleft()
                return result
            raise IndexError('empty queue')
        
        def max(self):
            if self._candidates_for_max:
                return self._candidates_for_max[0]
            raise IndexError('empty queue')
    ```
    - maintain the set of elements in the queue that have no later entry in the queue greater than them in a separate deque
        + elements in the deque will be ordered by their position in the queue, with the candidate closest to the head of the queue appearing first
        + since each entry in the deque is greater than or equal to its successors, the largest element in the queue is at the head of the deque
    - if the queue is dequeued, and if the element just dequeued is at the deque's head, pop the deque from its head; otherwise the deque remains unchanged
    - when enqueueing, iteratively evict from the deque's tail until the element at the tail is greater than or equal to the entry being enqueued, and then add the new entry to the deque's tail 
    - O(1) for dequeue
    - single enqueue operation may entail many ejections from the deque but the amortized time complexity of n enqueues and dequeues is O(n), since an element can be added and removed from the deque no more than once
    - O(1) for max

2. 
    ```
    class QueueWithMax:
        def __init__(self):
            self._enqueue = Stack()
            self._dequeue = Stack()
        
        def enqueue(self, x):
            self._enqueue.push(x)
        
        def dequeue(self):
            if self._dequeue.empty():
                while not self._enqueue.empty():
                    self._dequeue.push(self._enqueue.pop())
            if not self._dequeue.empty():
                return self._dequeue.pop()
            raise IndexError('Cannot get dequeue() on empty queue')
        
        def max(self):
            if not self._enqueue.empty():
                return self._enqueue.max() if self._dequeue.empty() else max(self._enqueue.max(), self._dequeue.max())
            elif not self._dequeue.empty():
                return self._dequeue.max()
            raise IndexError('Cannot get max() on empty queue')
    ```
    - use reduction
        1. solution for stack-with-max
        2. implementation of queue using 2 stacks
    - use 2 stacks-with-max to implement queue-with-max
    - stack-with-max: amortized O(1) for push, pop, and max
        + amortized O(1) enqueue, dequeue, and max for the queue


