
---
title: 重生之我在下班时间刷leetcode（8）
date: 2025-06-24 21:00:00
---


# LeetCode 题解：查找所有距离为 K 的键值索引

题目链接：[LeetCode - Find All K-Distant Indices in an Array](https://leetcode.cn/problems/find-all-k-distant-indices-in-an-array/description/?envType=daily-question&envId=2025-06-24)

## 🧠 题目描述简要

给定一个整数数组 `nums`，一个整数 `key` 和一个整数 `k`，请返回所有满足以下条件的索引 `i`：

存在某个索引 `j` 使得：

- `nums[j] == key` 且
- `abs(i - j) <= k`

换句话说，索引 `i` 到某个等于 `key` 的索引 `j` 的距离不超过 `k`。

---

## 🚀 思路分析

我们需要寻找所有距离为 `k` 内的索引 `i`，使得它靠近某个值为 `key` 的元素。可以从多种角度来思考：

### ✅ 方法一：暴力双重循环（思路清晰，效率较低）

1. 遍历每一个索引 `j`，如果 `nums[j] == key`：
2. 枚举 `i` 从 `j - k` 到 `j + k`，若在合法范围内，则将 `i` 加入结果集。

### 示例 Go 实现：

```go
func findKDistantIndices(nums []int, key int, k int) []int {
	var ans []int
	for i := 0; i < len(nums); i++ {
		for j := 0; j < len(nums); j++ {
			if nums[j] == key && abs(i-j) <= k {
				ans = append(ans, i)
				break
			}
		}
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}



**问题**：时间复杂度为 `O(n*k)`，数据大时容易超时。

---

### ✅ 方法二：滑动窗口优化（推荐，时间复杂度 O(n)）

我们可以优化搜索逻辑：

- 记录上一个 key 出现的位置 `last`。
- 遍历过程中，如果 `nums[i + k] == key`，就更新 `last = i + k`。
- 然后检查 `last >= i - k`，即说明当前 `i` 可以被纳入答案中。

### 示例 Go 实现：

```go
func findKDistantIndices(nums []int, key, k int) (ans []int) {
    last := -k - 1 // 保证 key 不存在时 last < i-k
    for i := k - 1; i >= 0; i-- {
        if nums[i] == key {
            last = i
            break
        }
    }

    for i := range nums {
        if i+k < len(nums) && nums[i+k] == key {
            last = i + k
        }
        if last >= i-k {
            ans = append(ans, i)
        }
    }
    return
}
