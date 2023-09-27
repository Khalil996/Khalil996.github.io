---
title: 打卡Day3

---
# 算法每日打开

### 题目：

#### [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例 1:**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

 

**提示：**

- `0 <= s.length <= 5 * 104`
- `s` 由英文字母、数字、符号和空格组成

#### 

### 题解：

这题的思路大概就是使用滑动窗口来实现，利用俩个指针，当我们的左指针指定起始位置时，右指针开始从其实指针开始右移，当遇到重复的字符时，将记录该长度然后去比对大小，然后通过删除起始位置的节点，使得左指针☞的是下一个节点，那么在前面这种情况下说明在原来的初始位置遇到重复字符的情况是在右指针指到目前位置才出现，说明现在的初始位置到原来的右指针的位置没有重复字符，所以继续让右指针继续右移直到走完整个字符串。

### 代码：

```golang
func lengthOfLongestSubstring(s string) int {
    m := map[byte]int{}
    n := len(s)
    r, ans := 0, 0
    for i := 0; i < n; i++ {
       if i != 0 {
          delete(m, s[i-1])
       }
       for r < n && m[s[r]] == 0 {
          m[s[r]]++
          r++
       }
       ans = max(ans, r-i)
    }
    return ans
}

func max(a, b int) int {
    if a > b {
       return a
    }
    return b
}
```

