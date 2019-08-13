# Python
## Basics
### String
- Strings are immutable
  + operations like `s = s[1:]` or `s += '123'` imply creating a new array of characters that is then assigned back to `s`
  + concatenating a single character *n* times to a string in a for loop has O(n<sup>2</sup>) time complexity

### Key methods for String
| Method                                             | Description                                                                      |
|----------------------------------------------------|----------------------------------------------------------------------------------|
| `s[index]`, `s[start:end]`                         | character at index, slicing from start to end - 1                                |
| `s + t`                                            | concatenate s and t                                                              |
| `s in t`, `s not in t`                             | check if s is/is not a substring of t                                            |
| `s.startswith(prefix)`                             | check if s starts with prefix                                                    |
| `s.endswith(suffix)`                               | check if s ends with suffix                                                      |
| `ss.split(delimiter)`                              | split s based on delimiter                                                       |
| `delimiter.join(list(str))`                        | join list of string with delimiter                                               |
| `s.lower()`, `s.upper()`, `s.capitalize`           | make all characters to lower case, upper case, capitalize first character only   |
| `char.isalnum()`                                   | check if the character is alphanumeric                                           |
| `Name {name}, Rank {rank}.format(name=Yu, rank=3)` | format string                                                                    |

# Java
## Basics
### String
- Strings are immutable
  + when building a string inside a for loop, use `StringBuilder` instead of concatenation 