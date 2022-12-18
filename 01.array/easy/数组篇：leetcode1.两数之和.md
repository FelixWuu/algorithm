## 两数之和思路

> 原题地址：
> https://leetcode-cn.com/problems/two-sum/

思路：

这题一拿到就很容易想到暴力解法。但是显然这肯定不是最优解。

**哈希表**

我们可以使用hash表来存储数据：
- key: 数组中的值
- value：key在数组中对应的索引

1. 遍历数组，依次获取其中的元素`num`
2. 查询`key`：`target-num`是否存在哈希表中
	- 如果存在，则找到目标，返回`key`为`num`和`target-num`的`value`，这就是它们的索引
	- 如果不存在，则将`key`：`target-num`，`value`：对应的索引放入哈希表中


### Golang 解法

```golang
func twoSum(nums []int, target int) []int {
    hm := make(map[int]int, len(nums))
    for key, num := range nums {
        if val, ok := hm[target - num]; ok {
            return []int{val, key}
        } 
        hm[num] = key
    }
    return nil
}
```

### Python 解法

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        hm = {}
        for key, num in enumerate(nums):
            if target-num in hm:
                return [hm[target-num], key]
            hm[num] = key
```