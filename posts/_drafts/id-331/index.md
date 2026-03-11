---
title: "质数判断"
draft: true
---

## 朴素判断

O()O()

```
bool isp(int n){
    for(int i=2;i*i<=n;++i){
        if(n%i==0){
            return 0;
        }
    }
    return 1;
}
```

## Miller\_Rabin素性测试

- 判断高精度数

- $O(k(\\log n)^3)$ ，k为测试次数

- [测试链接](https://www.luogu.com.cn/problem/U148828)

```
#include<bits/stdc++.h>
using namespace std;

typedef __int128 LL;

template<typename T>void read(T&x){
    x=0;bool f=0;
    char ch=getchar();
    while(ch<'0'||ch>'9'){
        if(ch=='-')f=1;
        ch=getchar();
    }
    while(ch>='0'&&ch<='9'){
        x=(x<<3)+(x<<1)+(ch^48);
        ch=getchar();
    }
    if(f)x=-x;
}

LL p[]={2,3,5,7,11,13,17,19,23,29,31,37,41,43};//k = 14

LL mul(LL a,LL b,LL mod){//龟速乘(安全，防溢出)
    LL res=0;
    a%=mod;
    while(b){
        if(b&1)res=(res+a)%mod;
        a=(a<<1)%mod;
        b>>=1;
    }
    return res;
}
LL qpow(LL b,LL e,LL mod){
    LL ans=1;
    b%=mod;
    while(e){
        if(e&1)ans=mul(ans,b,mod);
        b=mul(b,b,mod);
        e>>=1;
    }
    return ans;
}
bool miller_rabin(LL n){
    if(n<3||n%2==0)return n==2;
    LL u=n-1,t=0;
    while(u%2==0)u/=2,++t;
    for(auto a:p){
        if(n==a)return 1;
        if(n%a==0)return 0;
        LL v=qpow(a,u,n);
        if(v==1)continue;
        LL s=1;
        for(;s<=t;++s){
            if(v==n-1)break;
            v=mul(v,v,n);
        }
        if(s>t)return 0;
    }
    return 1;
}
```

# 质数筛

- [模板 leetcode](https://leetcode.cn/problems/count-primes/description/)

- [模板 luogu](https://www.luogu.com.cn/problem/P3383)

## 埃氏筛

- $O(n \\log(\\log n))$

```
const int N=1e5;

bitset<N>isp;
vector<int>p;

void ehrlich(int n){
    isp.set();//初始化为1
    isp[0]=isp[1]=0;
    for(int i=2;i*i<=n;++i){
        if(isp[i]){
            p.push_back(i);
            for(int j=i*i;j<=n;j+=i){
                isp[j]=0;
            }
        }
    }
}
```

- 如果只用于统计质数数量

```
int ehrlich_cnt(int n){
    if(n<=1)return 0;
    vector<int>isp(n+1,1);
    int cnt=(n+1)/2;
    for(int i=3;i*i<=n;i+=2){
        if(isp[i]){
            for(int j=i*i;j<=n;j+=2*i){
                if(isp[j]){
                    isp[j]=0;
                    cnt--;
                }
            }
        }
    }
    return cnt;
}
```

## 欧拉筛

- $O(n)$

```
const int N=1e5;

bitset<N>isp;
vector<int>p;

void euler(int n){
    isp.set();
    isp[0]=isp[1]=0;
    for(int i=2;i<=n;++i){
        if(isp[i])p.push_back(i);
        for(int x:p){
            if(x*i>n)break;
            isp[x*i]=0;
            if(i%x==0)break;
        }
    }
}
```

# 质因子分解

```
vector<int>f(int n){
    vector<int>ans;
    for(int i=2;i*i<=n;++i){
        if(n%i==0){
            ans.push_back(i);
            while(n%i==0)n/=i;
        }
    }
    if(n>1)ans.push_back(n);
    return ans;
}
```

## 习题

[leetcode 925](https://leetcode.cn/problems/largest-component-size-by-common-factor/)

- 并查集 + 质因子分解

```
class Solution {
    vector<int>fa;
    vector<int>siz;
    void built(int n){
        fa.resize(n);
        siz.resize(n);
        for(int i=0;i<n;++i){
            fa[i]=i;
            siz[i]=1;
        }
    }
    int find(int x){
        if(x!=fa[x]){
            fa[x]=find(fa[x]);
        }
        return fa[x];
    }
    void merge(int a,int b){
        int A=find(a);
        int B=find(b);
        if(A==B)return;
        if(siz[A]>siz[B]){
            fa[B]=A;
            siz[A]+=siz[B];
        }
        else{
            fa[A]=B;
            siz[B]+=siz[A];
        }
    }
public:
    int largestComponentSize(vector<int>& nums) {
        int n=nums.size();
        built(n);
        unordered_map<int,int>st;
        for(int i=0;i<n;++i){
            int t=nums[i];
            for(int j=2;j*j<=t;++j){
                if(t%j==0){
                    if(st.count(j)){
                        merge(i,st[j]);
                    }
                    else st[j]=i;
                    while(t%j==0)t/=j;
                }
            }
            if(t>1){
                if(st.count(t)){
                    merge(i,st[t]);
                }
                else st[t]=i;
            }
        }
        int ans=0;
        for(int i=0;i<n;++i){
            if(i==fa[i]){
                ans=max(ans,siz[i]);
            }
        }
        return ans;
    }
};
```
