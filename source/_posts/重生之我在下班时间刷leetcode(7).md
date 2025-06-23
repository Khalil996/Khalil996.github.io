
---
title: 重生之我在下班时间刷leetcode（7）
date: 2025-06-23 15:00:00
---

# LeetCode 题解：k 镜像数字的和

题目链接：[LeetCode - Sum of k-Mirror Numbers](https://leetcode.cn/problems/sum-of-k-mirror-numbers/?envType=daily-question&envId=2025-06-23)

## 🧠 题目描述简要

如果一个整数在十进制和 k 进制下都为回文数，我们称其为 **k 镜像数**。

给定两个整数 `k` 和 `n`，返回前 `n` 个 **k 镜像数** 的总和。

---

## 🚀 思路分析

### 💡 核心思想

为了满足在两个进制下都是回文，我们首先从十进制角度出发：

- 生成十进制的回文数（比直接遍历每个数效率高）。
- 然后将这些数转换为 k 进制，判断是否也是回文。
- 累加前 `n` 个满足条件的数字。

### 🔍 回文数生成技巧

我们按照如下方式构造十进制回文数：

- **奇数长度**：如 `12321` 是由 `123` + `21` 构成。
- **偶数长度**：如 `1221` 是由 `12` + `21` 构成。

因此我们使用双层循环：
- 外层 `base *= 10` 控制回文数的位数
- 内层遍历 `i` 构造不同的前缀

### ✅ 判断回文逻辑

```go
func isKPalindrome(x, k int) bool {
    if x % k == 0 {
        return false
    }
    rev := 0
    for rev < x/k {
        rev = rev * k + x % k
        x /= k
    }
    return rev == x || rev == x / k
}
```

这个函数等价于将十进制整数 x 转为 k 进制并判断其是否为回文（使用反转数对比技巧）。

---

## 💻 Go 代码实现

```go
const maxN = 30
var ans [10][]int

func isKPalindrome(x, k int) bool {
    if x % k == 0 {
        return false
    }
    rev := 0
    for rev < x/k {
        rev = rev*k + x%k
        x /= k
    }
    return rev == x || rev == x/k
}

func doPalindrome(x int) bool {
    done := true
    for k := 2; k < 10; k++ {
        if len(ans[k]) < maxN && isKPalindrome(x, k) {
            ans[k] = append(ans[k], x)
        }
        if len(ans[k]) < maxN {
            done = false
        }
    }
    if !done {
        return false
    }
    for k := 2; k < 10; k++ {
        for i := 1; i < maxN; i++ {
            ans[k][i] += ans[k][i-1]
        }
    }
    return true
}

func init() {
    for k := 2; k < 10; k++ {
        ans[k] = make([]int, 0, maxN)
    }
    for base := 1; ; base *= 10 {
        for i := base; i < base*10; i++ {
            x := i
            for t := i / 10; t > 0; t /= 10 {
                x = x*10 + t%10
            }
            if doPalindrome(x) {
                return
            }
        }
        for i := base; i < base*10; i++ {
            x := i
            for t := i; t > 0; t /= 10 {
                x = x*10 + t%10
            }
            if doPalindrome(x) {
                return
            }
        }
    }
}

func kMirror(k int, n int) int64 {
    return int64(ans[k][n-1])
}
```

---

## ✅ 总结

| 步骤 | 说明 |
|------|------|
| 构造十进制回文数 | 比逐个枚举更高效 |
| 检查是否也是 k 进制回文 | 用数学方法判断 |
| 预处理前 30 个答案并前缀和 | 快速查询 |
| 返回前 n 个 k 镜像数之和 | `ans[k][n-1]` |
| 时间复杂度 | 优化生成 + 查询，效率较高 |
