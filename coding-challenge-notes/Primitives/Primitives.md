# Primitives [NOT COMPLETE]
## Basics
### Integer
- **Integers** in *Python3* are **unbounded** = max integer representable is a function of the available memory
- `sys.maxsize`: find the word-size = max value integer that can be stored in the word (ex: 2<sup>63</sup> - 1  on a 64-bit machine)
- `sys.float_info`: bounds on **floats**
- **Negative numbers** are treated as their **2's complement value**
- *No concept of unsigned shift* in *Python*, since **integers** have **infinite precision**

### Float
- **Floats** are NOT **infinite precision**
- Refer to **infinity** as `float('inf')` [*pseudo max-int*] & `float('-inf')` [*pseudo min-int*]



### Bitwise operations
| Name        | Operator | Note                                                            |
|-------------|----------|-----------------------------------------------------------------|
| AND         |    `&`   | AND with 1: can see if the least significant digit is 1       |
| OR          |   `|`    | OR with 0: can check if the least significant digit is 0       |
| NOT         | `~`      |                                                                 |
| XOR         | `^`      | XOR 2 nums: Find location where bits differ                     |
| Right Shift | `>>`     |                                                                 |
| Left Shift  | `<<`     |                                                                 |

### Key methods for numeric types
| Method                                    | Description                         |
|-------------------------------------------|-------------------------------------|
| `abs()`                                   | absolute value                      |
| `math.ceil()`                             | ceil                                |
| `math.floor()`                            | floor                               |
| `min(a, b)`                               | min value                           |
| `max(a,b)`                                | max value                           |
| `pow(base, n)`                            | exponent (equivalent: `base ** n`)  |
| `math.sqrt()`                             | square root                         |
| `math.isclose(a, b [, rel_tol, abs_tol])` | compare 2 floats                    |

### Conversion
| Types           | Example                                           |
|-----------------|---------------------------------------------------|
| `str` & `int`   | `str(42) == '42'` <=> `int('42') == 42`           |
| `str` & `float` | `str(3.14) == '3.14'` <=> `float('3.14') == 3.14` |

### Random
| Method                                   | Description                                                                                                         |
|------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| `random.randrange(stop)`                 | Return random interger within 0 and stop - 1                                                                        |
| `random.randrange(start, stop [, step])` | Return random interger within the specified range and step size                                                     |
| `random.randint(a, b)`                   | Return random integer N s.t. `a <= N < b`                                                                           |
| `random.random()`                        | Return random float N in [0.0, 1.0)                                                                                 |
| `random.shuffle(arr)`                    | Randomly shuffle an array (`random.sample(arr, k = len(arr))` to shuffle immutable list)                            |
| `random.sample(population, k)`           | Return list of k unique elements chosen from the population sequence or set (*random sampling without replacement*) |
| `random.choice(seq)`                     | Return random element from the non-empty sequence *seq*                                                             |

## Examples
### Example 1
**[Problem]** Count number of bits set to 1 in a non-negative integer

1. O(n) solution using shifting and masking
  ```
  def count_bits(x):
    num_bits = 0
    while x:
      num_bits += x & 1   # masking 
      x >>= 1             # shifting
    return num_bits
  ```
  - ANDing with 1 to check if the last digit is 1
  - increment num_bits with the result of the ANDing 

2. ???

### Example 2
**[Problem]** Compute the parity of a binary word (64-bit word)
- **Parity**: 1 if the number of 1's in the word is odd; otherwise, 0
- Parity checks are used to detect single bit errors in data storage and communication

1. O(n) solution --> worst, need to check all bits
  ```
  def parity(x):
    result = 0
    while x:
      result ^= x & 1
      x >>= 1
    return result
  ```
  - ANDing with 1 to check if the last digit is 1
  - use XOR to change the result to be either 1 (odd) or 0 (even)

2. O(k) solution (k: # of bits set)
  ```
  def parity(x):
    result = 0
    while x:
      result ^= 1
      x &= x - 1    # Drops the lowest set bit of x
    return result
  ```
  - By erasing the lowest set bit in a word in a single operation, improve best and average case performance

3. O(n/L) using caching --> L width of the words for caching results
  ```
  def parity(x):
    MASK_SIZE = 16
    BIT_MASK = 0xFFFF
    return (PRECOMPUTED_PARITY[x >> (3 * MASK_SIZE)] ^ PRECOMPUTED_PARITY[(x >> (2 * MASK_SIZE)) & BIT_MASK] ^ PRECOMPUTED_PARITY[(x >> MASK_SIZE) & BIT_MASK] ^ PRECOMPUTED_PARITY[x & BIT_MASK])
  ```
  - Ex: Lookup table for 2-bit words -> cache: <0, 1, 1, 0> == parities of (00), (01), (10), (11)
  - Split the 64-bit word into 4 16-bit subwords and look up the parity
  - Cache the parity of all 16-bit words using an array (PRECOMPUTE_PARITY)
  - need to shift and mask the leading bits using masking

4. **O(log n)**
  ```
  def parity(x):
    x ^= x >> 32    # Only the right 32 bits matter
    x ^= x >> 16
    x ^= x >> 8
    x ^= x >> 4
    x ^= x >> 2
    x ^= x >> 1
    return x & 0x1
  ```
  - FACT: Parity of <b<sub>63</sub>, b<sub>62</sub>, ..., b<sub>1</sub>, b<sub>0</sub>> = Parity of <b<sub>63</sub>, ..., b<sub>32</sub>> XOR <b<sub>31</sub>, ..., b<sub>0</sub>>
  - EX: Parity of 11010111 = Parity of (1101 XOR 0111) [= 1010] = Parity of (10 XOR 10) [= 00] = Parity of (0 XOR 0) = 0

### Example 3
**[Problem]** Swap bits 
- Take a 64-bit integer as input and swap the bits at indices at i and j 
- Index 0 = LSB (Least significant bit)
1. O(1) solution 
  ```
  def swap_bits(x, i, j):
    if (x >> i) & 1 != (x >> j) & 1:
      bit_mask = (1 << i) | (1 << j)
      x ^= bit_mask
    return x
  ```
  - swap if i-th and j-th bits differ (swapping would not change anything if the two bits are the same)
  - if the 2 bits differ, swap them by flipping their values using XOR 

### Example 4
**[Problem]** Reverse bits 
- Ex: 1000000000000111 -> 1110000000000001
1. O(n/L) solution --> use cache
  ```
  def reverse_bits(x):
    MASK_SIZE = 16
    BIT_MASK = 0xFFFF
    return (PRECOMPUTED_REVERSE[x & BIT_MASK] << (3 * MASK_SIZE) | PRECOMPUTED_REVERSE[(x >> MASK_SIZE) & BIT_MASK] << (2 * MASK_SIZE) |
    PRECOMPUTED_REVERSE[(x >> (2 * MASK_SIZE)) & BIT_MASK] << MASK_SIZE |
    PRECOMPUTED_REVERSE[(x >> (3 * MASK_SIZE)) & BIT_MASK])
  ```
  - Array-based lookup-table (PRECOMPUTED_REVERSE) s.t. for every 16-bit intger y, PRECOMPUTED_REVERSE[y] holds the reverse of y

### Example 5
**[Problem]** Find a closest iteger with the same weight
- **Weight**: number of bits set to 1 for a nonnegative integer
- Assume x is not 0 or all 1's
1. O(n) Solution
  ```
  def closest_int_same_bit_count(x):
    NUM_UNSIGNED_BITS = 64
    for i in range(NUM_UNSIGNED_BITS - 1):
      if (x >> i) & 1 != (x >> i + 1) & 1:
        x ^= (1 << i) | (1 << (i + 1))
        return x
    raise ValueError('All bits are 0 or 1')
  ```
  - Flip bit at k1 and k2 (k1 > k2)
  - Absolute value of the difference between the original integer and the new one is 2<sup>k1</sup> - 2<sup>k2</sup>
  - To minimize the diff, must make k1 as small as possible and k2 as close to k1 
  - Also, must preserve the weight == bits at k1 and k2 must differ
    + Thus, swap the 2 rightmost consecutive bits that differ

2. O(1) Solution
  ```
  def closest_int_same_bit_count(x):
    diff = x ^ (x >> 1)
    rbs = diff & ~(diff - 1) 
    index = int(math.log(rbs, 2))
    return x ^ (1 << i | 1 << i + 1)
  ```
  - find the indices where the consecutive bits differ by XORing x with x rightshifted by 1 bit and masking

## FACTS
- `x & (x - 1)`: x with its lowest set bit erased
- `x & ~(x - 1)`: isolate the lowest bit that is 1 in x

### Example 5
**[Problem]** Reverse digits
- Input: integer
- Ouput: integer corresponding to the digitis of the input written in reverse order
  + 42 --> 24, -314 --> -413

1. O(n) time, O(1) space (n = number of digits of input)
  ```
  def reverse(x):
    result, x_remaining = 0, abs(x)
    while(x_remaining):
      result = result * 10 + x_remaining % 10
      x_remaining //= 10
    return -result if x < 0 else result
  ```

