# Dynamic Programming
## Python
### DP
- combining the solutions of multple smaller problems
    + same subproblem may reoccur
    + key to make DP efficient is caching results of intermediate computations

- key to solving a DP problem efficiently is finding a way to break the problem into subproblems such that:
    1. the original problem can be solved relatively easily once solutions to the subproblems are available, and
    2. these subproblem solutions are cached

- DP is applicable when can construct a solution to the given instance from solutions to subinstances of smaller problems of the same kind
- DP is applicable to optimization as well as counting and decision problems (where can express a solution recursively in terms of the same computation on smaller instances)
- conceptually DP involves recursion <=> often for efficiency, cache is built buttom-up == iteratively
- DP implemented recursively --> cache = dynamic data structure like hash table or BST
- DP implemented iteratively --> cache == 1- or multi-dimensional array
- To save space, cache space may be recycled once it is known that a set of entries will not be looked up again