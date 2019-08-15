# Hash table
## Python
### Hash table
- store keys, optionally, with corresponding values
- inserts, deletes, and lookups run in O(1) time on average
- store keys in an array
    + a key is stored in the array locations based on its hash code
    + hash code is an integer computed from the key by a hash function
    + if the hash function is chosen well, the objects are distributed uniformly accross the array location
- **collision**: when 2 keys map to the same location
    + one way of dealing with collision: linked list of objects at each array location
        - average time for lookups, insertions, and deletions is O(1 + n/m) (n: number of objects, m: length of the array)
        - if the load n/m grows large, rehashing can be applied 
            + a new array with a larger number of locations is allocated, and the objects are moved to the new array
            + rehashing is expensive(O(n + m) time) but if done infrequently, its amortized cost is low
- avoid mutuable objects as keys
    + when updating a key, first remove it, then update it, and finally add it back
- hash function
    + hard requirement: equal keys should have equal hash codes
    + soft requirement: hash function should spread keys == the hash codes for a subset of objects should be uniformly distributed across the underlying array 
    + Example hash function for strings
        ```
        def string_hash(s, modulus):
            MULT = 997
            return functools.reduce(lambda v, c: (v * MULT + ord(c)) % modulus, s, 0)
        ```
        - should examine all the characters in the string 
        - should give a large range of values
        - should not let one character dominate
        - rolling hash function is desired == if a character is deleted from the front of the string, and another added to the end, the new hash code can be computed in O(1) time

- hash table is a good data structure to represent a dictionary (set of strings)
- hash tables have the **best theoretical and real-world performance** for lookup O(1), insert (amortized O(1)), and delete (O(1))
- consider using a hash code as signature to enhance performance == filter out candidates
- consider using a precomputed `lookup` table instead of boilerplate if-then code for mappings (ex: character -> value, character -> character)
- `multimap`: map that contains multiple values for a single key
- bi-driectional map

### Example
**[Problem]** Anagram
- Input: set of words
- Output: groups of anagrams from those words (each group must contain at least 2 words)

1. O(nm log m) time
    ```
    def find_anagrams(dictionary):
        sorted_string_to_anagrams = collections.defaultdict(list)
        for s in dictionary:
            sorted_string_to_anagrams[''.join(sorted(s))].append(s)
        return [group for group in sorted_string_to_anagrams.values() if len(group) >= 2]
    ```
    - map strings to a representative
    - given any string, its sorted version can be used as a unique identifier for the anagram group it belongs to
        + want a map from a sorted string to the anagrams it corresponds to
        + use map with sorted strings as keys and values as arrays of corresponding strings from the original input


### Python libraries
- hash table-based data structures used in Python:
    1. stores keys
        + `set`
    2. stores key-value pairs
        + `dict`
        + `collections.defaultdict`
        + `collections.Counter`
    + all do not allow duplicate keys

- in `dict`, accessing value associated with a key that is not present leads to a `KeyError` exception
- `collections.defaultdict` returns the default value of the type that was specified when the collection was instantiated
    + if `d = collections.defaultdict(list)`, then if `k not in d` then `d[k]` is `[]`
- `collections.Counter` is used for counting the number of occurrences of keys, with a number of set-like operations
    ```
    c = collections.Counter(a=3, b=1)
    d = collections.Counter(a=1, b=2)
    
    # add 2 counters together: c[x] + d[x], collections.Counter({'a': 4, 'b': 3})
    c + d
    
    # subtract (keep only positive counts), collections.Counter({'a': 2})
    c - d

    # intersection: min(c[x], d[x]), collections.Counter({'a': 1, 'b': 1})
    c & d

    # union: max(c[x], d[x]), collections.Counter({'a': 3, 'b': 2})
    c | d
    ```
- `set` operations
    | function              | description                                       |
    |-----------------------|---------------------------------------------------|
    | `s.add(x)`            | insert x to set s                                 |
    | `s.remove(x)`         | remove the first occurrence of x in s             |
    | `s.discard(x)`        | remove element x from s if present                |
    | `x in s`              | True if an item of s is equal to x, else False    |

- iterations for key-pair data structures:
    1. `items()`: iterate over the key-value pairs
    2. `values()`: iterate over the values
    3. `keys()`: iterate over the keys

- *mutable* containers are not hashable --> cannot be added to a `set` or used as a key in a `dict`