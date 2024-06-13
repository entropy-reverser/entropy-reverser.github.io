---
title: "LeedCode算法设计练习专题"
subtitle: "无重复字符的最长字串"
date: 2024-04-21 12:00:00
layout: post
author: "逆熵人"
header-style: text
catalog: true
tags:
  - 数据结构与算法
  - PLF (编程语言基础)
  - Java
  - C
  - LeedCode笔记
---



## 无重复字符的最长子串
**难度**：中等.  **相关标签**：哈希表 字符串 滑动窗口

给定一个字符串 s ，请你找出其中不含有重复字符的 最长 
子串的长度。

---

**示例 1**：
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3

**示例 2**：
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1

示例 3:
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串

## 方法一：滑动窗口
思路及算法

思路和算法

我们先用一个例子考虑如何在较优的时间复杂度内通过本题。

我们不妨以示例一中的字符串 abcabcbb\texttt{abcabcbb}abcabcbb 为例，找出从每一个字符开始的，不包含重复字符的最长子串，
那么其中最长的那个字符串即为答案。对于示例一中的字符串，我们列举出这些结果，其中括号中表示选中的字符以及最长的字符串：

以 (a)bcabcbb 开始的最长字符串为 (abc)abcbb；
以 a(b)cabcbb 开始的最长字符串为 a(bca)bcbb；
以 ab(c)abcbb 开始的最长字符串为 ab(cab)cbb；
以 abc(a)bcbb 开始的最长字符串为 abc(abc)bb；
以 abca(b)cbb 开始的最长字符串为 abca(bc)bb；
以 abcab(c)bb 开始的最长字符串为 abcab(cb)b；
以 abcabc(b)b 开始的最长字符串为 abcabc(b)b；
以 abcabcb(b) 开始的最长字符串为 abcabcb(b)。

这样一来，我们就可以使用「滑动窗口」来解决这个问题了：
我们使用两个指针表示字符串中的某个子串（或窗口）的左右边界，其中左指针代表着上文中「枚举子串的起始位置」，而右指针即为上文中的 r_k

在每一步的操作中，我们会将左指针向右移动一格，表示 我们开始枚举下一个字符作为起始位置，然后我们可以不断地向右移动右指针，但需要保证这两个指针对应的子串中没有重复的字符。
在移动结束后，这个子串就对应着 以左指针开始的，不包含重复字符的最长子串。我们记录下这个子串的长度；

在枚举结束后，我们找到的最长的子串的长度即为答案。

```
class Solution {
    public int lengthOfLongestSubstring(String s) {
        // 哈希集合，记录每个字符是否出现过
        Set<Character> occ = new HashSet<Character>();
        int n = s.length();
        // 右指针，初始值为 -1，相当于我们在字符串的左边界的左侧，还没有开始移动
        int rk = -1, ans = 0;
        for (int i = 0; i < n; ++i) {
            if (i != 0) {
                // 左指针向右移动一格，移除一个字符
                occ.remove(s.charAt(i - 1));
            }
            while (rk + 1 < n && !occ.contains(s.charAt(rk + 1))) {
                // 不断地移动右指针
                occ.add(s.charAt(rk + 1));
                ++rk;
            }
            // 第 i 到 rk 个字符是一个极长的无重复字符子串
            ans = Math.max(ans, rk - i + 1);
        }
        return ans;
    }
}

```


## 方法二：哈希表


```
\\ java
class Solution {
    public int lengthOfLongestSubstring(String s) {
       
        Set<Character> occ = new HashSet<Character>();
        int n = s.length();
        
        int rk = 0, ans = 0;
        for (int i = 0; i < n; ++i) {
            if (i != 0) {
                
                occ.remove(s.charAt(i - 1));
            }
            while (rk < n && !occ.contains(s.charAt(rk))) {
               
                occ.add(s.charAt(rk));
                ++rk;
            }
            
            ans = Math.max(ans, rk - i);
        }
        return ans;
    }
}

```











































































