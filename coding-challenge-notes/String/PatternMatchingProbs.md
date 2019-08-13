### Example 1
**[Problem]** Find the First Occurrence of a substring
- Input: strings s (search string) and t (text)
- Output: first occurrence of s in t

## KMP


## Boyer-Moore


## Rabin-Karp
### Concept
- concept of "fingerprint"
- *m*: the length of s
- compute hash codes ("fingerprints") of each substring whose length is *m* 
  + key to efficiency == use incremental hash function
  + [EXAMPLE] **rolling hash**: 
    - hash code of a string == an additive function of each individual character)
    - getting the hash code of a sliding window of characters is very fast for each shift

- For Rabin-Karp to run in linear time, need a good hash function, to reduce likelihood of collisions, which entail potentially time consuming string equality checks

### EXAMPLE
Strings consist of letters from {A, C, G, T}
t = "GACGCCA" & s = "CGC"
1. Define codes: "A" == 0, "C" == 1, "G" == 2, "T" == 3
2. Define hash function: decimal number formed by the integer codes for the letters
  - "CGC" --> 121, "ACG" --> 12
3. Iterate through the substrings of length 3 (len(s) == 3) until find s 
  - even if found a substring that has the same hash code as s, need to explicitly check if the substring matches s because they may be a collision
  ```
  "GAC" --> 2 * 100 + 0 * 10 + 1 * 1 = 201 != 121
  "ACG" --> (201 - 2 * 100) * 10 + 2 * 1 = 12 != 121
  "CGC" --> (12 - 0 * 100) * 10 + 1 * 1 = 121 == 121 --> check if the substring matches s --> it does :)
  ```

### Implementation
```
def rabin_karp(t, s):
  if len(s) > len(t):
    return -1
  BASE = 26
  t_hash = functools.reduce(lambda h, c: h * BASE + ord(c), t[:len(s)], 0)
  s_hash = functools.reduce(lambda h, c: h * BASE + ord(c), s, 0)
  power_s = BASE**max(len(s) - 1, 0)
  for i in range(len(s), len(t)):
    if t_hash == s_hash and t[i - len(s):i] == s:
      return i - len(s)
    t_hash -= ord(t[i - len(s)]) * power_s
    t_hash = t_hash * BASE + ord(t[i])
  if t_hash == s_hash and t[-len(s):] == s:
    return len(t) - len(s)
  return -1
```

### Run time
- For a good hash function, O(m + n), independent of the input s and t, where m = len(s) and n == len(t)