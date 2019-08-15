### Example 1
**[Problem]** Compute the LCA, optimizing for close ancestors
- Input: two nodes in a binary tree (nodes have parent field)
- Output: LCA of the two nodes in the binary tree
    + time complexity should depend only on the distance from the nodes to the LCA

1. O(D0 + D1) time, O(D0 + D1) space (D0: distance from LCA to first node, D1: distance from LCA to second node)
    ```
    def lca(node0, node1):
        iter0, iter1 = node0, node1
        nodes_on_path_to_root = set()
        while iter0 or iter1:
            if iter0:
                if iter0 in nodes_on_path_to_root:
                    return iter0
                nodes_on_path_to_root.add(iter0)
                iter0 = iter0.parent
            if iter1:
                if iter1 in nodes_on_path_to_root:
                    return iter1
                nodes_on_path_to_root.add(iter1)
                iter1 = iter1.parent
        raise ValueError('node0 and node1 are not in the same tree')
    ```
    - alternate moving upwards from the 2 nodes and store the nodes visited as moving up in a hash table
    - each time visit a node , check to see if it has been visited before

### Example 2
**[Problem]** Find the nearest repeated entries in an array
- Input: an array of strings
- Output: distance between a closest pair of equal entries 
    + [Example] `["All", "work", "and", "no", "play", "make", "for", "no", "work", "no", "fun", "and", "no", "results"] --> second and third occurrence of "no" --> 2`

1. O(n) time, O(d) space (d: number of distinct entries in the array)
    ```
    def find_nearest_repetition(paragraph):
        word_to_latest_index, nearest_repeated_distance = {}, float('inf')
        for i, word in enumerate(paragraph):
            if word in word_to_latest_index:
                latest_equal_word = word_to_latest_index[word]
                nearest_repeated_distance = min(nearest_repeated_distance, i - latest_equal_word)
            word_to_latest_index[word] = i
        return nearest_repeated_distance if nearest_repeated_distance != float('inf') else -1
    ```
    - when processing the array, the closest previous equal entry only matters
    - as scan through the array, for each value seen so far, store in a hash table the latest index at which it appears
    - when processing the element, use the hash table to see the latest index less than the current index holding the same value

### Example 3
**[Problem]** Find the smallest subarray covering all values 
- Input: array of strings and set of strings
- Output: indices of the starting and ending index of a shortest subarray of the given array that contains all strings in the set

1. O(n) time (n: length of array)
    ```
    Subarray = collections.namedtuple('Subarray', ('start', 'end'))

    def find_smallest_subarray_covering_set(paragraph, keywords):
        keywords_to_cover = collections.Counter(keywords)
        result = Subarray(-1, -1)
        remaining_to_cover = len(keywords)
        left = 0
        for right, p in enumerate(paragraph):
            if p in keywords:
                keywords_to_cover[p] -= 1
                if keywords_to_cover[p] >= 0:
                    remaining_to_cover -= 1
            
            while remaining_to_cover == 0:
                if result == (-1, -1) or right - left < result[1] - result[0]:
                    result = (left, right)
                pl = paragraph[left]
                if pl in keywords:
                    keywords_to_cover[pl] += 1
                    if keywords_to_cover[pl] > 0:
                        remaining_to_cover += 1
                left += 1
        return result
    ```
    - use hashtable to record which strings in the set remain to be covered
        + each time increment the subarray length, need O(1) time to update the set of remaining strings
    - use linked list
    - when moving form i to i + 1, can reuse the work performed from i
        + if the smallest subarray starting at i covering the set ends at j, there is no point in considering subarrays starting at i + 1 and ending before j, since know they cannot cover the set
        + when advance to i + 1, either still cover the set, or have to advance j to cover the set
        + continuously advance one of i or j == O(n) time 

2. O(n) time
    ```
    def find_smallest_subarray_covering_set(paragraph, keywords):
        class DoublyLinkedListNode:
            def __init__(self, data=None):
                self.data = data
                self.next = self.prev = None
        class LinkedList:
            def __init__(self):
                self.head = self.tail = None
                self._size = 0
            
            def __len__(self):
                return self._size
            
            def insert_after(self, value):
                node = DoublyLinkedListNode(value)
                node.prev = self.tail
                if self.tail:
                    self.tail.next = node
                else:
                    self.hand = node
                self.tail = node
                self_size += 1
            
            def remove(self, node):
                if node.next:
                    node.next.prev = node.prev
                else:
                    self.tail = node.prev
                if node.prev:
                    node.prev.next = node.next
                else:
                    self.head = node.next
                node.next = node.prev = None
                self._size -= 1
        
        loc = LinkedList()
        d = {s: None for s in keywords}
        result = Subarray(-1, -1)
        for idx, s in enumerate(paragraph):
            if s in d:
                it = d[s]
                if it is not None:
                    loc.remove(it)
                loc.insert_after(idx)
                d[s] = loc.tail
                
                if len(loc) == len(keywords):
                    if (result == (-1, -1) or idx - loc.head.data < result[1] - result[0]):
                        result = (loc.head.data, idx)
        return result
    ``` 
    - streaming algorithm
        + keep track of latest occurrences of query keywords as process A
        + use a doubly linked list L to store the last occurrence (index) of each keyword in Q, and hash table H to map each keyword in Q to the corresponding node in L
        + each time a word in Q is encountered, remove its node from L (which find by using H), create a new node which records the current index in A, and append the new node to the end of L + also update H
            - by doing so, each keyword in L is ordered by its order in A; therefore, if L has n<sub>Q</sub> words (all keywords are shown) and the current index minus the index stored in the first node in L is less than current best, update current best


### Example 4
**[Problem]** Find the smallest subarray sequentially covering all values 
- Input: 2 arrays of strings
- Output: starting and ending indices of a shortest subarray of the first of the first array ("paragraph") that contains all the strings in the second array ("keywords") in the order in which they appear in the keywords array
    + keywords are all distinct

1. O(n) time, O(m) space (n: length of paragraph array, m: number of keywords)
    ```
    Subarray = colletions.namedtuple('Subarray', ('start', 'end'))

    def find_smallest_sequentially_covering_subset(paragraph, keywords):
        keyword_to_idx = {k: i for, k in enumerate(keywords)}

        last_occurrence = [-1] * len(keywords)

        shortest_subarray_length = [float('inf')] * len(keywords)

        shortest_distance = float('inf')
        result = Subarray(-1, -1)
        for i, p in enumerate(paragraph):
            if p in keyword_to_idx:
                keyword_idx = keyword_to_idx[p]
                if keyword_idx == 0:
                    shortest_subarray_length[keyword_idx] = 1
                elif shortest_subarrray_length[keyword_idx - 1] != float('inf'):
                    distance_to_previous_keyword = i - latest_occurrence[keyword_idx - 1]
                    shortest_subarray_length[keyword_idx] = distance_to_previous_keyword + shortest_subarray_length[keyword_idx - 1]
                last_occurrence[keyword_idx] = i

            if (keyword_idx == len(keywords) - 1 and shortest_subarray_length[-1] < shortest_distance):
                shortest_distance = shortest_subarray_length[-1]
                result = Subarray(i - shortest_distance + 1, i)
        return result
    ```
    - use
        1. a hash table mapping keywords to their most recent occurrences in the paragraph array as iterate through it
        2. a hash table mapping each keyword to the length of the shortest subarray ending at the most recent occurrence of that keyword
        + these 2 hash tables give the ability to determine the shortest subarray sequentially covering the first k keywords given the shortest subarray sequentially covering the first k - 1 keywords
    - when processing the *i*th string in the paragraph array, if that string is the *j*th keyword, update the most recent occurrence of that keyword to *i*
    - the shortest subarray ending at *i* which sequentially covers the first *j* keywords consists of the shortest subarray ending at the most recent occurrence of the first *j* - 1 keywords plus the elements from the most recent occurrence of the (*j* - 1)th keyword to *i*
    
### Example 5
**[Problem]** Find the longest subarray with distinct entries 
- Input: array of strings
- Output: length of a longest subarray with the property that all its elements are distinct
    + [Example] `[f, s, f, e, t, w, e, n, w, e] --> [s, f, e, t, w]`

1. O(n) time
    ```
    def longest_subarray_with_distinct_entries(A):
        most_recent_occurrence = {}
        longest_dup_free_subarray_start_idx = result = 0
        for i, a in enumerate(A):
            if a in most_recent_occurrence:
                dup_idx = most_recent_occrrence[a]
                if dup_idx >= longest_dup_free_subarray_start_idx:
                    result = max(result, i - longest_dup_free_subarray_start_idx)
                    longest_dup_free_subarray_start_idx = dup_idx + 1
            most_recent_occurrence[a] = i
        return max(result, len(A) - longest_dup_free_subarray_start_idx)
    ```
    - if know the longest duplicate-free subarray ending at a given index, the longest duplicate-free subarray ending at the next index is either 
        1. the previous subarray appended with the element at the next index, if that element does not appear in the longest duplicate-free subarray at the current index OR
        2. the subarray beginning at the most recent occurrence of the element at the next index to the next index
        + need a hash table storing the most recent occurrence of each element, and the longest duplicate-free subarray ending at the current element

### Example 6 
**[Problem]** Find the length of a longest contained interval
- Input: array of integers
- Output: size of a largest subset of integers in the array having the property that if 2 integers are in the subset, then so are all integers between them
    + [Example] `[3, -2, 7, 9, 8, 1, 2, 0, -1, 5, 8] --> [-2, -1, 0, 1, 2, 3] --> return 6`

1. O(n) time
    ```
    def longest_contained_range(A):
        unprocessed_entries = set(A):
        max_interval_size = 0
        while unprocessed_entries:
            a = unprocessed_entries.pop()
            lower_bound = a - 1
            while lower_bound in unprocessed_entries:
                unprocessed_entries.remove(lower_bound)
                lower_bound -= 1
            upper_bound = a + 1
            while upper_bound in unprocessed_entries:
                unprocessed_entries.remove(upper_bound)
                upper_bound += 1
            max_interval_size = max(max_interval_size, upper_bound - lower_bound - 1)
        
        return max_interval_size
    ```
    - do NOT need total ordering - all what maters is whether the integers adjacent to a given value are present
        + use hash table to store the entries
        + iterate over the entries in the array:
            1. if an entry *e* is present in the hash table, compute the largest interval including *e* such that all values in the interval are contained in the hash table 
            2. compute the largest interval by iteratively searching entries in the hash table of the form e + 1, e + 2, ..., and e - 1, e - 2, ...
            3. when done, to avoid duplicated computation, remove all the entries in the computed interval from the hash table, since all these entries are in the same largest contained interval
     
### Example 7 
**[Problem]** Compute all string decompositions
- Input: a string ("sentence") and an array of strings ("words")
- Output: starting indices of substrings of the sentence string which are the concatenation of all the strings in the words array
    + each string must appear exactly once, and their ordering is immaterial 
    + all strings in the words array have equal length
    + possible for the words array to contain duplicates 

1. O(Nnm) time (N: length of the sentence, m: number of words, n: length of each word)
    ```
    def find_all_substrings(s, words):
        def match_all_words_in_dict(start):
            curr_string_to_freq = collections.Counter()
            for i in range(start, start + len(words) * unit_size, unit_size):
                curr_word = s[i:i + unit_size]
                it = word_to_freq[curr_word]
                if it == 0:
                    return False
                curr_string_to_freq[curr_word] += 1
                if curr_string_to_freq[curr_word] > it:
                    return False
            return True
        
        word_to_freq = collection.Counter(words)
        unit_size = len(words[0])
        return [i for i in range(len(s) - unit_size * len(words) + 1) if match_all_words_in_dict(i)]
    ```
    - recursively check whether a string is the concatentation of strings in words
        + since all strings in words have equal length, only one distinct string in words can be a prefix of the given string
        + can directly check the first n characters of the string to see if they are in words
        + if not, the string cannot be the concatenation of words
        + if it is, remove that string from words and continue with the remainder of the string and the remaining words
    - to find substrings in the sentence string that are concatenation of the strings in words, use the above process for each index in the sentence as the starting index
