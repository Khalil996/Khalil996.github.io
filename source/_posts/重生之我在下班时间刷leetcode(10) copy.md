
---
title: 重生之我在下班时间刷leetcode（11）
date: 2025-07-01 21:00:00
---

# LeetCode 题解：找出原始输入的字符串 I

题目链接：[LeetCode - Find the Original Typed String I](https://leetcode.cn/problems/find-the-original-typed-string-i/description/?envType=daily-question&envId=2025-07-01)

---

## 🧠 题目描述简要

给定一个字符串 `word`，表示用户在输入一个原始字符串时因 **长按键盘** 导致连续输入了多个相同字符，题目要求你推测该原始字符串的可能个数。

- 每个长按的字符可以在原始字符串中只出现一次。
- 要找出有多少种可能的原始字符串，**使得长按之后可以变成 `word`**。

---

## 🚀 解题思路

### ✅ 直觉理解

对于每一段连续的相同字符 `c`，比如 `"aaa"`，它有以下推测可能：
- 原始字符串中只有一个 `c`
- 剩下的是长按导致的

换句话说：
- 每个连续段 `"cc...c"` 最少只能映射到一个 `c`
- 所以这段字符只对应 **1 个可能性**
- 然而如果我们允许不同的连续块被缩短为 1 个字符，那么：
  - 每个连续段（除了第一个）可以是否合并到前一个，也可以保留 → 分叉点

### ✅ 本题简化了问题：
- 每当遇到 `word[i] == word[i-1]`，就说明可能是长按导致重复的
- 因此我们可以在每次遇到这种重复时，认为可能是一个原始字符重复一次产生的分支

### ✅ 算法步骤

1. 初始化答案为 1
2. 遍历字符串 `word`
3. 如果当前字符和上一个字符相同，则说明可能是长按，答案乘 2
4. 实际代码中，为简化，我们只记录重复段的数量，最终答案就是 `重复段数 + 1`

---

## 💻 Go 代码实现

```go
func possibleStringCount(word string) int {
    ans := 1
    for i := 1; i < len(word); i++ {
        if word[i-1] == word[i] {
            ans++
        }
    }
    return ans
}
```

---

## 🔍 示例讲解

### 示例 1

```text
输入: word = "aabb"
连续重复段:
- "aa" → 对应 "a"
- "bb" → 对应 "b"

原始字符串可能是：
- "ab"

答案：1（只有一种可能）
```

### 示例 2

```text
输入: word = "aabaa"
连续重复段:
- 前两个 "a" 不重复
- 中间 "aa" 是重复
→ 可以把第二段的 "aa" 压缩为一个 "a"

原始字符串可能是：
- "aba"
- "aaba"

答案：2（两个可能）
```

---

## ✅ 总结

| 步骤 | 说明 |
|------|------|
| 初始化 | `ans = 1` |
| 每次重复出现 | 可能来源于长按，`ans++` |
| 最终结果 | 表示原始字符串的可能个数 |

---

## 📌 拓展思考

- 如果题目扩展为允许多个相同字符在原始字符串中重复，比如 `"aa"` → `"aaaa"`，该怎么处理？
- 本题中没有出现数字计数（如“a2b3”），若加入这种表示法，会变成压缩字符串匹配问题

---
