<div class="note sd-yinhao">
	<a href="https://leetcode.cn/problems/merge-sorted-array/submissions/" target="_blank">原题地址：88. 合并两个有序数组</a>
</div>



## 解题思路

- 合并存储在数组 `nums1` 中（不能开辟新数组）
- 需要排序

### 合并后排序

我们只需要先把数组 `num1` 和 `num2` 都合并，然后排序即可

**复杂度分析**：

- 时间复杂度：`O((m+n)log(m+n))`
  - 假设我们使用了快排，它的时间复杂度是 `O(nlogn)`
  - 我们现在需要排序一个合并后长度为 `m+n` 的数组，因此复杂度为 `O((m+n)log(m+n))`
- 空间复杂度：同样假设使用了快排，空间复杂度是 `O(log(m+n))`

### 双指针

需要一个中间数组 `tmp`，和两个指针 `p1, p2` 。它们分别指向两个数组的开头。比较所指元素的大小，然后将小的元素放到中间数组去，对应的指针往前移动。

**边界条件**

- 某个数组已遍历结束（即 `num1` 已遍历到 `m` 或 `num2` 已遍历到 `n`）
  - `p1` 已移动至 m，将 `num2` 剩余的元素添加到 `tmp`
  - ``p2` 已移动至 n，将 `num2` 剩余的元素添加到 `tmp`

**复杂度分析**

- 时间复杂度：遍历 `num1` 和 `num2`，所以是 `O(m+n)`
- 空间复杂度：需要一个中间数组，其长度为 `num1` 和 `num2`的长度和，所以是  `O(m+n)`

## 解法

### python

#### 合并后排序

```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        nums1[m:] = nums2
        nums1.sort()
```

### 双指针

```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        tmp = []
        p1, p2 = 0, 0
        while p1 < m or p2 < n:
            if p1 == m:
                tmp.append(nums2[p2])
                p2 += 1
            elif p2 == n:
                tmp.append(nums1[p1])
                p1 += 1
            elif nums1[p1] < nums2[p2]:
                tmp.append(nums1[p1])
                p1 += 1
            else:
                tmp.append(nums2[p2])
                p2 += 1
        nums1[:] = tmp

```

### golang

#### 合并后排序

```go
func merge(nums1 []int, m int, nums2 []int, n int)  {
    copy(nums1[m:], nums2)
    sort.Ints(nums1)
}
```

#### 双指针

```go
func merge(nums1 []int, m int, nums2 []int, n int)  {
    tmp := make([]int, 0, m+n)
    p1, p2 := 0, 0
    for p1 < m || p2 < n {
        if p1 == m {
            tmp = append(tmp, nums2[p2:n]...)
            break
        } else if p2 == n{
            tmp = append(tmp, nums1[p1:m]...)
            break
        } else if nums1[p1] < nums2[p2] {
            tmp = append(tmp, nums1[p1])
            p1++
        } else {
            tmp = append(tmp, nums2[p2])
            p2++
        }
    }

    copy(nums1, tmp)
}
```

### 总结

双指针的解法，一定要注意好加入 `tmp` 的元素是 `nums1` 的还是 `nums2` 的

