---
title: "质数"
date: 2026-02-09
categories: 
  - "math"
  - "template"
tags: 
  - "math"
  - "模板"
coverImage: "010023301e41c000_2026-02-04_12-14-43-155.png"
---

## 质数判断

### 朴素判断

判断 小 整数

O(n)O(\\sqrt{n})

```
bool isp(int n){
    for(int i=2;i*i<=n;++i){
        if(n%i==0)return 0;
    }
    return 1;
}
```

### Miller\_Rabin 素性测试

判断 高精度 整数

O(k(log⁡n)3)O( k (\\log n) ^ 3 )

k 为测试次数

[测试链接](https://www.luogu.com.cn/problem/U148828)

```
#include<bits/stdc++.h>
using namespace std;

typedef __int128 i128;

istream &operator>>(istream &is, __int128 &n) {
    string s;
    is >> s;
    n = 0;
    int sign = 1;
    int i = 0;
    if (s[0] == '-') {
        sign = -1;
        i = 1;
    }
    for (; i < (int)s.size(); i++) {
        n = n * 10 + (s[i] - '0');
    }
    n *= sign;
    return is;
}

i128 p[]={2,3,5,7,11,13,17,19,23,29,31,37,41,43};// 测试次数 k = 14

i128 mul(i128 a,i128 b,i128 mod){// 龟速乘(防溢出)
    i128 res=0;
    a%=mod;
    while(b){
        if(b&1)res=(res+a)%mod;
        a=(a<<1)%mod;
        b>>=1;
    }
    return res;
}

i128 qpow(i128 b,i128 e,i128 mod){
    i128 ans=1;
    b%=mod;
    while(e){
        if(e&1)ans=mul(ans,b,mod);
        b=mul(b,b,mod);
        e>>=1;
    }
    return ans;
}

bool mii128er_rabin(i128 n){
    if(n<3||n%2==0)return n==2;
    i128 u=n-1,t=0;
    while(u%2==0)u/=2,++t;
    for(auto a:p){
        if(n==a)return 1;
        if(n%a==0)return 0;
        i128 v=qpow(a,u,n);
        if(v==1)continue;
        i128 s=1;
        for(;s<=t;++s){
            if(v==n-1)break;
            v=mul(v,v,n);
        }
        if(s>t)return 0;
    }
    return 1;
}
```

## 质数筛

### 埃氏筛

O(nlog⁡(log⁡n))O(n \\log(\\log n))

```
const int N=1e5;

bitset<N>isp;
vector<int>p;//收集质数

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

如果只用于统计质数数量
```

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

### 欧拉筛

O(N)O ( N )

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

## 质因子分解

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
