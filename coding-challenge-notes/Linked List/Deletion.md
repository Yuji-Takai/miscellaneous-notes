### Example 1
**[Problem]** Delete a node from a SLL
- Input: Node (not tail) within a SLL 
- Output: Delete the node from the SLL

1. O(1) time, O(1) space
    ```
    def deletion_from_list(node_to_delete):
        node_to_delete.data = node_to_delete.next.data
        node_to_delete.next = node_to_delete.next.next
    ```
    1. copy the value part of the next node to the current node
    2. delete the next node
        - effectively deleted the current node in O(1)

### Example 2
**[Problem]** Remove the kth last element from a list
- Input: a SLL and an integer *k*
- Output: original SLL with kth last element removed 
    + Solve with O(1) space

1. O(n) time, O(1) space (1 pass)
    ```
    def remove_kth_last(L, k):
        dummy_head = ListNode(0, L)
        first = dummy_head.next
        for _ in range(k):
            first = first.next
        second = dummy_head
        while first:
            first, second = first.next, second.next
        second.next = second.next.next
        return dummy_head.next
    ```
    - use 2 iterators to traverse the list
        + the first iterator is advanced by k steps
        + the 2 iterators advance in tandem
    - when the first iterator reaches the tail, the second iterator is at the (k+1) th last node, and can remove the kth node

### Example 3
**[Problem]** Remove duplicates from a sorted list
- Input: SLL of integers in sorted order
- Output: original list with duplicates removed

1. O(n) time, O(1) space
    ```
    def remove_duplicates(L):
        it = L
        while it:
            next_distinct = it.next
            while next_distinct and next_distinct.data = it.data:
                next_distinct = next_distinct.next
            it.next = next_distinct
            it = next_distinct
        return L
    ```
    - remove all successive nodes with the same value as the current node
    