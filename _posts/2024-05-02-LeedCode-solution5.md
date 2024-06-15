---
title: "LeedCode算法设计练习专题"
subtitle:"最长回文子串(详解)"
date: 2024-05-02 12:00:00
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


**难度**：中等.  **相关标签**：双指针 动态规划 字符串

给你一个字符串 s，找到 s 中最长的 **回文** **子串**（字越少，题越难系列）

---

**示例 1**：
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案

**示例 2**：
输入：s = "cbbd"
输出："bb"


## 方法1：巧妙地求正逆字符串的交集
思路及算法

先看算法设计：

```
class Solution {
    public String longestPalindrome(String s) {

        int length = s.length();
        String maxStr="";
        String reverse=new StringBuffer(s).reverse().toString();

        int x=0;
        int y=1;
        while (x<length&&y<=length){
            String substring = s.substring(x, y);
            if (reverse.contains(substring)){
                if(substring.equals(new StringBuffer(substring).reverse().toString()))
                if (substring.length()>maxStr.length()){
                    maxStr=substring;
                }
                y++;
            }else {
                x++;
                y=x+1;
            }
        }

        return maxStr;
    }
}

```

利用回文字符串的特性，正反一样那么在正反字符串求最大的交集，即是最长的回文子字符串


## 方法2：动态规划

思路及算法

动态规划的核心就是要具备一种抽象的思想：
> 由小规模问题的解逐步递推到较大规模的问题解，自底向上

这是很重要的计算机算法思想，通过解决一个一个小问题来自然的解决最终复杂的大规模问题， 和贪婪算法思想也有些类似，类比到该字符串问题上
便是先限定要解决的字符串长度，从2开始然后逐步过渡到s.length()长度， 在每一个限定的长度中分析最两端的字符是否相等，如果相等的话并且该范围
大于2，那么就再去查看更内部的字符是否相等，


注意这里的方法可行性和合理性就是假设你要分析的字符串范围是i..j，在j-i+1=2的时候你是第一去解决的， 那么当j-i+1>2的时候， 当你决定i..j的字符串是否是回文
你就需要去确定 s[i+1][j-1]是否是回文，而这个范围下的解我们一定是之前已经解决掉的，非常有意思的递进关系

这就是用小问题的解去回答大问题， 用bool类型的dp[i][j]数组元素来表示字符串i....j是否是回文串
并且用maxlen变量记录最长的长度

```
//java
class Solution {
    public String longestPalindrome(String s) {

        int len=s.length();
        if (len<2) return s;

        
        int maxlen=1;
        int begin=0;

        char [] Array=s.toCharArray();
        boolean[][] dp=new boolean[len][len];
        for (int i=0;i<len;i++) dp[i][i]=true;


        for (int L=2;L<=len;L++){
            for(int i=0;i<len;i++){
                int j=i+L-1;
                if (j>=len) break;

                if(Array[i]==Array[j]){
                    if(L==2) dp[i][j]=true;
                    else {
                        dp[i][j]=dp[i+1][j-1];
                    }
                }

                if(dp[i][j]&&j-i+1>maxlen){
                    begin=i;
                    maxlen=j-i+1;
                }
                 

            }
        }

        return s.substring(begin,begin+maxlen);

    }

}

```


```
//C++
#include <iostream>
#include <string>
#include <vector>

using namespace std;

class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        if (n < 2) {
            return s;
        }

        int maxLen = 1;
        int begin = 0;
        // dp[i][j] 表示 s[i..j] 是否是回文串
        vector<vector<int>> dp(n, vector<int>(n));
        // 初始化：所有长度为 1 的子串都是回文串
        for (int i = 0; i < n; i++) {
            dp[i][i] = true;
        }
        // 递推开始
        // 先枚举子串长度
        for (int L = 2; L <= n; L++) {
            // 枚举左边界，左边界的上限设置可以宽松一些
            for (int i = 0; i < n; i++) {
                // 由 L 和 i 可以确定右边界，即 j - i + 1 = L 得
                int j = L + i - 1;
                // 如果右边界越界，就可以退出当前循环
                if (j >= n) {
                    break;
                }

                if (s[i] != s[j]) {
                    dp[i][j] = false;
                } else {
                    if (j - i < 3) {
                        dp[i][j] = true;
                    } else {
                        dp[i][j] = dp[i + 1][j - 1];
                    }
                }

                // 只要 dp[i][L] == true 成立，就表示子串 s[i..L] 是回文，此时记录回文长度和起始位置
                if (dp[i][j] && j - i + 1 > maxLen) {
                    maxLen = j - i + 1;
                    begin = i;
                }
            }
        }
        return s.substr(begin, maxLen);
    }
};

```


## 方法3：中心扩散方法

```
//自底向上，让人想到归并排序的自底向上方法

class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() < 1) {
            return "";
        }
        int start = 0, end = 0;
        for (int i = 0; i < s.length(); i++) {
            int len1 = expandAroundCenter(s, i, i);
            int len2 = expandAroundCenter(s, i, i + 1);
            int len = Math.max(len1, len2);
            if (len > end - start) {
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }
        return s.substring(start, end + 1);
    }

    public int expandAroundCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            --left;
            ++right;
        }
        return right - left - 1;
    }
}


```
---

**总之**，动态规划思想在解决诸如数组、字符串的最大最长问题时有很大的作用，充分彰显化繁为简的思想.















































































