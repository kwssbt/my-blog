---
title: "力扣周赛489"
date: 2026-02-15
categories: 
  - "leetcode-contest"
tags: 
  - "周赛"
---

## A [切换灯泡开关](https://leetcode.cn/problems/toggle-light-bulbs/)

**模拟**

```
class Solution {
public:
    vector<int> toggleLightBulbs(vector<int>& bulbs) {
        vector<int>on(101,0);
        for(int id:bulbs){
            on[id]^=1;
        }
        vector<int>ans;
        for(int i=1;i<=100;++i){
            if(on[i]){
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

## B [频率唯一的第一个元素](https://leetcode.cn/problems/first-element-with-unique-frequency/description/)

**模拟 + 哈希表**

```
class Solution {
public:
    int firstUniqueFreq(vector<int>& nums) {
        unordered_map<int,int>cnt;
        for(int x:nums){
            cnt[x]++;
        }
        if(cnt.size()==1)return nums[0];
        unordered_map<int,int>frq;
        for(auto [_,t]:cnt){
            frq[t]++;
        }
        if(frq.size()==1)return -1;
        int mf=0;
        int mt=0;
        for(auto [f,t]:frq){
            if(t>=mt){
                mf=f;
                mt=t;
            }
        }
        for(int x:nums){
            if(cnt[x]!=mf&&frq[cnt[x]]==1)return x;
        }
        return -1;
    }
};
```

## C [最长的准回文子字符串](https://leetcode.cn/problems/longest-almost-palindromic-substring/description/)

**暴力**

准回文串：删除一个字符后为回文串

判断是否为准回文串

对于回文串：删除最中心的字符后仍为回文串，符合定义

对于非回文串：尝试删除一个字符，判断是否为回文串

```
class Solution {
    bool check(string&s,int l,int r){
        while(l<=r){
            if(s[l]==s[r]){
                l++;
                r--;
            }
            else{
                return is(s,l+1,r)||is(s,l,r-1);
            }
        }
        return 1;
    }
    bool is(string&s,int l,int r){
        while(l<=r){
            if(s[l]==s[r]){
                l++;
                r--;
            }
            else{
                return 0;
            }
        }
        return 1;
    }
public:
    int almostPalindromic(string s) {
        int n=s.size();
        int ans=0;
        for(int l=0;l+ans<n;++l){
            for(int r=l+ans;r<n;++r){
                if(check(s,l,r)){
                    ans=max(ans,r-l+1);
                }
            }
        }
        return ans;
    }
};
```

## D [最大子数组异或值](https://leetcode.cn/problems/maximum-subarray-xor-with-bounded-range/description/)

**滑动窗口 + 单调队列 + 前缀和 + Trie树处理最大异或值**

数据结构拼好题

**滑动窗口 + 单调队列** 处理 "**最大值** 与 **最小值** 之间的差值不超过 **`k`** "

维护 **前缀和** 数组 **sum**，将 **子数组_\[l,r\]_的异或和** 转化为 **_sum\[l\]^sum\[r+1\]_**

**Trie树** 查询 [数组中两个数的最大异或值](https://leetcode.cn/problems/maximum-xor-of-two-numbers-in-an-array/)

```
class Solution {
    template<typename T>
    class MonotonicQueue{
    deque<T>q,m,M;
    public:
    void push(T e){
        q.push_back(e);
        while(!m.empty()&&m.back()>e)m.pop_back();
        m.push_back(e);
        while(!M.empty()&&M.back()<e)M.pop_back();
        M.push_back(e);
    }
    T max(){return M.front();}
    T min(){return m.front();}
    T pop(){
        T x=q.front();
        q.pop_front();
        if(x==m.front())m.pop_front();
        if(x==M.front())M.pop_front();
        return x;
    }
    int size()const{return q.size();}
    bool empty()const{return q.size()==0;}
    };
    class Trie{
        int n;
        struct node{
            int p;
            vector<node*>nex;
            node():p(0),nex(2,nullptr){}
        };
        node*root;
    public:
        Trie(int n):n(n){
            root=new node();
        }
        void add(int x){
            node*cur=root;
            for(int i=n-1;i>=0;--i){
                int j=(x>>i)&1;
                if(cur->nex[j]==nullptr)cur->nex[j]=new node();
                cur=cur->nex[j];
                cur->p++;
            }
        }
        void remove(int x){
            node*cur=root;
            for(int i=n-1;i>=0;--i){
                int j=(x>>i)&1;
                cur=cur->nex[j];
                cur->p--;
            }
        }
        int query(int x){
            int y=0;
            node*cur=root;
            for(int i=n-1;i>=0;--i){
                int u=(x>>i)&1;
                int v=u^1;
                if(cur->nex[v]&&cur->nex[v]->p){
                    y|=(v<<i);
                    cur=cur->nex[v];
                }
                else{
                    y|=(u<<i);
                    cur=cur->nex[u];
                }
            }
            return x^y;
        }
    };
public:
    int maxXor(vector<int>& nums, int k) {
        MonotonicQueue<int>qu;
        Trie st=Trie(15);
        int n=nums.size();
        vector<int>sum(n+1);
        for(int i=0;i<n;++i){
            sum[i+1]=sum[i]^nums[i];
        }
        int ans=ranges::max(nums);
        int l=0,r=0;
        while(r<n){
            st.add(sum[r]);
            qu.push(nums[r++]);
            while(qu.max()-qu.min()>k){
                st.remove(sum[l++]);
                qu.pop();
            }
            ans=max(ans,st.query(sum[r]));
        }
        return ans;
    }
};
```
