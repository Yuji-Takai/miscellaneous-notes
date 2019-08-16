### Example 1
**[Problem]** Computing the h-index 
- Input: array of positive integers
- Output: the largest h such that there are at least h entries in the array that are greater than or equal to h

1. O(n log n) time, O(1) space
    ```
    def h_index(citations):
        citations.sort()
        n = len(citations)
        for i, c in enumerate(citations):
            if c >= n - i:
                return n - i
        return 0
    ```

### Example 2
**[Problem]** Remove first-name duplicates 
- Input: array of names
- Output: array with all first-name duplicates removed 

1. O(n log n) time, O(1) space
    ```
    class Name:
        def __init__(self, first_name, last_name):
            self.first_name = first_name
            self.last_name = last_name
        
        def __eq__(self, other):
            return self.first_name == other.first_name
        
        def __lt()___(self, other):
            return (self.first_name < other.first_name if self.first_name != other.first_name else self.last_name < other.last_name)
    
    def eliminate_duplicates(A):
        A.sort()
        write_idx = 1
        for cand in A[1:]:
            if cand != A[write_idx - 1]:
                A[write_idx] = cand
                write_idx += 1
        del A[write_idx:]
    ```
    - using hash table has a worst-case space complexity of O(n) (though time is O(n))
    - reuse the input array for storing the final result to avoid additional space
        1. sort the array, which brings equal elements together --> O(n log n)
        2. eliminate the duplicates --> O(n)

### Example 3
**[Problem]** Smallest nonconstructible value 
- Input: array of positive integers
- Output: smallest number which is NOT the sum of a subset of elements of the array
    + [Example] `[1, 1, 1, 1, 1, 5, 10, 25] --> 21`

1. O(n log n) time
    ```
    def smallest_nonconstructible_value(A):
        max_constructible_value = 0
        for a in sorted(A):
            if a > max_constructible_value + 1:
                break
            max_constructible_value += a
        return max_constructible_value + 1
    ```
    - the smallest element in the array sets a lower bound on the change amount that can be constructed from that array
    - suppose a collection of numbers can produce every value up to and including V, but not V + 1, and now adding a new element u to the collection
        1. if u <= V + 1: can still produce every value up to and including V + u and cannot produce V + u + 1
        2. if u > V + 1: even by adding u to the collection, cannot produce V + 1
    - order of the elements within the array makes no difference to the amounts that are constructible
        + sorting the array allows to stop when reach a value that is too large to help, since all subsequent values are at least as large as that value
        + let M[i - 1] be the max constructible amount from the first i elements of the sorted array
            - if the next array element x is greater than M[i - 1] + 1, M[i - 1] is still the maximum constructible amount, so stop and return M[i - 1] + 1 as the result
            - otherwise, set M[i] = M[i - 1] + x and continue with element i + 1

### Example 4
**[Problem]** Partitioning and sorting an array with many repeated entries 
- Input: array of student objects (each student object has an integer-valued age field that is to be treated as a key)
- Output: original array rearranged such taht students of equal age appear together
    + the order in which different ages appear is not important

1. O(n) time, O(m) space (m: number of distinct ages)
    ```
    Person = collections.namedtuple('Person', ('age', 'name'))

    def group_by_age(people):
        age_to_count = collections.Counter((person.age for person in people))
        age_to_offset, offset = {}, 0
        for age, count in age_to_count.items():
            age_to_offset[age] = offset
            offset += count
        
        while age_to_offset:
            from_age = next(iter(age_to_offset))
            from_idx = age_to_offset[from_age]
            to_age = peopl[from_idx].age
            to_idx = aget_to_offset[people[from_idx].age]
            people[from_idx], people[to_idx] = people[to_idx], people[from_idx]
            age_to_count[to_age] -= 1
            if age_to_count[to_age]:
                age_to_offset[to_age] = to_idx + 1
            else:
                del age_to_offset[to_age]
    ```
    1. iterate through the array and record the number of students of each age in a hash (key: age, value: corresponding count)
    2. update the array in-place
        + maintain a subarray for each of the different types of elements
        + each subarray marks out entries which have not yet been assigned elements of that type 
        + swap elements across these subarrays to move them to their correct position
    - use 2 hash tables to track the subarrays 
        1. starting offset of the subarray 
        2. size of the subarray
        + as soon as the subarray becomes empty, remove it

    - if required to sort by age as well, use BST to map ages to counts, since BST-based map keeps ages in sorted order --> O(n + m log m) time ==> counting sort

### Example 6
**[Problem]** Team photo day - 1 
- Input: 2 teams (both teams have the same number of players) and the heights of the players in the team
- Output: true if it is possible to place players to take the photo subject to the placement constraint
    + a player in the back row must be taller than the player in front of him
    + all players in a row must be from the same team

1. O(n log n) time
    ```
    class Team:
        Player = collections.namedtuple('Player', ('height'))

        def __init__(self, height):
            self._players = [Team.Player(h) for h in height]
        
        @staticmethod
        def valid_placement_exists(team0, team1):
            return all(a < b for a, b in zip(sorted(team0._players), sorted(team1._players)))
    ```
    - if team A's tallest player is not taller than the tallest player in team B, then it is not possible to place team A behind team B and statisfy the placement constraint
    - if team A's tallest player is taller than the tallest player in team B, should place him behind the tallest player in B
        + same goes on for second,... tallest of A
    - efficiently check whether A's tallest, second tallest, etc players are each taller than B's tallest, second tallest, etc players by first sorting the arrays of player heights 


### Example 7
**[Problem]** Compute a salary threshold 
- Input: existing salaries and the target payroll
- Output: salary cap
    + every employee who earned more than the cap last year will be paid the cap this year
    + every employee who earned no more than the cap will see no change in their salary

1. O(n log n) time (due to sorting)
    ```
    def find_salary_cap(target_payroll, current_salaries):
        current_salaries.sort()
        unadjusted_salary_sum = 0.0
        for i, current_salary in enumerate(current_salaries):
            adjusted_people = len(current_salaries) - i
            adjusted_salary_sum = current_salary * adjusted_people
            if unadjusted_salary_sum + adjusted_salary_sum >= target_payroll:
                return (target_payroll - unadjusted_salary_sum)/adjusted_people
            unadjusted_salary_sum += current_salary
        return -1.0
    ```
    - as increase the cap, as long as it does not exceed someone's salary, the payroll increases linearly
        + suggests to iterate through the salaries in increasing order
    - if salary array and its prefix sums sorted in advance, then for a given value of T --> use binary search to get the cap in O(log n)