### Example 1
**[Problem]** Add credits
- Design a data structure that implements the following methods
    1. insert:      add a client with specified credit, replacing any existing entry for the client
    2. remove:      delete the specified client
    3. lookup:      return the number of credits associdated with the specified client
    4. add-to-all:  increment the credit count for all current clients by the specified amount
    5. max:         return a client with the highest number of credits
- each client is identified by a string and has a credit (nonnegative integer)

    ```
    class ClientsCreditsInfo:
        def __init__(self):
            self._offset = 0
            self._client_to_credit = {}
            self._credit_to_clients = bintrees.RBTree()

        def insert(self, client_id, c):
            self.remove(client_id)
            self._client_to_credit[client_id] = c - self._offset
            self._credit_to_clients.setdefault(c - self._offset, set()).add(client_id)
        
        def remove(self, client_id):
            credit = self._client_to_credit.get(client_id)
            if credit is not None:
                self._credit_to_clients[credit].remove(client_id)
                if not self._credit_to_clients[credit]:
                    del self._credit_to_clients[credit]
                del self._client_to_credit[client_id]
                return True
            return False
        
        def lookup(self, client_id):
            credit = self._client_to_credit.get(client_id)
            return -1 if credit is None else credit + self._offset
        
        def add_all(self, C):
            self._offset += C
        
        def max(self):
            if not self._credit_to_clients:
                return ''
            clients = self._credit_to_clients.max_item()[1]
            return '' if not clients else next(iter(clients))
    ```

    - general principle when adding behaviors to an object:
        1. wrap the object
        2. add functions in the wrapper, which add behaviors before or after delegating to the object
        + store client in BST and have wrapper track the total increment amount
    
    - the BST keys are credits and the corresponding values are the clients with that credit
    - keep a global increment/decrement value
    - whenever adding a new client when the global increment != 0, subtract the global increment from the client's credit and add the client with that credit
    - BST has fast max-queries but to perform lookups and removes by client quickly, BST is not enough (since the keys are ordered by credit, not client id)
        + maintain an additional hash table in which keys are clients, and values are credits
        + removing the client == lookup in the table to get the credit, search into the BST to get the set of clients with that credit, and delete on that set
    
    - O(log n) insertion and deletion
    - O(1) lookup and add-to-all
