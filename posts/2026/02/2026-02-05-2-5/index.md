---
title: "2 - 5"
date: 2026-02-05
categories: 
  - "daily-question"
---

[转换数组](https://leetcode.cn/problems/transformed-array/description/?envType=daily-question&envId=2026-02-05)

题目难度：Easy

![](images/6143AECC07C6C3E6FAD83A2872D3D112-658x1024.png)

循环数组

```
class Solution {
public:
    vector<int> constructTransformedArray(vector<int>& nums) {
        int n=nums.size();
        vector<int>ans;
        for(int i=0;i<n;++i){
            if(nums[i]>0){
                ans.push_back(nums[(i+nums[i])%n]);
            }
            else if(nums[i]<0){
                ans.push_back(nums[(i+nums[i]%n+n)%n]);
            }
            else ans.push_back(0);
        }
        return ans;
    }
};
```
