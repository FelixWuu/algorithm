<div class="note sd-yinhao">
	<a href="https://leetcode.cn/problems/pascals-triangle/" target="_blank">原题地址：118. 杨辉三角</a>
</div>



## 解题思路

![杨辉三角](https://pic.leetcode-cn.com/1626927345-DZmfxB-PascalTriangleAnimated2.gif)

- 从官方的动图中，我们可以发现开始有计算的是从第3层开始的。第一、二层比较特殊，未参与计算
  - 1 层我们默认生成 `[1]`
  - 2层实际上也是符合规律的，详见下面的思路
- 其余层观察可以发现
  - 首位和末位都是 1
  - 中间的数，分别是它上层相接的两个数的和，同时这两个数也是相邻的
  - 我们只需要通过计算上层相邻的两个数，两两相加，得到的和集合就是本层中间的元素
  - 上面说到的 2 层，也是符合这个规律的，因为 1 层只有一个元素，无法两两相加，因此得到的是一个空 list，然后首末位都是1，因此得到 `[1, 1]`



## 解法

### python

```python
class Solution:
    def generate(self, numRows: int) -> List[List[int]]:
        def get_full_layer(arr):
            return [1] + arr + [1]

        res = []
        for i in range(numRows):
            if i == 0:
                res.append([1])
            else:
                layer = []
                for j in range(len(res[i-1]) - 1):
                    layer.append(res[i-1][j] + res[i-1][j+1])
                res.append(get_full_layer(layer))
            
        return res
```

- 我们只需要特殊处理第一层，其余层可以根据上一层计算得到中间的元素
- 首位和末位需要补充元素 1

### golang

```go
func generate(numRows int) [][]int {
    res := make([][]int, 0, numRows)
    for i := 0; i < numRows; i++ {
        if i == 0 {
            res = append(res, []int{1})
        } else {
            layer := make([]int, 0)
            for j := 0; j < len(res[i-1]) - 1; j++ {
                layer = append(layer, res[i-1][j] + res[i-1][j+1])
            }

            res = append(res, get_full_layer(layer))
        }
    }

    return res
}

func get_full_layer(arr []int) []int {
    full := make([]int, 0, len(arr)+2)
    full = append(full, 1)
    full = append(full, arr...)
    full = append(full, 1)
    return full
}
```

## 总结

注意边界条件

- 第一层单独处理
- 遍历上层来生成本层中间部分的元素时，注意不要越界，比如
  - python 解法的第 12 行 `for j in range(len(res[i-1]) - 1)`
  - go 解法的第 8 行 `for j := 0; j < len(res[i-1]) - 1; j++`
  - 只遍历到 `len(res[i-1]) - 1` 是因为我们需要于后一位计算，即还要加上`res[i-1][j+1]`，所以只遍历到倒数第二位即可