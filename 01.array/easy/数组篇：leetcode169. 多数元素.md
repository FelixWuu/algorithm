<div class="note sd-yinhao">
	<a href="https://leetcode.cn/problems/majority-element/" target="_blank">原题地址：169. 多数元素</a>
</div>



## 解题思路

题目要素：

- 多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素
  - 说明这个数是众数
  - 也说明这个数的数量超过数组长度的一半

基础题目没有对复杂度做限制，那么我们可以很简单的想到以下几种方式：

- 排序：排完序后， `n/2` 必然是众数，也就是说这个位置必然就是最多元素，以快排为例：
  - 时间复杂度：`O(nlogn)`
  - 空间复杂度：`O(logn)`
- 哈希表：统计每一个元素出现的次数，选出最大的value
  - 时间复杂度：`O(n)`
  - 空间复杂度：`O(n)`
- 分治：
  - 假设元素 a 是数组的多数元素，我们将数组分为两个部分，那么 a 必然是至少一个部分的众数
  - 使用递归拆分，当所有子问题的数组长度都为1时，唯一的元素必然是众数
  - 开始回溯
    - 左右区间众数相同，则这一段的众数就是这个元素
    - 左右区间众数不同，则我们需要比较两个众数在整个区间中出现的次数
  - 按上述方式回溯到最顶层，众数也就出来了
  - 时间复杂度：`O(nlogn)`
  - 空间复杂度：`O(logn)`，我们需要额外的数组空间，用于存放每次分治的对半分的数组

## 解法

### python

**排序**

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        nums.sort()
        return nums[len(nums) // 2]
```

**哈希表**

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        cnt_dict = {}
        for n in nums:
            cnt_dict[n] = cnt_dict.get(n, 0) + 1

        return max(cnt_dict, key=cnt_dict.get)
```

**分治**

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        def get_mode(left, right) -> int:
            if left == right:
                return nums[left]

            mid = (right - left) // 2 + left
            left_mode = get_mode(left, mid)
            right_mode = get_mode(mid+1, right)

            if left_mode == right_mode:
                return left_mode

            left_cnt = sum(1 for i in range(left, right+1) if nums[i] == left_mode)
            right_cnt = sum(1 for i in range(left, right+1) if nums[i] == right_mode)

            return left_mode if left_cnt > right_cnt else right_mode

        return get_mode(0, len(nums) - 1)
```

解题思路：

- `4-5行`：left == right，说明两者索引一致，已经到了递归的最底层，直接返回当前元素即可
- `7行`：计算中间位置，将数组对半分
- `8-9行`：计算左右两个子数组的众数
  - 为什么是 `left - mid`，`mid+1 - right`，有什么依据？
  - 注意我们第7行 mid 的计算方式，使用 `(left, mid-1), (mid, right)`会无法正确划分子数组的，具体可以使用一个长度为2的数组带入进去试试
- `11-12行`：如果两个子数组的众数都一致，我们就不用算了
- `14-15行`：如果两个子数组的总数不一致，我们就要合并后，遍历整个数组，左众数和右众数哪个出现的个数多

### golang

**排序**

```golang
func majorityElement(nums []int) int {
    sort.Ints(nums)
    return nums[len(nums)/2]
}
```

**哈希表**

```golang
func majorityElement(nums []int) int {
    resMap := make(map[int]int, len(nums))
    for _, num := range nums {
        if _, ok := resMap[num]; ok {
            resMap[num] += 1
        } else {
            resMap[num] = 1
        }
    }

    half := len(nums) / 2
    for key, val := range resMap {
        if val > half {
            return key
        }
    }

    return 0
}
```

**分治**（略）



## **进阶：**尝试设计时间复杂度为 O(n)、空间复杂度为 O(1) 的算法解决此问题

### 摩尔投票法

官方的题解说得很多，有点看不太懂，我整理一下我自己的想法，通俗一点的说出来

- 我们已知的
  - 多数元素占了数组 n/2 以上
  - 除了多数元素，其他的元素不止一种

我们可以这样理解，假设有一处藏宝地，寻宝猎人们各属于不同的队伍，各个队伍都想独吞这块藏宝地。现在我们把所有数组中所有的元素都当成寻宝猎人，其中数字相同的寻宝猎人属于同一支队伍。到达藏宝地时，如果藏宝地没有人，则猎人会开始据守。

每次从数组中派遣一名寻宝猎人抢夺宝藏，如果守护宝藏的是同队伍的寻宝猎人，则他们会共同据守宝藏。当来个不同阵营的猎人时，会互相厮杀，且必然出现一换一的伤亡。

最终数组中所有寻宝猎人都已派遣完时，由于多数元素的猎人数量超过了数组的一半，他们必然能占领宝藏。具体如下：

- 第一个到达藏宝地的猎人发现这里没人，开始据守，人数 `count = 1`
- 接下来寻宝猎人依次前往藏宝地
  - 与藏宝地猎人属于同队伍，一起据守，当前人数 `count += 1`
  - 与藏宝地猎人属于不同队伍，一换一，当前人数 `count -= 1`
  - 当新来的猎人发现藏宝地无人，他将占领此地，人数 `count = 1`

直到数组中的猎人全部派遣完，查看藏宝地的猎人是哪个队伍的，就知道多数元素是谁

#### python

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        count = 1
        team = nums[0]
        for i in range(1, len(nums)):
            if nums[i] == team:
                count += 1
            elif count == 0:
                team = nums[i]
                count = 1
            else:
                count -= 1

        return team
```

#### golang

```golang
func majorityElement(nums []int) int {
    team := nums[0]
    count := 1
    for i, l := 1, len(nums); i < l; i++ {
        if nums[i] == team {
            count += 1
        } else if count == 0 {
            team = nums[i]
            count = 1
        } else {
            count -= 1
        }
    }

    return team
}
```

