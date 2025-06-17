---
title: 重生之我在下班时间刷leetcode（2）
date: 2025-06-17 21:09:00
---

# LeetCode 题解：统计与给定整数相等的下标对数组

题目链接：[LeetCode - Count the Number of Arrays with K Matching Adjacent Elements](https://leetcode.cn/problems/count-the-number-of-arrays-with-k-matching-adjacent-elements/?envType=daily-question&envId=2025-06-17)

## 🧠 题目描述简要

给定三个整数 `n`、`m` 和 `k`，表示你需要构造一个长度为 `n` 的数组，数组元素取值范围为 `1 ~ m`，要求数组中**恰好有 `k` 对相邻元素相等**。求满足条件的数组总数，结果对 `10^9 + 7` 取模。

---

## 🚀 思路分析

我们将这个问题转化为组合数学问题：

1. **在数组中恰好有 `k` 个相邻位置相等**，说明我们从 `n-1` 个相邻对中选出 `k` 个使它们相等，其余的不相等。
2. **构造方法：**
   - 先选择哪些相邻对是相等的：从 `n-1` 个位置中选出 `k` 个，组合数 `C(n-1, k)`。
   - 第一个元素可以随便取，有 `m` 种选择。
   - 对于每一个不相等的相邻位置（`n-1-k` 个），有 `m-1` 种取法（不能与前一个相等）。

所以，最终结果为：

```
C(n-1, k) * m * (m-1)^(n-1-k)
```

---

## 🧮 公式推导

设：

- `C(n-1, k)` 是从 `n-1` 个相邻对中选择 `k` 个相等的组合数
- `m` 是首元素的选择
- `(m-1)^(n-1-k)` 是非相等相邻对的选法

所以：

```text
答案 = C(n-1, k) × m × (m-1)^(n-1-k)
```

---

## 💻 Go 代码实现

```go
const mx = 1e5 + 10
const mod = 1e9 + 7

var f [mx]int64
var g [mx]int64

// 快速幂
func qpow(a int64, k int) int64 {
    res := int64(1)
    for k != 0 {
        if k&1 == 1 {
            res = res * a % mod
        }
        a = a * a % mod
        k >>= 1
    }
    return res
}

// 预处理阶乘和阶乘逆元
func init() {
    f[0], g[0] = 1, 1
    for i := 1; i < mx; i++ {
        f[i] = f[i-1] * int64(i) % mod
        g[i] = qpow(f[i], mod-2)
    }
}

// 组合数 C(m, n)
func comb(m, n int) int64 {
    return f[m] * g[n] % mod * g[m-n] % mod
}

// 主函数
func countGoodArrays(n int, m int, k int) int {
    ans := comb(n-1, k) * int64(m) % mod * qpow(int64(m-1), n-k-1) % mod
    return int(ans)
}
```

---

## ✅ 总结

| 步骤 | 说明 |
|------|------|
| 选出 `k` 个相邻对相等的位置 | 组合数 `C(n-1, k)` |
| 第一个元素可任意选 | `m` 种选择 |
| 剩下 `n - 1 - k` 个位置必须不相等 | 每个位置有 `m - 1` 种选择 |
| 时间复杂度 | 预处理 O(n)，查询 O(1) |
