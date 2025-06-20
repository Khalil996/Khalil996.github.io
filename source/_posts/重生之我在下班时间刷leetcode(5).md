
---
title: 重生之我在下班时间刷leetcode（5）
date: 2025-06-20 20:49:00
---
# LeetCode 题解：最大曼哈顿距离（经过 k 次变向）

题目链接：[LeetCode - Maximum Manhattan Distance After K Changes](https://leetcode.cn/problems/maximum-manhattan-distance-after-k-changes/description/?envType=daily-question&envId=2025-06-20)

## 🧠 题目描述简要

给定一个字符串 `s` 表示一个人按顺序移动的路径，路径由 `'N'`、`'S'`、`'E'`、`'W'` 组成，代表四个方向。你最多可以**修改其中的 `k` 个字符**（改变方向），问你走完路径后能达到的**最大曼哈顿距离**是多少。

---

## 🚀 思路分析

### 什么是曼哈顿距离？

曼哈顿距离是当前位置 `(x, y)` 到原点 `(0, 0)` 的距离：

```text
distance = abs(x) + abs(y)
```

### 如何修改方向以最大化曼哈顿距离？

- 每次修改方向可以让当前位置远离原点。
- 每一步不是向远方向移动的，都可以考虑修改为与当前位置方向相反的方向，以增加距离。
- 每修改一步，最多能额外带来 `2` 的距离提升（比如从向北走一步变成向南走一步）。

### 遍历过程

- 初始化 `(x, y) = (0, 0)`。
- 模拟每一步移动更新坐标。
- 每一步之后计算当前的最大曼哈顿距离为：

```go
abs(x) + abs(y) + 2 * k
```

但要注意：**修改的步数不能超过当前已走的步数（i+1）**，所以最终答案是：

```go
max(ans, min(abs(x)+abs(y)+2*k, i+1))
```

---

## 💻 Go 代码实现

```go
func maxDistance(s string, k int) int {
    ans, x, y := 0, 0, 0

    for i, c := range s {
        switch c {
        case 'N':
            y++
        case 'S':
            y--
        case 'E':
            x++
        default: // 'W'
            x--
        }

        ans = max(ans, min(Abs(x)+Abs(y)+k*2, i+1))
    }

    return ans
}

func Abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```

---

## ✅ 总结

| 步骤 | 说明 |
|------|------|
| 模拟原路径并计算坐标变化 | 根据 'N'/'S'/'E'/'W' 更新 x 和 y |
| 每步尝试用 k 次变向增加最大距离 | 最多增加 `2*k` 曼哈顿距离 |
| 限制最大步数不能超过当前走的步数 | `min(abs(x)+abs(y)+2*k, i+1)` |
| 用 `max` 更新答案 | 保留整个过程中最优解 |
| 时间复杂度 | O(n) |
