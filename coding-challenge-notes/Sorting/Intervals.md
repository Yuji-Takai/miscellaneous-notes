### Example 1
**[Problem]** Render a calendar 
- Input: set of events
- Output: maximum number of events that take place concurrently

1. O(n log n) time, O(n) space
    ```
    Event = collections.namedtuple('Event', ('start', 'finish'))
    Endpoint = collcetions.namedtuple('Endpoint', ('time', 'is_start'))

    def find_max_simultaneous_events(A):
        E = [p for event in A for p in (Endpoint(event.start, True), Endpoint(event.finish, False))]
        E.sort(key=lambda e: (e.time, not e.is_start))

        max_num_simultaneous_events, num_simultaneous_events = 0.0
        for e in E:
            if e.is_start:
                num_simultaneous_events += 1
                max_num_simultaneous_events = max(num_simultaneous_events, max_num_simultaneous_events)
            else:
                num_simultaneous_events -= 1
        return max_num_simultaneous_events
    ```
    - number of events scheduled for a given time changes only at times that are start or end times of an event
    - sort the set of all the endpoints in ascending order
        + if 2 endpoints have equal times, and one is a start time and the other is an end time, the one corresponding to the start time comes first
        + if both are start or finish time, break ties arbitrarily
    - as proceed through endpoints, can incrementally track the number of events taking place at that endpoint using a counter
        + for each endpoint that is the start of an interval, increment the counter by 1
        + for each endpoint that is the end of an interval, decrement the counter by 1
    - maximum value attained by the counter is maximum number of overlapping intervals

### Example 2
**[Problem]** Merging intervals
- Input: an array of disjoint closed intervals with integer endpoints (sorted by increasing order of left endpoint) and an interval to be added
- Output: union of the intervals in the array and the added interval (should be sorted by left endpoint)
    + [EXAMPLE] `[-4, -1], [0, 2], [3, 6], [7, 9], [11, 12], [14, 17] + [1, 8] --> [-4, -1], [0, 9], [11, 12], [14, 17]` 

1. O(n) time
    ```
    Interval = collections.namedtuple('Interval', ('left', 'right'))

    def add_interval(disjoint_intervals, new_interval):
        i, result = 0, []
        while (i < len(disjoint_intervals) and new_interval.left > disjoint_intervals[i].right):
            result.append(disjoint_intervals[i])
            i += 1
        while (i < len(disjoint_intervals) and new_interval.right >= disjoint_intervals[i].left):
            new_interval = Interval(min(new_interval.left, disjoint_intervals[i].left), max(new_interval.right, disjoint_intervals[i].right))
            i += 1
        
        return result + [new_interval] + disjoint_intervals[i:]
    ```
    - focus on endpoints and use the sorted property to quickly process intervals in the array
    - processing an interval in the array takes place in 3 stages:
        1. iterate through intervals which appear completely before the interval to be added --> all these intervals are added directly to the result
        2. as soon as encounter an interval that intersects the interval to be added, compute its union with the interval to be added --> this union is itself an interval
            1. iterate through subsequent intervals, as long as they intersect with the union being formed 
            2. add the single union to the result
        3. iterate through the remaining intervals 
    

### Example 3
**[Problem]** Computing the union of intervals 
- Input: set of intervals
- Output:set of disjoint intervals expressing the union of the input set

1. O(n log n) time
    ```
    Endpoint = collections.namedtuple('Endpoint', ('is_closed', 'val'))
    Interval = collections.namedtuple('Interval', ('left', 'right'))

    def union_of_intervals(intervals):
        if not intervals:
            return []
        intervals.sort(key=lambda i: (i.left.val, not i.left.is_closed))
        result = [intervals[0]]
        for i in intervals:
            if intervals and (i.left.val < result[-1].right.val or (i.left.val == result[-1].right.val and (i.left.is_closed or result[-1].right.is_closed))):
                if (i.right.val > result[-1].right.val or (i.right.val == result[-1].right.val and i.right.is_closed)):
                    result[-1] = Interval(result[-1].left, i.right)
            else:
                result.append(i)
        return result
    ```
    - process the intervals in sorted order, so that can focus on a subset of intervals as proceed
        1. sort the intervals on their left endpoints --> allows to not have to revisit intervals which are entirely to the left of the interval currently being processed
            + when sorting, if two intervals have the same left endpoint, put intervals which are left-closed first
            + break ties arbitrarily
        2. iterate through the sorted array of intervals:
            1. the interval most recently added to the result does not intersect the current interval, nor does its right endpoint equal the left endpoint of the current interval --> simply add the current interval to the end of the result array as a new interval
            2. the interval most recently added to the result intersects the current interval --> update the most recently added interval to the union of it with the current interval
            3. the interval most recently added to the result has its right endpoint equal to the left endpoint of the current interval, and one (or both) of these endpoints are closed --> update the most recently added interval to the union of it with the current interval