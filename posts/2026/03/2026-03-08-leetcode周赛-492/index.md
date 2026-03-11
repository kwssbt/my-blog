---
title: "leetcode周赛 - 492"
date: 2026-03-08
categories: 
  - "leetcode-contest"
tags: 
  - "周赛"
---

## A [容量最小的箱子](https://leetcode.cn/problems/minimum-capacity-box/description/)

时间复杂度：_**`O(N)`**_

```
class Solution {
public:
    int minimumIndex(vector<int>& capacity, int itemSize) {
        int cap=INT_MAX;
        int id=-1;
        for(int i=0;i<capacity.size();++i){
            if(capacity[i]>=itemSize){
                if(capacity[i]<cap){
                    cap=capacity[i];
                    id=i;
                }
            }
        }
        return id;
    }
};
```

## B [找出最小平衡下标](https://leetcode.cn/problems/find-the-smallest-balanced-index/description/)

时间复杂度：_**`O(N)`**_

```
class Solution {
public:
    int smallestBalancedIndex(vector<int>& nums) {
        int n=nums.size();
        long long sum=0;
        for(int x:nums)sum+=x;
        long long mul=1;
        for(int i=n-1;i>=0;--i){
            sum-=nums[i];
            if(mul==sum){
                return i;
            }
            if(mul>sum){
                break;
            }
            mul*=nums[i];
        }
        return -1;
    }
};
```

## C [将一个字符串排序的最小操作次数](https://leetcode.cn/problems/minimum-operations-to-sort-a-string/description/)

**脑筋急转弯**

设原字符串 **s** 排序后的结果为 **t**

_**无需操作：**_

原字符串有序（ **_s == t_** ），最小操作 0 次

**_无法操作：_**

只有两个字符，且两个字符逆序，无法排序返回 -1

**_需操作：_**

设最小的字符为 **a**，最大的字符为 **z**

排序后的字符串 **t** 可以看作：**a** ………… **z**

操作次数只与 s 中 a，z 的位置有关

若 s 为 a …………

直接对除 a 外的所有字符进行一次排序，最小操作 1 次

同理 若 s 为 ………… z

直接对除 z 外的所有字符进行一次排序，最小操作 1 次

若 s 为 …… a ……

先对 a 及其左侧所有字符进行一次排序 ，得到 a …………

再对除 a 外的所有字符进行一次排序，最小操作 2 次

同理 若 s 为 …… z ……

先对 z 及其左侧所有字符进行一次排序 ，得到 ………… z

再对除 z 外的所有字符进行一次排序，最小操作 2 次

若 s 为 z ………… a

只能先对除 z 外所有字符进行一次排序，得到 z a …………

再对 z a 进行一次排序，得到 a z …………

再对除 a 外的所有字符进行一次排序，最小操作 3 次

或者先对除 a 外所有字符进行一次排序，得到 ………… z a

再对 z a 进行一次排序，得到 ………… a z

再对除 z 外的所有字符进行一次排序，最小操作 3 次

时间复杂度：**_`O(N)`_** 用于找到 a , z 的位置

```
class Solution {
public:
    int minOperations(string s) {
        int n=s.size();
        string t=s;
        ranges::sort(t);
        if(t==s)return 0;
        if(n==2)return -1;
        char a=t[0];
        char z=t[n-1];
        if(s[0]==a||s[n-1]==z)return 1;
        int pa=0;
        while(s[pa]!=a)pa++;
        if(pa!=n-1)return 2;
        int pz=n-1;
        while(s[pz]!=z)pz--;
        if(pz!=0)return 2;
        return 3;
    }
};
```

## D [划分二进制字符串的最小费用](https://leetcode.cn/problems/minimum-cost-to-partition-a-binary-string/description/)

**分治 + 记忆化递归** / **区间DP**

处理一个 **前缀和数组 `_presum_`** \[ \] 用于快速计算每个区间 **`1`** 的数量

对于一个区间 【 L , R 】

区间长度 len = R - L + 1

区间内 1 的数量 x = presum\[ R+1 \] - presum\[ L \]

若 x 为 0

区间费用只能是 **flatCost**

若 x 不为 0 且 len 为奇 ，不可划分

区间费用只能是 **x \* len \* encCost**

若 x 不为 0 且 len 为偶 ，可划分

区间费用为 **min( x \* len \* encCost , 左区间费用+右区间费用）**

时间复杂度：**_`O(NlogN)`_**

```
class Solution {
public:
    long long minCost(string s, int encCost, int flatCost) {
        int n=s.size();
        vector<int>presum(n+1);
        for(int i=0;i<n;++i){
            presum[i+1]=presum[i]+(s[i]-'0');
        }
        unordered_map<long long,long long>mem;
        function<long long(int,int)>DP=[&](int l,int r)->long long{
            if(l>r)return 0;
            long long key=(long long)l*n+r;
            if(mem[key]){
                return mem[key];
            }
            int x=presum[r+1]-presum[l];
            int len=r-l+1;
            if(x==0){
                return mem[key]=flatCost;
            }
            if(len&1){
                return mem[key]=(long long)len*x*encCost;
            }
            int mid=(l+r)>>1;
            return mem[key]=min((long long)len*x*encCost,DP(l,mid)+DP(mid+1,r));
        };
        return DP(0,n-1);
    }
};
```
