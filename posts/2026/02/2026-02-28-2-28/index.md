---
title: "2 - 28"
date: 2026-02-28
categories: 
  - "daily-question"
tags: 
  - "每日一题"
---

[连接连续二进制数字](https://leetcode.cn/problems/concatenation-of-consecutive-binary-numbers/description/?envType=daily-question&envId=2026-02-28)

题目难度：Medium

给你一个整数 n ，请你将 **1** 到 **n** 的二进制表示连接起来，并返回连接结果对应的 **十进制** 数字对 **_109 + 7_** 取余的结果。

**模拟**、**位运算**

```
class Solution {
    int P=1e9+7;
public:
    int concatenatedBinary(int n) {
        long ans=0;
        for(int i=1;i<=n;++i){
            int t=i;
            int b=0;
            while(t){
                b++;
                t>>=1;
            }
            ans=(ans<<b|i)%P;
        }
        return ans;
    }
};
```
