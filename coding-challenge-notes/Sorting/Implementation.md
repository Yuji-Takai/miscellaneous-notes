### Example 1
**[Problem]** Implement a fast sorting algorithm for lists 
- Implement a stable sort that sorts linked lists efficiently

1. O(n<sup>2</sup>) time (worst case: reverse sorted)
    ```
    def insertion_sort(L):
        dummy_head = ListNode(0, L)
        while L and L.next:
            if L.data > L.next.data:
                target, pre = L.next, dummy_head
                while prev.next.data < target.data:
                    pre = pre.next
                temp, pre.next, L.next = pre.next, target, target.next
                target.next = temp
            else:
                L = L.next
        retun dummy_head.next
    ```

2. O(n log n) time, O(log n) space (= max function call stack depth)
    ```
    def stable_sort_list(L):
        if not L or not L.next:
            return L
        pre_slow, slow, fast = None, L, L
        while fast and fast.next:
            pre_slow = slow
            fast, slow = fast.next.next, slow.next
        pre_slow.next = None # splits the list into two equal sized lists
        return merge_two_sorted_lists(stable_sort_list(L), stable_sort_list(slow))
    ```
    - unlike arrays, lists can be merged in-place
        + insertion to the middle of a list is O(1) time
    - merge sort on linked list
        1. decompose the list into 2 equal-sized sublists around the node in the middle of the list
            - find this middle node by advancing two iterators through the list, one twice as fast as the other
            - when the fast iterator reaches the end of the list, the slow iterator is at the middle of the list
        2. recurse on the sublists