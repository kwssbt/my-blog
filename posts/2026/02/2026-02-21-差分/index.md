---
title: "差分"
date: 2026-02-21
categories: 
  - "base-algorithms"
tags: 
  - "基础算法"
coverImage: "010023301e41c000_2026-02-04_12-12-11-122-e1771659127769.png"
---

## 一维差分

对于原始数组 `**a[]**`

通过 `**d[i]=a[i]-a[i-1]**` 初始化出 **`d[]`** 差分数组

对差分数组进行若干次修改

```
// 对区间 [l,r] 加 k
void add(int l,int r,int k){
    d[l]+=k;
    d[r+1]-=k;
}
```

最后累加得到结果

```
// 下标从 1 到 n
void up(int n){
    for(int i=1;i<=n;++i){
        d[i]+=d[i-1];
        a[i]=d[i];
    }
}
```

或者直接在原始数组 **`a[]`** 上通过 `a[i]-=a[i-1]` 进行初始化

```
void add(int l,int r,int k){
    a[l]+=k;
    a[r+1]-=k;
}
void up(int n){
    for(int i=1;i<=n;++i){
        a[i]+=a[i-1];
    }
}
```

## 二维差分

为了避免若干边界问题

对于一个 **n\*m** 的原始数组 `**a[n][m]**`

我们加工出一个 **(n+2)\*(m+2)** 的差分数组 `**d[n+2][m+2]**`

最外层都用 **0** 包裹

```
//对以[i,j]为左上角,[x,y]为右下角的矩形 + k
void add(int i,int j,int x,int y,int k){
    d[i][j]+=k;
    d[i][y+1]-=k;
    d[x+1][j]-=k;
    d[x+1][y+1]+=k;
}
```

累加操作类比二维前缀和

```
void up(int n,int m){
    for(int i=1;i<=n;++i){
        for(int j=1;j<=m;++j){
            d[i][j]=d[i-1][j]+d[i][j-1]-d[i-1][j-1];
        }
    }
}
```

## 等差数列差分

在区间 **\[ l , r \]** 上加上一个以 **s** 为首项，**d** 为公差，**e** 为末项的等差数列

```
void add(int l,int r,int s,int d,int e){
    a[l]+=s;
    a[l+1]+=d-s;
    a[r+1]+=-d-e;
    a[r+2]+=e;
}
// 累加两次
void up(int n){
    for(int i=1;i<=n;++i){
        a[i]+=a[i-1];
    }
    for(int i=1;i<=n;++i){
        a[i]+=a[i-1];
    }
}
```
