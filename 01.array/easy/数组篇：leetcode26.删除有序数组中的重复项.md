原题地址:

https://leetcode.cn/problems/remove-duplicates-from-sorted-array/

## 解题思路

拿到这道题我最先想到了快慢指针。原因有二：

1. 不能改变数组的长度，必须在原地修改数组
2. 数组是**升序排序**的

首先第一点，就说明我们不能开辟新空间，那么就需要在内部移动元素，其次第二点，说明了数组已经排好序了，重复的数据都在一起，我们只需要记得重复出现的数据第一位在哪里，后续的重复数据都可以忽略，去找下一个新元素了。

因此，我们需要一个慢指针来记录第一个出现的重复元素，第二个快指针前往寻找新元素。然后把新元素放到慢指针的后一位即可。最后慢指针及其前面的部分数组就是一个已去重的数组了。

### 快慢指针

1. 设置快慢指针，都在数组首位
2. 移动快指针，看快指针指向的元素是否与慢指针当前的元素相等。
   - 相等，慢指针前进一位
   - 不相等，慢指针不动，快指针继续前移，直到所指的元素与慢指针指向的元素不同
     - 将快指针所指的元素搬迁到慢指针之后，慢指针前进一位
3. 快指针遍历完数组后，并做完2所述操作后，返回慢指针所指部分及其前面的数组，即为已去重数组，题目结果需要我们返回list的长度，返回slow+1即可（slow指针是索引，计算长度需要加回1）

#### 边界问题注意事项

1. 空的nums， 我们直接返回空的list就行了 
2. 思路第三步 `快指针遍历完数组后`，注意快指针是使用的索引，所以判断遍历完数组应该是 fast == 最后一个索引

#### Python解法

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return []

        slow, fast = 0, 0
        while fast < len(nums) - 1:      
            fast += 1
            if nums[slow] != nums[fast]:
                slow += 1
                nums[slow] = nums[fast]
        
        return slow + 1
```

#### Golang 解法

```go
func removeDuplicates(nums []int) int {
    slow := 0
    for fast, l := 1, len(nums); fast < l; fast ++ {
        if nums[slow] != nums[fast] {
            slow ++
            nums[slow] = nums[fast]
        }
    }

    return slow + 1
}
```

#### 总结

可以看到 python 和 golang 两种解法中，我的 fast 判断是看起来似乎不一样

- python 中是 `fast < len(nums) - 1`
- golang 中是 `fast < len(nums)`

其实这就是边界问题注意事项中的第二点，结束条件可能比想象中的要难写出来，取决于我们快慢指针的设定，python 中快指针起始位置与慢指针相同，golang 中快指针比慢指针先走一步。我们可能不同时刻大致思路相同，但写法不同，所以，做题时一定要注意好边界条件！

- 我们的写法，在起始时的情况是否符合
- 我们的写法到结束时刻是否符合

