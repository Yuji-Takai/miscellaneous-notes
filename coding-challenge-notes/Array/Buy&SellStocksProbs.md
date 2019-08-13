### Example 7
**[Problem]** Buy and sell a stock once
- Input: Array denoting the daily stock price
- Output: maximum profit that could be made by buying and then selling one share of that stock
1. O(nlog(n)) using divide and conquer
  1. split array into 2 subarrays
  2. compute the best result for the first and second subarray
  3. take the better of the results for the 2 subarrays
  4. take min of 1st half and max of 2nd half to handle the case when the buy and sell take place in separate subarrays
  5. return the max of the results from 3 & 4  
  - Recurrence: T(n) = 2T(n/2) + O(n) --> O(nlog(n))

2. O(n) time, O(1) space
  ```
  def buy_and_sell_stock_once(prices):
    min_price_so_far, max_profit = float('inf'), 0.0
    for price in prices:
      max_profit_sell_today = price - min_price_so_far
      max_profit = max(max_profit, max_profit_sell_today)
      min_price_so_far = min(min_price_so_far, price)
    return max_profit
  ```
  - max profit that can be made by selling on a specific day is determined by the *minimum* of the stock prices over the *previous* days
  - keep track of the minimum element *m* and the maximum profit seen so far
  - if current_element - *m* > maximum profit recorded so far, update the max profit

### Example 8
**[Problem]** Buy and sell a share *at most* twice
- Input: Array denoting the daily stock price
- Output: Maximum profit that could be made by buying and selling a share at most twice
- the second buy must be made on *another date after the first sale*

1. O(n) time, O(n) space
  ```
  def buy_and_sell_stock_twice(prices):
    max_total_profit, min_price_so_far = 0.0, float('inf')
    first_buy_sell_profits = [0] * len(prices)
    for i, price in enumerate(prices):
      min_price_so_far = min(min_price_so_far, price)
      max_total_profit = max(max_total_price, price - min_price_so_far)
      first_buy_sell_profits[i] = max_total_profit
    
    max_price_so_far = float('inf')
    for i, price in reversed(list(enumerate(prices[1:], 1))):
      max_price_so_far = max(max_price_so_far, price)
      max_total_profit = max(max_total_profit, max_price_so_far - price + first_buy_sell_profits[i - 1])
    return max_total_profit
  ```
  - record the best solution for A[0, j] (1 <= j <= n - 1)
  - reverse iterate, combining the best solution for a single buy-and-sell for A[j, n - 1] (1 <= j <= n - 1)
  - for each day combine the result of reverse iterate with the result from the forward iteration for the previous day
    + yields the maximum profit if buy and sell once before the current day and once at or after the current day

2. O(n) time, O(1) space
  ```
  def maxProfit(prices):
    sell1, sell2, buy1, buy2 = 0, 0, float('-inf'), float('-inf')
    for price in prices:
      buy1 = max(buy1, -price)
      sell1 = max(sell1, buy1 + price)
      buy2 = max(buy2, sell - price)
      sell2 = max(sell2, buy2 + price)
    return sell2
  ```
  - buy1: minimum price to buy the stock
  - sell1: best profit so far, if sell the stock by the ith Day (1st transaction)
  - buy2: best profit so far, if buy the stock by the ith Day (2nd transaction)
  - sell2: final profit

### Example 9
**[Problem]** Buy and sell a stock many times
- Input: Array denoting the daily stock price
- Output: Maximum profit that could be made by buying and selling a share as many as times as wanted

1. O(n) time, O(1) space
  ```
  def maxProfit(prices):
    maxProfit = 0
    for i in range(len(prices) - 1):
      if price[i] < price[i + 1]:
        maxProfit += (price[i + 1] - price[i])
    return maxProfit
  ```
  - add the profit gained from every consecutive transaction
  - almost the same as 1 transaction 