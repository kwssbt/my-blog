---
title: "同余与逆元"
date: 2026-02-24
categories: 
  - "math"
tags: 
  - "math"
coverImage: "010023301e41c000_2026-02-24_20-34-36-608.png"
---

## 同余运算

**加法同余：(a + b) %p = (a%p + b%p) %p**

**乘法同余：(a \* b) %p = (a%p) \* (b%p) %p**

**减法同余：(a - b) %p = (a%p - b%p + p) %p**

**除法同余与逆元**：

**逆元：**

若 **xy = 1 (mod P)** ，则在 **mod(P)** 意义下：**x,y** 互为逆元

**除法同余**：

**(a / b) %p = (a%p \* B%p) %p** ，其中 **B** 为 **b** 在 **mod(p)** 意义下的逆元

## 求逆元

**费马小定理（P 为质数）**

**X** 在 **mod(P)** 意义下的逆元为 **X ^ (P-2)**

```
// 快速幂
ll qpow(ll b,ll e,ll p){
    ll res=1;
    b%=p;
    while(e){
        if(e&1)res=ans*b%p;
        e>>=1;
        b=b*b%p;
    }
    return res;
}
// x 在 mod(p)意义下的逆元
ll inv(ll x,ll p){
    return qpow(x,p-2,p);
}
```

**拓展欧几里得（X 与 P 互质）**

```
int ex_gcd(int a,int b,int &x,int &y){
    if(b==0){
        x=1,y=0;
        return a;
    }
    int g=ex_gcd(b,a%b,y,x);
    y-=a/b*x;
    return g;
}
int inv(int X,int p){
    int x,y;
    ex_gcd(X,p,x,y);
    return (x%p+p)%p;
}
```

**线性求逆元**

预处理 **\[1 ,n\]** 在 **mod(P)** 意义下的逆元

```
const int N=1e8+5;
int inv[N];
void init(int n,int p){
    inv[1]=1;
    for(int i=2;i<=n;++i){
        inv[i]=1LL*(p-p/i)*inv[p%i]%p;
    }
}
```
