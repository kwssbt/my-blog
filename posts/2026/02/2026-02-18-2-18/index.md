---
title: "2 - 18"
date: 2026-02-18
categories: 
  - "daily-question"
tags: 
  - "每日一题"
---

[交替位二进制数](https://leetcode.cn/problems/binary-number-with-alternating-bits/)

题目难度：Easy

![](images/91E3C48A599662A7C9505D4B19C8091D-953x1024.png)

```
class Solution {
public:
    bool hasAlternatingBits(int n) {
        bool f=1;
        int x=n&1;
        n>>=1;
        while(n){
            int t=n&1;
            if(x==t)return 0;
            x=t;
            n>>=1;
        }
        return 1;
    }
};
```
