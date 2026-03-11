---
title: "2 - 21"
date: 2026-02-21
categories: 
  - "daily-question"
tags: 
  - "每日一题"
---

[二进制表示中质数个计算置位](https://leetcode.cn/problems/prime-number-of-set-bits-in-binary-representation/)

题目难度：Easy

![](images/16910DD9D4544DAAE493F8F497E65E1B-710x1024.png)

**模拟**

```
class Solution {
public:
    int countPrimeSetBits(int l, int r) {
        auto isp=[](int x)->bool{
            if(x==1)return 0;
            for(int i=2;i*i<=x;++i){
                if(x%i==0)return 0;
            }
            return 1;
        };
        auto cnt=[](int x)->int{
            int t=0;
            while(x){
                if(x&1)t++;
                x>>=1;
            }
            return t;
        };
        int ans=0;
        for(int i=l;i<=r;++i){
            if(isp(cnt(i)))ans++;
        }
        return ans;
    }
};
```
