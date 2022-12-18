原题地址：

#### [搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

## 解题思路

刚刚看到题目时，就觉得这道题为什么能在 leetcode 上呢？虽然是简单题，但也太简单了吧？

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        return len([n for n in nums if n < target])
```

如上，python 一句就写完了。（注：这个题解能通过，但是实际上是不符合题目要求的）

实际上，题中也说到了`请必须使用时间复杂度为 O(log n) 的算法。`

这句话相当于提醒你，可以用**二分查找**来解题了，这道题非常简单，最基础的二分查找。

## Python解法

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)
        while left < right:
            mid = left + (right-left) // 2
            if nums[mid] < target:
                left = mid + 1
            else:
                right = mid
        return left
```

- 第 7 行： 下一轮搜索区间是 `[mid+1 : right]`
- 第 9 行： 下一轮搜索区间是 `[left : mid]`

## Golang解法

```go
func searchInsert(nums []int, target int) int {
    left, right := 0, len(nums)
    for left < right {
        mid := left + (right - left) / 2
        if nums[mid] < target {
            left = mid + 1
        } else {
            right = mid
        }
    }
    
    return left
}
```

