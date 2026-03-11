---
title: "组合数学"
draft: true
---

```
const int N=1e5+5;
const int P=1e9+7;
ll qpow(ll b,ll e){
    ll ans=1;
    b%=P;
    while(e){
        if(e&1)ans=ans*b%P;
        e>>=1;
        b=b*b%P;
    }
    return ans;
}
ll f[N];//f[i]表示i!在(mod p)意义下的余数
ll inv[N];//inv[i]表示i！在(mod p)意义下的逆元
void built(){
    f[0]=f[1]=1;
    for(int i=2;i<=N;++i){
        f[i]=1LL*i*f[i-1]%P;
    }
    inv[N]=qpow(f[N],P-2);
    for(int i=N;i;--i){
        inv[i-1]=1LL*i*inv[i]%P;
    }
}
//（mod p）意义下的组合数
ll C(int n,int m){// n! / m! / (n-m)!
    ll ans=f[n];
    ans=ans*inv[m]%P;
    ans=ans*inv[n-m]%P;
    return ans;
}
```
