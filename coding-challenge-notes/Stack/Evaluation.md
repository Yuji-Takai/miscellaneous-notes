### Example 1
**[Problem]** Evaluate RPN expressions
- Input: arithmetical expression in RPN
- Output: number that the expression evaluates to
    + a str is an arithmetical expression in **Reverse Polish Notation** (**RPN**) if:
        1. it is a single digit or a sequence of digits, prefixed with an option -
        2. It is of the form "A, B, ○" where A and B are RPN expressions and ○ is one of +, -, x, /
    + an RPN expression can be evaluated uniquely to an integer, which is determined recursively
        - base case corresponds to Rule 1, which is an integer expressed in base-10 positional system
        - recursive case corresponds to Rule 2, and the RPNs are evaluated in the natural way
    + [Example] `"3,4,+,2,x,1,+" --> (3 + 4) x 2 + 1 = 15`

1. O(n) time
    ```
    def evaluate(expression):
        intermediate_results = []
        DELIMITER = ','
        OPERATORS = { 
            '+': lambda y, x: x + y,
            '-': lambda y, x: x - y,
            '*': lambda y, x: x * y,
            '/': lambda y, x: int(x / y)
        }

        for token in expression.split(DELIMITER):
            if token in OPERATORS:
                intermediate_results.append(OPERATORS[token](intermediate_results.pop(), intermediate_results.pop()))
            else:
                intermediate_results.append(int(token))
        return intermediate_results[-1]
    ```
    - need to record partial results, and as operators are encountered, the operators are applied to the partial results
    - partial results are added and removed in LIFO order
    
### Example 2
**[Problem]** Normalize pathnames
- Input: str representing a pathname
- Output: shortest equivalent pathname
    + assume individual directories and files have names that use only alphanumeric characters
    + subdirectory names may be combined using forward slashes (/), the current directory (.), and parent directory (..)
    + [Example] `"/usr/lib/../bin/gcc" --> "/usr/bin/gcc", "scripts//./../scripts/awkscripts/././" --> "scripts/awkscripts"`

1. O(n) time, O(n) space
    ```
    def shortest_equivalent_path(path):
        if not path:
            raise ValueError('Empty string is not a valid path.')
        path_names = []
        # Special case: absolute path
        if path[0] == '/':
            path_names.append('/')
        for token in (token for token in path.split('/') if token not in ['.', '']):
            if token == '..':
                if not path_names or path_names[-1] == '..':
                    path_names.append(token)
                else:
                    if path_names[-1] == '/':
                        raise ValueError('Path error')
                    path_names.pop()
            else:
                path_names.append(token)
        result = '/'.join(path_names)
        return result[result.startswith('//'):] # avoid starting with //
    ```
    - Process left-to-right, splitting on forward slashes and recording directory and file names
    - Each time `..` is encountered, delete the most recent name, which corresponds to going up directory hierarchy
    - individual periods are skipped
    - if the string begins with `/`, cannot go up from it -> record in the stack
    - if the string does NOT begin with `/`, may encounter an empty stack when process `..`, which indicates a path that begins with an ancestor of the current working path
        + need to record in order to give the shortest equivalent path
    - final state of the stack directly corresponds to the shortest equivalent directory path

### Example 3
**[Problem]** Compute building with sunset view
- Input: a series of buildings
- Output: set of buildings which view the sunset 
    + process from east to west order
    + each building is specified by its height
    + each building has windows facing west
    + any building which is to the east of a building of equal or greater height cannot view the sunset

1. O(n) time, O(n) space (best 0(1) space)
    ```
    def examine_buildings_with_sunset(sequence):
        BuildingWithHeight = collections.namedtuple('BuildingWithHeight', ('id', 'height'))
        candidates = []
        for building_idx, building_height in enumerate(sequence):
            while candidates and building_height >= candidates[-1].height:
                candidates.pop()
            candidates.append(BuildingWithHeight(building_idx, building_height))
        return [c.id for c in reversed(candidates)]
    ```
    - if a building is to the east of a taller building, it cannot view the sunset
    - use a stack to record buildings that have a view
        + each time a building *b* is processed, if it is taller than the building at the top of the stack, pop the stack until the top of the stack is taller than *b* - all the buildings thus removed lie to the east of a taller building
        + each building is pushed and popped at most once = O(n) run time
