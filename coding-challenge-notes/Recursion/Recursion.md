# Recursion
## Python
### Recursion
- approach to problem solving where the solution depends partially on solutions to smaller instances of related problems
- recusive function:
    1. base case
    2. calls to the same function with different arguments
    3. recursion has to converge to the solution

- when asked to remove recursion from a program, consider mimicking call stack with the stack data structure
- recursion can be easily removed from a tail-recursive program using a while-loop == no stack needed

- divide and conquer: 
    1. repeatedly decompose a problem into two or more smaller independent subproblems of the same kind, until it gets to instancees that are simple enough to be solved directly
    2. the solutions to the subproblems are combined to give a solution to the original problem
    - ex: merge sort and quicksort 


### Example: Euclidean algorithm for Greatest Common Divisor (GCD)
- if y > x, the GCD of x and y is the GCD of x and y - x --> GCD of x and y is the GCD of x and y mod x
- O(log max(x, y)) time, O(n) space (n: number fo bits needed to represent the inputs)
```
def gcd(x, y):
    return x if y == 0 else gcd(y, x % y)
```
