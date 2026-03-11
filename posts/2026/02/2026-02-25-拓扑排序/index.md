---
title: "拓扑排序"
date: 2026-02-25
categories: 
  - "graph"
tags: 
  - "graph"
coverImage: "010023301e41c000_2026-02-25_10-32-51-599-e1771987668123.png"
---

## 拓扑排序

- 有向无环图

- 可以判断环

## 流程

- 找到所有入度为 **0** 的点

- 将入度为 **0** 的点删除，同时将所有由它出发可以到达的点的入度减 **1**

- 重复，直到所有点入度都为 **0**

- 如果最后存在入度不为 **0** 的点，说明存在环

```
const int N=1e5;
 
vector<int>gra[N];
int in[N];
 
void top(int n){
    for(int u=1;u<=n;++u){
        for(int v:gra[u]){
            in[v]++;
        }
    }
    queue<int>q;
    for(int i=1;i<=n;++i){
        if(in[i]==0){
            q.push(i);
        }
    }
    while(q.size()){
        int u=q.front();
        q.pop();
        for(int v:gra[u]){
            if(--in[v]==0){
                q.push(v);
            }
        }
    }
}
```
