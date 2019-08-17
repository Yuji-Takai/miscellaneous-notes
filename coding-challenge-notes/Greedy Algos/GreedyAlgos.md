# Greedy Algorithms
## Python
### Greedy Algorithm
- an algorithm that computes a solution in steps; at each step the algorithm makes a decision that is locally optimum, and it never changes that decision
    + does NOT necessarily produce the optimum solution

### Example
- for US currency (1, 5, 10, 25, 50, 100) - greedy algorithmm for making change results in the minimum number of coins
``` O(1) time (fixed number of coins)
def change_making(cents):
    COINS = [100, 50, 25, 10, 5, 1]
    num_coins = 0
    for coin in COINS:
        num_coins += cents // coin
        cents %= coin
    return num_coins
```