

---
title: 重生之我在下班时间刷leetcode（10）
date: 2025-06-27 21:00:00
---


# LeetCode 题解：统计最大组的数目

题目链接：[LeetCode - Count Largest Group](https://leetcode.cn/problems/count-largest-group/description/?envType=daily-question&envId=2025-06-27)

---

## 🧠 题目描述

给你一个整数 `n`，请你将 `1` 到 `n` 之间的每个数字分组，分组依据是每个数的「数位和」。

- 同一数位和的数字属于同一组。
- 找出数位和最多的组一共有多少个。

例如：
- 数字 13 的数位和是 1 + 3 = 4
- 数字 24 的数位和是 2 + 4 = 6

---

## 🚀 解题思路

### ✅ 思路简述

我们需要找出数位和最大的「组」出现的次数。问题可以转化为：

1. 对 `1 ~ n` 中每个数字，计算其数位和 `sumDigits(x)`。
2. 用哈希表统计每个数位和出现的次数。
3. 找出最大出现次数（即最大组的人数），统计有多少组达到了这个人数。

### ✅ 方法一：直接模拟（适用于小范围）

可以用一个 map 或数组模拟每个数的数位和，并记录每个和出现的次数，最后统计次数最大值即可。

**时间复杂度：** `O(n log n)`，因为每个数的数位和最多 `log10(n)` 位。

---

### ✅ 方法二：数位 DP 优化（高级）

当 `n` 非常大时，我们不能暴力遍历所有 `1 ~ n`，而是借助 **数位 DP** 技巧：

#### 核心思路：

- 定义 `dfs(i, left, limitHigh)`：表示从第 `i` 位开始填数，还需要凑出数位和 `left` 的个数。
- 利用记忆化数组 `memo[i][left]` 缓存子问题。
- `limitHigh` 控制是否当前前缀仍受 `n` 限制（也就是是否在范围内）。

#### 为什么可行：

- 数位和最多为 `m * 9`（`m` 为位数），搜索状态是有限的。
- 枚举每个可能的数位和 `target`，统计有多少数满足 `sumDigits(x) == target`。
- 最后找出最大组的人数，并统计这种组的个数。

---

## 💻 Go 代码实现

```go
func countLargestGroup(n int) (ans int) {
    s := strconv.Itoa(n)
    m := len(s)
    memo := make([][]int, m)
    for i := range memo {
        memo[i] = make([]int, m*9+1)
        for j := range memo[i] {
            memo[i][j] = -1
        }
    }

    var dfs func(i, left int, limitHigh bool) int
    dfs = func(i, left int, limitHigh bool) (res int) {
        if i == m {
            if left == 0 {
                return 1
            }
            return
        }

        if !limitHigh {
            p := &memo[i][left]
            if *p != -1 {
                return *p
            }
            defer func() { *p = res }()
        }

        hi := 9
        if limitHigh {
            hi = int(s[i] - '0')
        }

        for d := 0; d <= min(hi, left); d++ {
            res += dfs(i+1, left-d, limitHigh && d == hi)
        }
        return
    }

    maxCnt := 0
    for target := 1; target <= m*9; target++ {
        cnt := dfs(0, target, true)
        if cnt > maxCnt {
            maxCnt = cnt
            ans = 1
        } else if cnt == maxCnt {
            ans++
        }
    }
    return
}
```

---

## 📌 示例

输入：`n = 13`

数位和分组如下：

- 和为1：1
- 和为2：2
- 和为3：3
- 和为4：4, 13
- 和为5：5, 14
- 和为6：6, 15, 24
- 和为7：7, 16, 25
- 和为8：8, 17, 26, ...
- 最大组大小为 2，对应多个和 → 答案为这些组的个数。

---

## ✅ 总结

| 方法 | 说明 | 复杂度 |
|------|------|--------|
| 模拟统计 | 遍历每个数字并求其数位和 | O(n log n) |
| 数位 DP ✅ | 枚举每种数位和，统计个数 | O(位数 × 数位和) |
| 空间优化 | 使用记忆化剪枝重复状态 | O(位数 × 数位和) |

---

## 📌 小贴士

- 数位 DP 是处理数位限制类问题的常见技巧。
- 记忆化搜索非常关键，否则递归会爆栈或超时。
