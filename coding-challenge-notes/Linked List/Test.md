### Example 1
**[Problem]** Test for cyclicity
- Input: Head of a singly linked list
- Output:  null if there does not exist a cycle, else the node at the start of the cycle 

1. O(n) time, O(1) space
    ```
    def has_cycle(head):
        def cycle_len(end):
            start, step = end, 0
            while True:
                step += 1
                start = start.next
                if start is end:
                    return step

        fast = slow = head
        while fast and fast.next and fast.next.next:
            slow, fast = slow.next, fast.next.next
            if slow is fast:
                cycle_len_advanced_iter = head
                for _ in range(cycle_len(slow)):
                    cycle_len_advanced_iter = cycle_len_advanced_iter.next
                it = head
                while it is not cycle_len_advanced_iter:
                    it = it.next
                    cycle_len_advanced_iter = cycle_len_advanced_iter.next
                return it
        return None
    ```
    - slow iterator and fast iterator
    - in each iteration, advance the slow iterator by one and the fast iterator by two 
        + list has a cycle if and only if the two iterators meet
        + if the fast iterator jumps over the slow iterator, the slow iterator will equal the fast iterator in the next step
    - after detecting a cycle, find the start of the cycle, by first calculating the cycle length C
    - after finding the cycle length, find the first node on the cycle by using two iterators that are C apart from each other
    - advance the two in tandem, and when they meet, that node must be the first node on the cycle

### Example 2
**[Problem]** Test for overlapping lists - lists are cycle free
- Input: 2 cycle-free SLL (there may be list nodes that are common to both)
- Output: the first overlapping node (or null if no overlapping)

1. O(n) time, O(1) space
    ```
    def overlapping_no_cycle_lists(l0, l1):
        def length(L):
            length = 0
            while L:
                length += 1
                L = L.next
            return length
        l0_len, l1_len = length(l0), length(l1)
        if l0_len > l1_len:
            l0, l1 = l1, l0
        for _ in range(abs(l0_len - l1_len)):
            l1 = l1.next
        
        while l0 and l1 and l0 is not l1:
            l0, l1 = l0.next, l1.next
        return l0
    ```
    - The lists overlap iff both have the same tail node 
        + once the lists converge at a node, they cannot diverge at a later node
        + checking for overlap amounts to finding the tail nodes for each list
    - To find the first overlapping node: 
        1. compute the length of each list
        2. find the first overlapping node by advancing through the longer list by the difference in length, and then advancing through both lists in tandem, stopping at the first common node
        3. If end reached without finding a common node, the lists do not overlap

### Example 3
**[Problem]** Test for overlapping lists - lists may have cycles
- Input: 2 SLL (two lists may each or both have a cycle)
- Output: the first overlapping node 
    + the node may not be unique - if one node ends in a cycle, the first cycle node encountered when traversing it may be different from the first cycle node encountered when traversing the second list, even though the cycle is the same
    + in such cases, may return either of the 2 nodes

1. O(n) time, O(1) space (*n*: total number of nodes in the 2 input lists)
    ```
    def overlapping_lists(l0, l1):
        root0, root1 = has_cycle(l0), has_cycle(l1) # store the start of cycle if any
        if not root0 and not root1:
            return overlapping_no_cycle_lists(l0, l1) # from example 2
        elif (root0 and not root1) or (not root0 and root1):
            return None # one has cycle and other does not
        temp = root1
        while True:
            temp = temp.next
            if temp is root0 or temp is root1:
                break
        
        if temp is not root0:
            return None # cycles are disjoint, l0 and l1 do not end in the same cycle
        def distance(a, b):
            dis = 0
            while a is not b:
                a = a.next
                dis += 1
            return dis
        
        # l0 and l1 end in the same cycle
        # locate the overlapping node if they first overlap before cycle starts
        stem0_length, stem1_length = distance(l0, root0), distance(l1, root1)
        if stem0_length > stem1_length:
            l1, l0 = l0, l1
            root1, root0 = root0, root1
        for _ in range(abs(stem0_length - stem1_length)):
            l1 = l1.next
        while l0 is not l1 and l0 is not root0 and l1 is not root1:
            l0, l1 = l0.next, l1.next
        
        # if l0 == l1 before reaching root0 == overlap first occurs before the cycle starts
        # otherwise, the first overlapping node is not unique, can return any node on the cycle
        return l0 if l0 is l1 else root0
    ```
    1. Case: neither list is cyclic --> Example 2
    2. Case: one list is cyclic and the other is not --> cannot overlap 
    3. Case: both cyclic --> if overlap, must be identical
        1. Sub case: the paths to the cycle merge before the cycle, in which case there is a unique first node that is common
        2. Sub case: the paths reach the cycle at different nodes on the cycle





### Example 4
**[Problem]** Test whether a SLL is palindromic
- Input: a SLL
- Output: whether the SLL is palindromic

1. O(n) time, O(1) space
    ```
    def is_linked_list_a_palindrome(L):
        slow = fast = L
        while fast and fast.next:
            fast, slow = fast.next.next, slow.next
        first_half_iter, second_half_iter = L, reverse_linked_list(slow)
        while second_half_iter and first_half_iter:
            if second_half_iter.data != first_half_iter.data:
                return False
            second_half_iter, first_half_iter = (second_half_iter.next, first_half_iter.next)
        return True
    ```
    - getting first node in a SLL = O(1) time operation
        + pay a one-time cost of O(n) time complexity to get the reverse of the second half of the original list, after which testing palindromicity of the original list reduces to testing if the first half and the reversed second half are equal
    - this approach changes the list passed in, but the reveresed sublist can be reversed again to restore the original list