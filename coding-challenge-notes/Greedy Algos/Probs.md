### Example 1
**[Problem]** Compute an optimum assignment of tasks
- Input: set of tasks
- Output: optimum assignment
    + tasks are split amongst 2 workers and the workers will work simultaneously
    + each task is associated with the time to finish
    + find the way to split so that the time taken to finish all tasks is minimized

1. O(n log n) time
    ```
    PairedTasks = collections.namedtuple('PairedTasks', ('task_1', 'task_2'))

    def optimum_task_assignment(task_durations):
        task_durations.sort()
        return [PairedTasks(task_durations[i], task_durations[~i]) for i in range(len(task_durations) // 2)]
    ```
    - pair the shortest, second shortest, third shortest, etc tasks with the longest, second longest, third longest, etc tasks


### Example 2
**[Problem]** Schedule to minimize waiting time
- Input: service times for a set of queries
- Output: minimum waiting time of the schedule for processing the queries that minimizes the total waiting time
    + queries must be processed one at a time, but can be done in any order
    + Waiting time: time a query waits before its turn comes 
    + [EXAMPLE] [2, 5, 1, 3] --> waiting time = 0 + (2) + (2 + 5) + (2 + 5 + 1) = 17 

1. O(n log n) time
    ```
    def minimum_total_waiting_time(service_times):
        service_times.sort()
        total_waiting_time = 0
        for i, service_time in enumerate(service_times):
            num_remaining_queries = len(service_times) - (i + 1)
            total_waiting_time += service_time * num_remaining_queries
        return total_waiting_time
    ```
    - should sort the queries by their service time and then process them in the order of nondecreasing service time

### Example 3
**[Problem]** The interval covering problem
- Input: set of closed intervals
- Output: minimum size of set of numbers that covers all the intervals
    + [EXAMPLE] [0, 3], [2, 6], [3, 4], [6, 9] -->  0, 2, 3, 6 works but minimum is 3, 6

1. O(n log n) time
    ```
    Interval = collections.namedtuple('Interval', ('left', 'right'))

    def find_minimum_visits(intervals):
        intervals.sort(key=operator.attrgetter('right'))
        last_visit_time, num_visits = float('-inf'), 0
        for interval in intervals:
            if interval.left > last_visit_time:
                last_visit_time = interval.right
                num_visits += 1
        return num_visits
    ```
    - restrict attention to numbers which are endpoints without losing optimality
    - consider interval that ends first (interval whose right endpoint is minimum)
        + to cover it, must pick a number that appears in it 
        + should pick its right endpoint, since any other intervals covered by a number in the interval will continue to be covered if the right endpoint is picked (otherwise the interval began with cannot be the interval that ends first)
        + once choose that endpoint, can remove all other covered intervals, and continue with the remaining set
    1. sort all the intervals, comparing on right endpoints
    2. select the first interval's right endpoint
    3. iterate through the intervals, looking for the first one not covered by this right endpoint
    4. as soon as such interval is found, select its right endpoint and continue the iteration
