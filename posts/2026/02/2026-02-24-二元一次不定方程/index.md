---
title: "二元一次不定方程"
date: 2026-02-24
categories: 
  - "math"
tags: 
  - "math"
coverImage: "010023301e41c000_2026-02-04_12-11-21-897.png"
---

## 裴蜀定理

对于不全为零的整数 **a,b**，一定存在一组 **x,y**，使得 **_ax + by = gcd(a,b)_**

**推论：**

**ax + by = 1** 有解，当且仅当，**gcd(a,b)=1**, 即 **a,b** 互质

**ax + by = c** 有解，当且仅当，**gcd(a,b) | c**, 即 **c** 是 **gcd(a,b)** 的倍数

可由两项推广到多项

## 拓展欧几里得定理

**解 ax + by = gcd(a,b)**

```
//返回值为gcd(a, b)，同时求出满足 ax+by=gcd(a,b) 的一组解 x,y
 
int ex_gcd(int a,int b,int &x,int &y){//x,y传引用
    if(b==0){
        x=1,y=0;
        return a;
    }
    int g=ex_gcd(b,a%b,y,x);
    y-=a/b*x;
    return g;
}
```

## 二元一次方程

**解 ax + by = c**

**一、有解判定**

根据裴蜀定理：当且仅当 **gcd(a,b) | c** 时有解

**二、特解 X,Y**

根据拓展欧几里得定理：

先求 **x, y** ：**ax + by = gcd(a,b)**

**X = x \* c/gcd(a,b)**

**Y = y \* c/gcd(a,b)**

**三、通解**

**参数 kx,ky** ：

**kx = b / gcd(a,b)**

**ky = a / gcd(a,b)**

**通解 x,y** ：

**x = X + k \* kx**

**y = Y - k \* ky**

**四、最小正整数解**

**x = ( X % kx + kx ) % kx**
