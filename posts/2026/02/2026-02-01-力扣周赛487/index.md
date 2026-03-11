---
title: "力扣周赛487"
date: 2026-02-01
categories: 
  - "leetcode-contest"
tags: 
  - "周赛"
---

## A [统计单比特整数](https://leetcode.cn/problems/count-monobit-integers/)

_**0**_ 是单比特整数

除 _**0**_ 外，所有单比特整数二进制下每一位都是 _**1**_

对 _**\[ 1 , n \]**_ 的每一个数逐位检查

```
class Solution {
    bool check(int x){
        while(x){
            if(x&1){
                x>>=1;
                continue;
            }
            else{
                return 0;
            }
        }
        return 1;
    }
public:
    int countMonobit(int n) {
        int ans=1;
        for(int i=1;i<=n;++i){
            if(check(i)){
                ans++;
            }
        }
        return ans;
    }
};
```

## B [删除子数组后的最终元素](https://leetcode.cn/problems/final-element-after-subarray-deletions/description/)

答案为 **第一个数与最后一个数的较大值**

反证：

不妨记 **第一个数与最后一个数的较大值为 M，较小值为 m**。

一、假设最终元素小于 **_M_**：

_Alice_ 先手，她希望最终元素尽可能大，那么她可以一次删除除 **_M_** 外的所有数。

所以答案不可能小于 **_M_**。

二、假设最终元素大于 **_M_**：

如果最终元素为**_T， T > M_**

1._Alice_ 第一次操作没有删去 **_M_** ：

_Bob_ 希望最终元素尽可能小，为了让最终元素不是**_T ,_**此时他可以一次删除除 **_M_** 外的所有数。

2._Alice_ 第一次操作删去了 **_M_**：

_Alice_ 删去了 **_M_** ，那么 **_m_** 一定没有被删。

_Bob_ 希望最终元素尽可能小，那么此时他可以一次删除除 **_m_** 外的所有数，而 **_m < M_**。

所以答案不可能大于 **_M_**。

综上，答案为 **第一个数与最后一个数的较大值**。

```
class Solution {
public:
    int finalElement(vector<int>& nums) {
        return max(nums[nums.size()-1],nums[0]);
    }
};
```

## C [设计共享出行系统](https://leetcode.cn/problems/design-ride-sharing-system/description/)

关键是存在删除操作 _void **cancelRider(int id)**_

线性表 ：实时删除是 **_O( n )_**

有序表：实时删除是 **_O( logn )_**

不过此题不需要实时删除，更好的姿态是对需要删除的元素打个标记，实现 **_“懒”_** 删除，平均代价 **_O(1)_**

```
class RideSharingSystem {
    queue<int>driver;
    queue<int>rider;
    unordered_map<int,int>wait;
public:
    RideSharingSystem() {
        
    }
    
    void addRider(int id) {
        rider.push(id);
        wait[id]=1;
    }
    
    void addDriver(int id) {
        driver.push(id);
    }
    
    vector<int> matchDriverWithRider() {
        vector<int>ans={-1,-1};
        if(driver.empty()){
            return ans;
        }
        while(rider.size()&&wait[rider.front()]==0){
            wait.erase(rider.front());
            rider.pop();
        }
        if(rider.empty()){
            return ans;
        }
        ans[0]=driver.front();
        driver.pop();
        ans[1]=rider.front();
        rider.pop();
        return ans;
    }
    
    void cancelRider(int id) {
        if(wait.count(id)){
            wait[id]=0;
        }
    }
};
```

## D [移除一个元素后的最长交替子数组](https://leetcode.cn/problems/longest-alternating-subarray-after-removing-at-most-one-element/description/)

**_O(n)_** 预处理前后缀

**_O(n)_** 枚举删除位置 ，**_O(1)_** 合并

```
class Solution {
public:
    int longestAlternating(vector<int>& nums) {
        int n=nums.size();
        //pre[i][0] : 以 nums[i] 结尾，最后一对[i-1,i]是上升
        //pre[i][1] : 以 nums[i] 结尾，最后一对[i-1,i]是下降
        vector<vector<int>>pre(n,vector<int>(2,1));
        for(int i=1;i<n;++i){
            if(nums[i-1]<nums[i]){//[i-1,i]上升
                pre[i][0]=max(pre[i][0],pre[i-1][1]+1);
            }
            if(nums[i-1]>nums[i]){//[i-1,i]下降
                pre[i][1]=max(pre[i][1],pre[i-1][0]+1);
            }
        }
        //suf[i][0] : 以 nums[i] 开头，开头一对[i,i+1]是上升
        //suf[i][1] : 以 nums[i] 开头，开头一对[i,i+1]是下降
        vector<vector<int>>suf(n,vector<int>(2,1));
        for(int i=n-2;i>=0;--i){
            if(nums[i]>nums[i+1]){//[i,i+1]下降
                suf[i][1]=max(suf[i][1],suf[i+1][0]+1);
            }
            if(nums[i]<nums[i+1]){//[i,i+1]上升
                suf[i][0]=max(suf[i][0],suf[i+1][1]+1);
            }
        }
        int ans=1;
        for(int i=0;i<n;++i){
            ans=max({ans,pre[i][0],pre[i][1]});
        }
        for(int i=1;i<n-1;++i){
            if(nums[i-1]>nums[i+1]){//下降
                ans=max(ans,pre[i-1][0]+suf[i+1][0]);
            }
            if(nums[i-1]<nums[i+1]){//下降
                ans=max(ans,pre[i-1][1]+suf[i+1][1]);
            }
        }
        return ans;
    }
};
```
