### Example 1
**[Problem]** Implement an ISBN cache
- Create a cache for looking up prices of books identified by their ISBN (both ISBNs and prices as positive integers)
    + **ISBN**: string of length 10, last character = sum of the first digits, mod 11, with 10 represented by 'X'
- Use Least Recent Used (LRU) policy for cache eviction
    + Insert: if an ISBN is already present, insert should not update the price, but should update that ISBN to be the most recently used entry
    + Lookup: given an ISBN, return the corresponding price; if the element is not present, return -1. If the ISBN is present, update the entry to be the most recently used ISBN
    + Erase: remove the specified ISBN and corresponding value from the case. Return true if the ISBN was present; otherwise return false

1. O(1) lookup
    ```
    class LruCache:
        def __init__(self, capacity):
            self._isbn_price_table = collections.OrderedDict()
            self._capacity = capacity

        def lookup(self, isbn):
            if isbn not in self._isbn_price_table:
                return -1
            price = self._isbn_price_table.pop(isbn)
            self._isbn_price_table[isbn] = price
            return price
        
        def insert(self, isbn, price):
            if isbn in self._isbn_price_table:
                price = self._isbn_price_table.pop(isbn)
            elif len(self._isbn_price_table) == self._capacity:
                self._isbn_price_table.popitem(last=False)
            self._isbn_price_table[isbn] = price
        
        def erase(self, isbn):
            return self._isbn_price_table.pop(isbn, None) is not None
    ```
    - use a hash table to quickly lookup price by using ISBNs as keys and prices as values
    - avoid processing all ISBNs
    - the ISBNs are ordered by when they were most recently used, and only need to be able to find the oldest ISBN efficiently --> record the ISBNs in a queue (in addition to hash table)
        + for each ISBN, store a reference to its location in the queue
        + each time lookup on an ISBN, move to the front of the queue (use linked list implementation of the queue, so that items in the middle of the queue can be moved to the head)
        + do the same when inserting an ISBN that's already present
        + if an insert results in the queue size exceeding n, the ISBN at the tail of the queue is deleted from the cache (i.e. from the queue and the hash table)

### Example 2
**[Problem]** Implement a hash function for chess
- Design a hash function for chess game states 
- Input: state (256 bits), the hash code for that state, and a move
- Output: hash code for the updated state

1. treating the board as a sequence of 64 base 13 digits
    - one digit per square, with the squares numbered from 0 to 63
    - each digit encodes the state of a square
    - hash function sum from 0 to 63 of c<sub>i</sub>p<sup>i</sup> (digit in location i, p: prime number)
    - incremental updates entail computing terms like p<sup>i</sup>

2. creating a random 64-bit integer code for each of the 13 states that each of 64 squares can be in
    - 13 x 64 = 832 random codes 
    - hash code for the state of the chessboard is the XOR of the code for each location
        + updates are very fast
        + maximum number of word-level XORs performed is 4, for captures or a castling