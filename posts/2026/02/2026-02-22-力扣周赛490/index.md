---
title: "力扣周赛490"
date: 2026-02-22
categories: 
  - "leetcode-contest"
tags: 
  - "周赛"
---

## A [计算比赛分数差](https://leetcode.cn/problems/find-the-score-difference-in-a-game/description/)

**模拟**

```
class Solution {
public:
    int scoreDifference(vector<int>& nums) {
        int n=nums.size();
        int a=0,b=0;
        bool f=1;
        for(int i=0;i<n;++i){
            if(nums[i]&1){
                f^=1;
            }
            if((i+1)%6==0){
                f^=1;
            }
            if(f){
                a+=nums[i];
            }
            else{
                b+=nums[i];
            }
        }
        return a-b;
    }
};
```

## B [阶数数字排列](https://leetcode.cn/problems/check-digitorial-permutation/)

**模拟 + 排序**

```
class Solution {
public:
    bool isDigitorialPermutation(int n) {
        vector<int>f{1,1,2,6,24,120,720,5040,40320,362880};
        int sum=0;
        int t=n;
        while(t){
            sum+=f[t%10];
            t/=10;
        }
        string a=to_string(sum);
        string b=to_string(n);
        sort(a.begin(),a.end());
        sort(b.begin(),b.end());
        return a==b;
    }
};
```

## C [重新排列后的最大按位异或值](https://leetcode.cn/problems/maximum-bitwise-xor-after-rearrangement/)

**模拟 + 贪心**

```
class Solution {
public:
    string maximumXor(string s, string t) {
        int a=0,b=0;
        for(char c:t){
            if(c=='1')a++;
            else b++;
        }
        string ans="";
        for(char c:s){
            if(c=='1'){
                if(b){
                    b--;
                    ans+='1';
                }
                else{
                    a--;
                    ans+='0';
                }
            }
            else{
                if(a){
                    a--;
                    ans+='1';
                }
                else{
                    b--;
                    ans+='0';
                }
            }
        }
        return ans;
    }
};
```

## D [统计结果等于 k 的序列数目](https://leetcode.cn/problems/count-sequences-to-k/description/)

**折半查找 + 分数运算**

```
class Solution {
    unsigned long long gcd(unsigned long long a,unsigned long long b){
        return b==0?a:gcd(b,a%b);
    }
    void simp(unsigned long long&de,unsigned long long&mo){
        unsigned long long g=gcd(de,mo);
        de/=g;
        mo/=g;
    }
public:
    int countSequences(vector<int>& nums, unsigned long long k) {
        int n=nums.size();
        map<pair<unsigned long long,unsigned long long>,int>L,R;
        function<void(int,int,unsigned long long,unsigned long long)>
        dfs_L=[&](int l,int r,unsigned long long de,unsigned long long mo)->void{
            if(l==r){
                simp(de,mo);
                L[{de,mo}]++;
                return;
            }
            dfs_L(l+1,r,de,mo);
            dfs_L(l+1,r,de*nums[l],mo);
            dfs_L(l+1,r,de,mo*nums[l]);
        };
        function<void(int,int,unsigned long long,unsigned long long)>
        dfs_R=[&](int l,int r,unsigned long long de,unsigned long long mo)->void{
            if(l==r){
                simp(de,mo);
                R[{de,mo}]++;
                return;
            }
            dfs_R(l+1,r,de,mo);
            dfs_R(l+1,r,de*nums[l],mo);
            dfs_R(l+1,r,de,mo*nums[l]);
        };
        dfs_L(0,n>>1,1,1);
        dfs_R(n>>1,n,1,1);
        int ans=0;
        for(auto [val,l]:L){
            auto [de,mo]=val;
            unsigned long long tar_mo=de*k;
            unsigned long long tar_de=mo;
            simp(tar_de,tar_mo);
            auto it=R.find({tar_de,tar_mo});
            if(it!=R.end()){
                int r=it->second;
                ans+=l*r;
            }
        }
        return ans;
    }
};
```
