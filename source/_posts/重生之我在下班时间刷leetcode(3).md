
---
title: 重生之我在下班时间刷leetcode（3）
date: 2025-06-18 22:00:00
---

# LeetCode 题解：将数组分成若干长度为 3 的子数组

题目链接：[LeetCode - Divide Array Into Arrays With Max Difference](https://leetcode.cn/problems/divide-array-into-arrays-with-max-difference/?envType=daily-question&envId=2025-06-18)

## 🧠 题目描述简要

给定一个数组 `nums` 和一个整数 `k`，你需要将数组划分成若干个**长度为 3 的子数组**，使得每个子数组中最大值和最小值的差不超过 `k`。

如果无法做到，返回空；否则返回所有划分的子数组。

---

## 🚀 思路分析

我们观察到：

- 子数组长度固定为 3，所以我们必须能整除分组。
- 为了让每组最大值和最小值之差尽可能小，应该先对数组排序。
- 每组取连续的 3 个数即可判断是否满足差值小于等于 `k`。

### 具体步骤

1. 对 `nums` 进行排序。
2. 使用标准库函数 `slices.Chunk(nums, 3)` 将排序后的数组分块。
3. 遍历每个分块 `a`：若 `a[2] - a[0] > k`，说明该组最大差值超过 `k`，返回 `nil`。
4. 若所有分块都满足条件，则返回结果。

---

## 💻 Go 代码实现

```go
func divideArray(nums []int, k int) (ans [][]int) {
    slices.Sort(nums) // 排序数组
    for a := range slices.Chunk(nums, 3) { // 每 3 个数一组
        if a[2]-a[0] > k { // 最大值 - 最小值 > k，失败
            return nil
        }
        ans = append(ans, a)
    }
    return
}
