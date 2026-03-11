---
title: "2 - 16"
date: 2026-02-16
categories: 
  - "daily-question"
tags: 
  - "每日一题"
---

[颠倒二进制位](https://leetcode.cn/problems/reverse-bits/)

题目难度：Easy

![](images/9C7A35EB2C3D9CEF1DE36FDCEC30B96F-823x1024.png)

**模拟**

```
class Solution {
public:
    int reverseBits(int n) {
        int ans=0;
        for(int i=0;i<32;++i){
            ans=(ans<<1)+n%2;
            n>>=1;
        }
        return ans;
    }
};
```

**库函数**

```
class Solution {
public:
    int reverseBits(int n) {
        return __builtin_bitreverse32(n);
    }
};
```
