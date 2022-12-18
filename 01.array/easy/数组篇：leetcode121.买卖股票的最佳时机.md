<div class="note sd-yinhao">
	<a href="https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/" target="_blank">原题地址：121. 买卖股票的最佳时机</a>
</div>


## 解题思路

### 双指针

我们定义两个指针，`p1` 和 `p2`，以及一个变量 `res` 用来存放当前最大利润

- `p1`： 指向当前已找到的最小的元素
- `p2`：寻找 min 后面的最大元素以及比 min 小的元素
- `res` 用来存放当前最大利润。**计算最大利润的时机是 `p1` 指针发生变化**
  - 当 `p2` 遍历完数组时，需要再计算一次最大利润
- `p1` 指针不动， `p2` 向后寻找元素， 变量 min 和 max 分别用来存放当前的最小元素与最大元素
  - 当寻找到比当前 max 大的元素，替换 max
  - 当寻找到比当前 min 小的元素，替换 min，`p1` 指向这个元素， `p2` 也指向这个元素
  - min 变更时记得重新计算利润，然后再和当前的最大利润比较

<div class="note yellow">
    <span>解题过程中不用定义p1, p2</span>
    <p>1. p1, p2 实际上是不用定义的，只是更方便理解，形成解题的思维。解题过程中，我们只需要关注 min, max 的值</p>
	<p>2. p1 不用定义，是因为我们在遍历过程中，找到更小值就把这个这个值赋予了 min， 所以不需要它</p>
    <p>3. p2 不用定义，是因为我们需要遍历整个数组，用当前遍历位置的索引来表示就好， 所以不需要它</p>
</div>



## 解法

### python

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        res = 0
        min_val, max_val = prices[0], prices[0]
        for i in range(len(prices)):
            tmp_res = 0
            if min_val > prices[i]:
                tmp_res = max_val - min_val
                min_val = prices[i]
                max_val = prices[i]
            elif max_val < prices[i]:
                max_val = prices[i]

            res = max(res, tmp_res)

        res = max(max_val-min_val, res)
        return res
```

### golang

```go
func maxProfit(prices []int) int {
    res := 0
    min, max := prices[0], prices[0]
    for _, price := range prices {
        tmpRes := 0
        if min > price {
            tmpRes = max - min
            min, max = price, price
        } else if max < price {
            max = price
        }

        res = maxPrice(tmpRes, res)
    }

    return maxPrice(max-min, res)
}

func maxPrice(p1, p2 int) int {
    if p1 > p2 {
        return p1
    } else {
        return p2
    }
}
```

