---
title: "LeedCode算法设计练习专题"
subtitle: "访问数组的位置使分数最大"
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


**难度**：中等.  **相关标签**：哈希表 字符串 滑动窗口

给你一个下标从 0 开始的整数数组 nums 和一个正整数 x 。

你 一开始 在数组的位置 0 处，你可以按照下述规则访问数组中的其他位置：

如果你当前在位置 i ，那么你可以移动到满足 i < j 的 任意 位置 j 。
对于你访问的位置 i ，你可以获得分数 nums[i] 。
如果你从位置 i 移动到位置 j 且 nums[i] 和 nums[j] 的 奇偶性 不同，那么你将失去分数 x 。
请你返回你能得到的 最大 得分之和。

**注意** ，你一开始的分数为 nums[0] 。

---

**示例 1**：
输入：nums = [2,3,6,1,9,2], x = 5
输出：13
解释：我们可以按顺序访问数组中的位置：0 -> 2 -> 3 -> 4 。
对应位置的值为 2 ，6 ，1 和 9 。因为 6 和 1 的奇偶性不同，所以下标从 2 -> 3 让你失去 x = 5 分。
总得分为：2 + 6 + 1 + 9 - 5 = 13 

**示例 2**：
输入：nums = [2,4,6,8], x = 3
输出：20
解释：数组中的所有元素奇偶性都一样，所以我们可以将每个元素都访问一次，而且不会失去任何分数。
总得分为：2 + 4 + 6 + 8 = 20



## 失败的方法：暴力枚举动态规划
思路及算法

先看算法设计：

```
class Solution {
    long ans,temp;

    public void dfs(int[] nums,int i,int j,int x){
        if (j>=nums.length){ 
            if(temp>ans) ans=temp;
            return;
        }

        for (int a=j;a<nums.length;a++){
            long tempp=temp;
            if(nums[i]%2!=nums[a]%2){
                temp+=nums[a]-x;
                
            }
            else temp+=nums[a];
            dfs(nums,a,a+1,x);
            temp=tempp;
        }
        


    }

public long maxScore(int[] nums, int x) {
        ans=0;
        temp=nums[0];
        dfs(nums,0,1,x);

        return ans;
    }   


}
```
我并不知道问题到底出现在哪，但显示失败，该算法使用的思想是暴力列举所有的可能性，例如对于数组【2，3，6，1，8】，从2开始会先深度遍历3-6-1-8，其中在遇到相邻的奇偶不同的会-x，
然后以8为重点结束后，在从3-6-8， 然后再3-1-8......


## 成功的方法：动态规划

```
class Solution {
    public long maxScore(int[] nums, int x) {
        long[] f = new long[2];
        Arrays.fill(f, -(1L << 60));
        f[nums[0] & 1] = nums[0];
        for (int i = 1; i < nums.length; ++i) {
            int v = nums[i];
            f[v & 1] = Math.max(f[v & 1], f[v & 1 ^ 1] - x) + v;
        }
        return Math.max(f[0], f[1]);
    }
}

```
该算法将问题简化为相同性质的规模更小的子问题，逐步解决子问题，不断更新奇数组和偶数组的值，并选出其中最大的值来作为当前的分数和












































































