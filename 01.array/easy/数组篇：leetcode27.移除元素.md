## 移除元素思路

> 原题地址
> https://leetcode.cn/problems/remove-element/submissions/

刚刚开始我也没头绪，知道这是一个简单的题目，但想不出解法。刷题都是这样的，在漫漫的刷题过程中总结出来解题的方法，然后找方案套上去，看看行不行得通。

最简单最容易想到的就是暴力破解，我一个个遍历，把不重复的数据拿出来维护就行了。但题目说了不给额外空间，要求空间复杂度是O(1)。

于是我更新了我的思路，我只需要遍历时，遇到重复数据跳过它，然后下一个不是重复数据就移动到当前的位置。

这就需要用到指针了，这里有两个知识点
**双指针**：通过两个指针来维护数据，具体细节接下来讲；
**数组的删除**：我们都知道数组存放在连续的存储空间中，删除操作是把指定的数据删掉，然后把它后面的数据照搬过来补上。我们这题就是用到了这个特性。

那么我的解法就出来了：
1. 定义两个指针，fast 和 slow
2. 判断fast指针的元素是不是目标元素
	- 是：fast向前走一位，slow不动
	- 不是：nums[slow] = nums[fast], 然后 fast 和 slow 各向前走一位
3. 当fast走完数组后，非目标元素已经全部放在数组前面了，也就是slow指针之前的数据都是非目标元素，然后我们返回slow，这就是非目标元素子数组的长度，这应该不难理解。

于是我就以这个思路，写出来快慢指针golang解法，并通过。然后我准备写个python版本时，突然意识到，我们遍历时，for循环会一个个元素的向着末尾查找，这个不就是一个现成的快指针吗？所以我们只需要维护一个慢指针就好了。:joy:

于是更简洁的写法就写出来了，即题解中的双指针

## Golang 解法

**快慢指针**

```golang
func removeElement(nums []int, val int) int {
    slow := 0
    for fast, l := 0, len(nums); fast < l; fast++ {
        if nums[fast] == val {
            continue
        }
        
        nums[slow] = nums[fast]
        slow++
    }

    return slow
}
```

**双指针**
```golang
func removeElement(nums []int, val int) int {
    p := 0
    for _, v := range nums {
        if v != val {
            nums[p] = v
            p += 1
        }
    }
    return p
}
```

## Python 解法
**双指针**
```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        p = 0
        for i in nums:
            if i != val:
                nums[p] = i
                p += 1
        return p
```

## Java 解法
**双指针**
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int p = 0;
        for (int i : nums) {
            if (i != val) {
                nums[p++] = i;
            }
        }
        return p;
    }
}
```