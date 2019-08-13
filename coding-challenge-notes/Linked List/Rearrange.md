### Example 1
**[Problem]** Merge 2 sorted lists
- Input: 2 sorted linked lists
- Output: Single linked list resulting from merging the 2 lists

1. O(n) time, O(1) space
    ```
    def merge_two_sorted_lists(L1, L2):
        dummy_head = tail = ListNode()
        while L1 and L2:
            if L1.data < L2.data:
                tail.next, L1 = L1, L1.next
            else:
                tail.next, L2 = L2, L2.next
        tail.next = L1 or L2
        return dummy_next.next
    ```

### Example 2
**[Problem]** Reverse a single sublist
- Input: Singly linked list L, 2 integers s and f
- Output:  L with order of the nodes from the sth node to fth node (inclusive) reversed
    + Numbering begins at 1 (head node is the 1st node)
    + Do not use additional nodes

1. O(f) time, O(1) space (single pass approach)
    ```
    def reverse_sublist(L, start, finish):
        dummy_head = sublist_head = ListNode(0, L)
        for _ in range(1, start):
            sublist_head = sublist_head.next
        
        sublist_iter = sublist_head.next
        for _ in range(finish - start):
            temp = sublist_iter.next
            sublist_iter.next, temp.next, sublist_head.next = (temp.next, sublist_head.next, temp)
        
        return dummy_head.next

    ```

### Example 3
**[Problem]** Implement cyclic right shift for SLL
- Input: a SLL and a nonnegative integer k
- Output: list cyclically shifted to the right by k

1. O(n) time, O(1) space
    ```
    def cyclically_right_shift_list(L, k):
        if not L:
            return L
        
        tail, n = L, 1
        while tail.next:
            n += 1
            tail = tail.next
        
        k %= n
        if k == 0:
            return L
        tail.next = L
        steps_to_new_head, new_tail = n - k, tail
        while steps_to_new_head:
            steps_to_new_head -= 1
            new_tail = new_tail.next
        new_head = new_tail.next
        new_tail.next = None
        return new_head
    ```
    - linked lists can be cut and the sublists can be reassembled very efficiently
    1. find the tail node *t*
    2. update t's successor as the original head
        - the original head is to become the kth node from the start of the new list
    3. new head is the (n - k)th node in the initial list

### Example 4
**[Problem]** Implement even-odd merge
- Input: SLL
- Output: original list after undergoing even-odd merge
    + **Even-odd merge**: list consisting of the even-numbered nodes followed by the odd-numbered nodes
    + `L -> l0 -> l1 -> l2 -> l3 -> l4 ==> L -> l0 -> l2 -> l4 -> l1 -> l3`

1. O(n) time, O(1) space
    ```
    def even_odd_merge(L):
        if not L:
            return L
        even_dummy_head, odd_dummy_head = ListNode(0), ListNode(0)
        tails, turn = [even_dummy_head, odd_dummy_head], 0
        while L:
            tails[turn].next = L
            L = L.next
            tails[turn] = tails[turn].next
            turn ^= 1 # alternate between even and odd
        tails[1].next = None
        tails[0].next = odd_dummy_head.next
        return even_dummy_head.next
    ```
    - avoid extra space using existing list nodes
        + iterate through the list and append even elements to one list and odd elements to another list
    - use indicator variable to tell which list to append to
    - append the odd list to the even list


### Example 5
**[Problem]** Implement list pivoting
- Input: SLL and integer k
- Output: original SLL after list pivoting
    + **Pivot of a list of integers wrt k**: list with its node reordered so that all nodes containing keys less than *k* appear before nodes containing *k*, and all nodes containing keys greater than *k* appear after the nodes containing *k*
    + **[EXAMPLE]** ` L->3->2->2->11->7->5->11 && k = 7 ==> L->3->2->2->5->7->11->11`

1. O(n) time, O(1) space
    ```
    def list_pivoting(l, x):
        less_head = less_iter = ListNode()
        equal_head = equal_iter = ListNode()
        greater_head = greater_iter = ListNode()

        while l:
            if l.data < x:
                less_iter.next = l
                less_iter = less_iter.next
            elif l.data == x:
                equal_iter.next = l
                equal_iter = equal_iter.next
            else:
                greater_iter.next = l
                greater_iter = greater_iter.next
            l = l.next
        
        greater_iter.next = None
        equal_iter.next = greater_head.next
        less_iter.next = equal_head.next
        return less_head.next
    ```

### Example 6
**[Problem]** Add list-based integers
- Input: 2 SLLs of digits (LSD comes first)
- Output: list representing corresponding to the sum of the integers the input lists represent

1. O(n + m) time, O(max(n, m)) space (n, m: lengths of the lists)
    ```
    def add_two_numbers(L1, L2):
        place_iter = dummy_head = ListNode()
        carry = 0
        while L1 or L2 or carry:
            val = carry + (L1.data if L1 else 0) + (L2.data if L2 else 0)
            L1 = L1.next if L1 else None
            L2 = L2.next if L2 else None
            place_iter.next = ListNode(val % 10)
            carry, place_iter = val // 10, place_iter.next
        return dummy_head.next

    ```
