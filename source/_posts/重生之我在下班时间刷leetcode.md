---
title: 重生之我在下班时间刷leetcode（1）
date: 2025-06-16 19:00:00
---
# LeetCode 题解：最大差值（最大差值之间的增量元素）

题目链接：[LeetCode - 最大差值](https://leetcode.cn/problems/maximum-difference-between-increasing-elements/description/?envType=daily-question&envId=2025-06-16)

## Go 函数 `maximumDifference` 分析与优化

### 🧠 问题描述

我写了两个版本的 `maximumDifference` 函数：

- 第一个版本没有显式初始化 `ans`，使用默认值 `0`
- 第二个版本显式初始化了 `ans = -1`

我发现**第一个可以正常运行，第二个却“跑不通”**（逻辑不符合预期）。

---

### ✅ 第一个版本（默认初始化）

```go
func maximumDifference(nums []int)(ans int) {
    for i, _ := range nums {
        for j := i; j < len(nums); j++ {
            if nums[i] > nums[j] {
                continue
            } else {
                ant := nums[j] - nums[i]
                if ans <= ant {
                    ans = ant
                }
            }
        }
    }
    if ans == 0 {
        return -1
    }
    return ans
}
```

- `ans` 默认是 `0`
- 只有在最大差值为 `0` 时才返回 `-1`

---

### ❌ 第二个版本（初始化为 -1）

```go
func maximumDifference(nums []int)(ans int) {
    ans = -1
    for i, _ := range nums {
        for j := i; j < len(nums); j++ {
            if nums[i] > nums[j] {
                continue
            } else {
                ant := nums[j] - nums[i]
                if ans <= ant {
                    ans = ant
                }
            }
        }
    }
    return ans
}
```

- 显式设置 `ans = -1`
- 当 `j == i` 时，`nums[i] - nums[j] == 0` 也会被计算进来，可能影响结果
- 第二版逻辑也没判断是否跳过 `i == j`，导致 `0` 可能被错误地当作最大差值

---

### 🛠 推荐改进：避免 `i == j` 的情况

```go
func maximumDifference(nums []int) int {
    ans := -1
    for i := 0; i < len(nums); i++ {
        for j := i + 1; j < len(nums); j++ { // 注意 j 从 i+1 开始
            if nums[j] > nums[i] {
                diff := nums[j] - nums[i]
                if diff > ans {
                    ans = diff
                }
            }
        }
    }
    return ans
}
```

---

### 🚀 最优解：一次遍历 O(n) 时间复杂度

```go
func maximumDifference(nums []int) int {
    minVal := nums[0]
    maxDiff := -1

    for i := 1; i < len(nums); i++ {
        if nums[i] > minVal {
            diff := nums[i] - minVal
            if diff > maxDiff {
                maxDiff = diff
            }
        } else {
            minVal = nums[i]
        }
    }
    return maxDiff
}
```

---

### ✅ 总结

| 问题点                    | 说明                                             |
|---------------------------|--------------------------------------------------|
| `j := i` 导致 `i == j`    | 会出现无效差值（如 0）被纳入比较                |
| 第二个版本使用 `-1` 初始值 | 合理，但逻辑需要跳过 `i == j` 比较              |
| 推荐方案                  | 改为 `j := i + 1` 或使用一次遍历（O(n)）方式    |
