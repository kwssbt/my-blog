---
title: "2 - 22"
date: 2026-02-22
categories: 
  - "daily-question"
tags: 
  - "每日一题"
---

二进制间距

题目难度：Easy

![](images/86F899320F759A10C94AA831B17CB53C-761x1024.png)

**模拟**

```
class Solution {
public:
    int binaryGap(int n) {
        int ans=0;
        int cnt=0;
        while(n){
            if(n&1){
                if(cnt==0)cnt++;
                else{
                    ans=max(ans,cnt);
                    cnt=1;
                }
            }
            else{
                if(cnt)cnt++;
            }
            n>>=1;
        }
        return ans;
    }
};
```
