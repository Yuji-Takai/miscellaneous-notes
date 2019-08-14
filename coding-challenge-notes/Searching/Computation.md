### Example 1
**[Problem]** Compute the integer square root 
- Input: nonnegative integer
- Output: largest integer whose square is less than or equal to the given integer
    + [Example] `16 --> 4, 300 -> 17`
1. O(log k) time
    ```
    def square_root(k):
        left, right = 0, k
        while left <= k:
            mid = left + (right - left) // 2
            mid_squared = mid * mid
            if mid_squared <= k:
                left = mid + 1
            else:
                right = mid - 1
        return left - 1
    ```
    - maintain an interval consisting of values whose squares are unclassified with respect to *k* (i.e. it might be less than or greater than *k*)
    1. initialize interval to [0, k]
    2. compare the square of m = ⌊(l + r) / 2⌋ with k, use the elimination rule to update the interval
        + if m<sup>2 <= k, update interval to [m + 1, r]
        + if m<sup>2 > k, update interval to [l, m - 1]
    3. terminate when interval is empty == when every number less than *l* has a square less than or equal to *k* and *l*'s square is greater than *k*, so the result is *l* - 1

### Example 2
**[Problem]** Compute the real square root 
- Input: floating point value
- Output: input's square root

1. O(log x/s) time (s: tolerance)
    ```
    def square_root(x):
        left, right = (x, 1.0) if x < 1.0 else (1,0, x)

        while not math.isclose(left, right):
            mid = left + (right - left) // 2
            mid_squared = mid * mid
            if mid_squared > x:
                right = mid
            else:
                left = mid
        return left
    ```
    - if a number is too big to be the square root of *x*, then any number bigger than that number can be eliminated
    - if a number is too small to be the square root of *x*, then any number smaller than that number can be eliminated

    - cannot start with [0, x] because the square root is larger than *x* when x < 1.0
        + if x >= 1.0, can tighten the lower and upper bounds to 1.0 and *x*, respectively (since if 1.0 <= x then x <= x<sup>2</sup>)
        + if x < 1.0, can use x and 1.0 as the lower and upper bounds respectively (since the square root of *x* is greater than *x* but less than 1.0)
