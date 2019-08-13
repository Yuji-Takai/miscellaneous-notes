### Example 1
**[Problem]** Implement a stack with max API
- Design a stack that includes a max operation, in addition to push and pop
- Method should return the maximum value stored in the stack

1. O(1) implementation for pop, push, and max, O(n) space
    ```
    class Stack:
        ElementWithCachedMax = collections.namedtuple('ElementWithCachedMax', ('element', 'max'))
        def __init__(self):
            self._element_with_cached_max = []
        def empty(self):
            return len(self._element_with_cached_max) == 0
        def max(self):
            if self.empty():
                raise IndexError('max(): empty stack')
            return self._element_with_cached_max[-1].max
        def pop(self):
            if self.empty():
                raise IndexError('pop(): empty stack')
            return self._element_with_cached_max.pop().element
        def push(self, x):
            self._element_with_cached_max.append(self.ElementWithCachedMax(x, x if self.empty() else max(x, self.max())))
    ```
    - Removes the need of recomputing the maximum within the remaining elements after pop
    - Use caching (trading time for space)
        + For each entry in the stack, cache the maximum stored at or below that entry

2. O(1) implementation for pop, push, and max (better average space than Solution 1, O(n) worst, O(1) best)
    ```
    class Stack:
        class MaxWithCount:
            def __init__(self, max, count):
                self.max, self.count = max, count
        def __init__(self):
            self._element = []
            self._cached_max_with_count = []
        def empty(self):
            return len(self._element) == []
        def max(self):
            if self.empty():
                raise IndexError('max(): empty stack')
            return self._cached_max_with_count[-1].max
        def pop(self):
            if self.empty():
                raise IndexError('pop(): empty stack')
            pop_element = self._element.pop()
            current_max = self._cached_max_with_count[-1].max
            if pop_element == current_max:
                self._cached_max_with_count[-1].count -= 1
                if self._cached_max_with_count[-1].count == 0
                    self._cached_max_with_count.pop()
            return pop_element
        def push(self, x):
            self._element.append(x)
            if len(self._cached_max_with_count) == 0:
                self._cached_max_with_count.append(self.MaxWithCount(x, 1))
            else:
                current_max = self._cached_max_with_count[-1].max
                if x == current.max:
                    self._cached_max_with_count[-1].count += 1
                elif x > current_max:
                    self._cached_max_with_count.append(self.MaxWithCount(x, 1))
    ```
    - If an element *e* being pushed is smaller than the maximum element already in the stack, then *e* can never be the maximum, so do not need to record it
    - Cannot store the sequence of maximum values in a separate stack because of the possibility of duplicates
        + Resolve this by additionally recording the number of occurrences of each maximum value
    - Worst-case additional space is O(n)
        + Occurs when each key pushed is greater than all keys in the primary stack
    - When the number of distinct keys is small, or the maximum changes infrequently, the additional space complexity is less, O(1) in the best-case
        
