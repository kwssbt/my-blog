---
title: "单调栈"
date: 2026-02-20
categories: 
  - "data-struct"
  - "other"
  - "template"
tags: 
  - "基础算法"
  - "data-struct"
  - "others"
coverImage: "010023301e41c000_2026-02-06_18-07-39-123-e1771587434125.png"
---

获取每一个元素两侧第一个大于（小于）它的元素

```
const int N=1e5;
int a[N];
int top,st[N],L[N],R[N];
int n;
void init(){
    top=-1;
    for(int r=0;r<n;++r){
        while(top!=-1&&a[st[top]]<=a[r]){// "<=" 获取第一个大于a[i]的元素，">=" 获取第一个小于a[i]的元素
            int i=st[top--];
            L[i]=top==-1?-1:st[top];
            R[i]=r;
        }
        st[++top]=r;
    }
    while(top!=-1){
        int i=st[top--];
        L[i]=top==-1?-1:st[top];
        R[i]=n;
    }
    //处理重复元素
    for(int i=n-2;i>=0;--i){
        if(R[i]!=n&&a[R[i]]==a[i]){
            R[i]=R[R[i]];
        }
    }
}
```
