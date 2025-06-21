
---
title: 重生之我在下班时间刷leetcode（6）
date: 2025-06-21 15:00:00
---

# LeetCode 题解：最少删除次数使字符串成为 K 特殊字符串

题目链接：[LeetCode - Minimum Deletions to Make String K-Special](https://leetcode.cn/problems/minimum-deletions-to-make-string-k-special/description/?envType=daily-question&envId=2025-06-21)

## 🧠 题目描述简要

定义一个字符串是 **K 特殊字符串**：如果所有字符出现的频率之间的差值不超过 `k`。

给定一个字符串 `word` 和整数 `k`，你可以删除任意字符，问最少需要删除多少个字符才能使其变成 K 特殊字符串？

---

## 🚀 思路分析

我们想要调整所有字母的频率，使得 **任意两个频率之差不超过 `k`**。这就意味着我们要让所有字母的频率**接近**某个目标频率 `base`，允许的波动范围是 `±k`。

### 算法步骤

1. **统计每个字符出现的频率**（26 个小写字母）。
2. 将所有频率排序，尝试每个频率作为 `base`。
3. 设当前基准频率为 `base`，对于后面的每个频率 `c`，我们允许最多保留 `base + k` 个，超过的都视为删除。
4. 计算在这种情况下最多能保留的字符数 `sum`。
5. 枚举所有可能的 `base` 值，取最大保留数 `maxSave`。
6. 最终答案就是：`len(word) - maxSave`。

---

## 💻 Go 代码实现

```go
func minimumDeletions(word string, k int) int {
    cnt := make([]int, 26)
    for _, b := range word {
        cnt[b-'a']++
    }

    slices.Sort(cnt)
    maxSave := 0

    for i, base := range cnt {
        sum := 0
        for _, c := range cnt[i:] {
            sum += min(c, base+k) // 多余的都删掉
        }
        maxSave = max(maxSave, sum)
    }

    return len(word) - maxSave
}
```

> 注意：需要定义 `min` 和 `max` 函数，或使用标准库中的泛型版本（Go 1.21+）。

---

## ✅ 总结

| 步骤 | 说明 |
|------|------|
| 统计每个字符的频率 | 长度为 26 的数组 |
| 枚举每个频率作为 `base` | 尝试作为统一频率目标 |
| 对每个 `base`，保留每个字母最多 `base + k` 次 | 超出的部分删除 |
| 计算最大保留字符数 | `maxSave` |
| 最小删除次数 | `len(word) - maxSave` |
| 时间复杂度 | O(26^2)，即常数时间 |
