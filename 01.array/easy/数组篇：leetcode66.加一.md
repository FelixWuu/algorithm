<div class="note sd-yinhao">
	<a href="https://leetcode.cn/problems/plus-one/" target="_blank">原题地址：66.加一</a>
</div>



## 解题思路

本题考察的是数学思维在数组中的应用，首先就要想到以下几点：

- 本题的数组首位是数字的首位，数组的末位是数字的末位，所以第一次开始加一的地方应该是数组最后一位（索引 -1）
- 如果是 9，要先前进位
  - 即变为 0，然后前一位 +1
- 特殊情况，整个数组中的元素都是 9，会有数组越界或者又返回最末位的情况
  - 这种情况我们直接创建一个比原来数组长度 +1 的数组即可，首位为1，其余为0



## 解法

### python

```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        i = len(digits) - 1
        if digits[i] != 9:
            digits[i] += 1
            return digits
        else:
            while i > 0:
                digits[i] = 0
                digits[i-1] += 1
                if digits[i-1] % 10 != 0:
                    return digits
                i -= 1
        return [1] + [0] * len(digits)
```

### golang

```go
func plusOne(digits []int) []int {
    i := len(digits) - 1
    if digits[i] != 9 {
        digits[i] += 1
        return digits
    } else {
        for i > 0 {
            digits[i] = 0
            digits[i-1]++
            if digits[i-1] % 10 != 0 {
                return digits
            }

            i--
        }
    }

    res := make([]int, len(digits)+1)
    res[0] = 1
    return res
}
```

### 边界条件

python解法的第 8 行，golang 解法的第 7 行，只需要大于 0，等于 0 会有越界或者回到末位的问题（因为我们要计算 `digits[i-1]`）