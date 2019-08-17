### Example 1
**[Problem]** 2 sum problem
- Input: sorted int array and target value
- Output: true if there are 2 entries in the array that add up to that value

1. O(n<sup>2</sup>) time
    - brute force: test all pairs 

2. O(n) time, O(n) space
    - add each element of the array to a hash table
    - test for each element e if K - e (K: target value) is present in the hash table

3. O(n) time, O(1) space
    ```
    def has_two_sum(A, t):
        i, j = 0, len(A) - 1
        while i <= j:
            if A[i] + A[j] == t:
                return True
            elif A[i] + A[j] < t:
                i += 1
            else:
                j -= 1
        return False
    ```
    - use invariant: maintain a subarray that is guaranteed to hold a solution, if it exists
        + this subarray is initialized to the entire array
        + iteratively shrink the subarray from one side or the other
            + shrinking makes use of the sortedness of the array
            + if the sum of the leftmost and rightmost elements is less than the target, then the leftmost element can never be combined with some element to obtain the targe

### Example 2
**[Problem]** The 3-sum problem
- Input: array (entries may not be distinct) and a number
- Output: true if there are three entries in the array which add up to the specified number

1. O(n<sup>2</sup>) time, O(1) space
    ```
    def has_three_sum(A, t):
        A.sort()
        return any(has_two_sum(A, t - a) for a in A)
    ```
    - avoid additional space complexity by first sorting the input
        + sort A and for each A[i], search for indices j and k such that A[j] + A[k] = t - A[i]
        1. start with A[0] + A[n - 1]
            + if this equals t - A[i], done
            + otherwise:
                1. if A[0] + A[n - 1] < t - A[i], move to A[1] + A[n - 1] 
                    - there is no change A[0] pairing with any other entry to get t - A[i] (since A[n - 1] is the largest value in A)
                2. if A[0] + A[n - 1] > t - A[i], move to A[0] + A[n - 2]
            + an entry is eliminated in each iteration (O(1) per iteration), yielding O(n) time to find A[j] and A[k] such that A[j] + A[k] = t - A[i] if such entries exist
    - invariant: if 2 elements which sum to the desired value exist, they must lie within the subarray currently under consideration

### Example 3
**[Problem]** Find the majority element
- Input: sequence of strings 
- Output: majority element (find just by a single pass over the sequence)
    + know *a priori* that more than half the strings are repetitions of a single string (majority element) but the positions where the majority element occurs are unknown

1. O(n) time, O(1) space
    ```
    def majority_search(stream):
        candidate, candidate_count = None, 0
        for it in stream:
            if candidate_count == 0:
                candidate, candidate_count = it, candidate_count + 1
            elif candidate == it:
                candidate_count += 1
            else:
                candidate_count -= 1
        return candidate
    ```
    - intuition:
        1. group entries into 2 subgroups: those containing the majority element & those not containing the majority element 
        2. since size of first group > size of second group, if 2 entries are different, at most one can be the majority element
        3. by discarding both, the difference in size of the first subgroup and second subgroup remains the same, so the majority of the remaining entries remains unchanged
    - algorithm:
        1. have a candidate for the majority element, and track its count
            + candidate is initialized to the first entry
        2. iterate through remaining entries
            + each time see an entry equal to the candidate, increment the count
            + if the entry is different, decrement the count
            + if the count becomes zero, set the next entry to be the candidate       

### Example 4
**[Problem]** The gasup problem
- Input: gasup problem
- Output: ample city
    + assume there exists an ample city
    + gasup problem: 
        - number of cities are arranged on a circular road
        - need to visit all the cities and come back to the starting city
        - a certain amount of gas is available at each city
        - amount of gas summed over all cities == amount of gas required to go around the road once
        - gas tunk has unlimited capacity
    + ample city: if can begin at that city with an empty tank, refill at it, and then travel through all the remaining cities, refilling at each, and return to the ample city, without running out of gas at any point

1. O(n) time, O(1) space
    ```
    MPG = 20

    def find_ample_city(gallons, distances):
        remaining_gallons = 0
        CityAndRemainingGas = collections.namedtuple('CityAndRemainingGas', ('city', 'remaining_gallons'))
        city_remaining_gallons_pair = CityAndRemainingGas(0, 0)
        num_cities = len(gallons)
        for i in range(1, num_cities):
            remaining_gallons += gallons[i - 1] - distances[i - 1] // MPG
            if remaining_gallons < city_remaining_gallons_pair.remaing_gallons:
                city_remaining_gallons_pair = CityAndRemainingGas(i, remaining_gallons)
        return city_remaining_gallons_pair.city
    ```
    - *z*: a city where the amount of gas in the tank is minimum when entrying that city
        + this does not matter where begin from
        + pick *z* as the starting point, with the gas present at *z*:
            1. since never have less gas than started with at *z*
            2. when return to *z* have 0 gas
            - it means can complete the journey without running out of gas
    - computation to determine *z* can be easily performed with a single pass over all the cities simulating the changes to amount of gas as advance

### Example 5
**[Problem]** Compute the maximum water trapped by a pair of vertical lines
- Input: integer array
- Output: pair of entries that trap the maximum amount of water
    + the array of integers defines a set of lines parallelt to the Y-axis, starting from x = 0
    + find the pair of lines that together with the X-axis trap the most water

1. O(n) time
    ```
    def get_max_trapped_water(heights):
        i, j, max_water = 0, len(heights) - 1, 0
        while i < j:
            width = j - i
            max_water = max(max_water, width * min(heights[i], heights[j]))
            if heights[i] > heights[j]:
                j -= 1
            else:
                i += 1
        return max_water
    ```
    1. start from widest pair: 0 and n - 1
    2. record the corresponding amount of trapped water = ((n - 1) - 0) x min(A[0], A[n - 1])
        + if A[0] > A[n - 1] then for any k > 0, the water trapped between k and n - 1 is less than the water trapped between 0 and n - 1 --> so only consider max water trapped between 0 and n - 2
        + converse if A[0] <= A[n - 1]
    - continuously reduce the subarray that must be explored while recording the most water trapped so far

### Example 6
**[Problem]** Compute the largest rectangle under the skyline
- Input: array A that represents the heights of adjacent buildings of unit width
- Output: area of the largest rectaingle contained in the skyline 

1. O(n) time, O(n) space
    ```
    def calculate_largest_rectangle(heights):
        pillar_indices, max_rectangle_area = [], 0
        for i, h in enumerate(heights + [0]):
            while pillar_indices and heights[pillar_indices[-1]] >= h:
                height = heights[pillar_indices.pop()]
                width = i if not pillar_indices else i - pillar_indices[-1] - 1
                max_rectangle_area = max(max_rectangle_area, height * width)
            pillar_indices.append(i)
        return max_rectangle_area
    ```
    - as advance through the buildings, all need to keep track of is buildings that have not been blocked yet
    - can replace existing building whose height equals that of the current building with the current building
        + call these buildings the set of active pillars
    
    - whenever a building is removed from the active pillar set, know exactly how far to the right the largest rectangle that is supports goes on
    - when removing a blocked building from the active pillar set, to find how far to the left its largest supported rectangle extends, simply look for the closest active pillar that has a lower height
    - insertion and deletion into the active pillar set take place in LIFO order, use stack
        + rightmost building in the active pillar == top of stack
        + using a stack, makes it easy to find how far to the left the largest rectangle that's supported by a building in the active pillar set extends == simply look at the building below it in the stack
    - at the end of the iteration:
        + the stack will not be empty (at the very least, it will contain the last building)
        + the largest rectangle supported by these buildings ends at the nth building
    