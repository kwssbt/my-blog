---
title: "2 - 19"
date: 2026-02-18
categories: 
  - "daily-question"
tags: 
  - "每日一题"
---

[计数二进制子串](https://leetcode.cn/problems/count-binary-substrings/description/?envType=daily-question&envId=2026-02-19)

题目难度：Easy

![](images/3449AC23B2E6EBE471A7E03E4D18943F-964x1024.png)

**模拟**

```
class Solution {
public:
    int countBinarySubstrings(string s) {
        vector<int>a;
        int n=s.size();
        int i=0;
        while(i<n){
            int j=i;
            while(j+1<n&&s[j+1]==s[j])j++;
            a.push_back(j-i+1);
            i=j+1;
        }
        int ans=0;
        for(int i=1;i<a.size();++i){
            ans+=min(a[i-1],a[i]);
        }
        return ans;
    }
};
```
