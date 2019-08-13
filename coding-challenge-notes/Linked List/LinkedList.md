# Linked List  
## Python
### Linked List vs Array 
- both contain objects in a linear order
- insertion and deletion
    1. Linked list: O(1)
    2. Array: O(n)
- access of kth element:
    1. Linked list: O(n)
    2. Array: O(1)

### Prototype of Linked List Node
    ```
    class ListNode:
        def __init__(self, data=0, next=None):
            self.data = data
            self.next = next
    ```

### Basic operations of Linked List
1. Search for a key - O(n)
    ```
    def search_list(L, key):
        while L and L.data != key:
            L = L.next
        return L
    ```

2. Insert a new node after a specified node - O(1)
    ```
    def insert_after(node, new_node):
        new_node.next = node.next
        node.next = new_node
    ```

3. Delete a node - O(1)
    ```
    def delete_after(node):
        node.next = node.next.next
    ```

### Tips
- consider using dummy head to avoid having to check for empty lists
