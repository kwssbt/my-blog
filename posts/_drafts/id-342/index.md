---
title: "并查集"
categories: 
  - "data-struct"
  - "template"
tags: 
  - "data-struct"
  - "模板"
coverImage: "010023301e41c000_2026-02-04_12-15-03-769-e1770709969245.png"
draft: true
---

## 普通并查集

**路径压缩 + 大挂小 + 连通分量查询**

```
class UF{
    vector<int>fa,siz;
    int cnt;
public:
    UF(int n):cnt(n),fa(n),siz(n,1){
        for(int i=0;i<n;++i){
            fa[i]=i;
        }
    }
    int find(int x){
        return x==fa[x]?x:fa[x]=find(fa[x]);
    }
    bool check(int a,int b){
        return find(a)==find(b);
    }
    int merge(int a,int b){
        a=find(a);
        b=find(b);
        if(a==b)return 0;
        if(siz[a]>siz[b]){
            fa[b]=a;
            siz[a]+=siz[b];
        }
        else{
            fa[a]=b;
            siz[b]+=siz[a];
        }
        cnt--;
        return 1;
    }
    int count(){
        return cnt;
    }
};
```

## 带权并查集

```
template<class T>
class UF{
    vector<int>fa,siz;
    vector<T>dis;
public:
    UF(int n):fa(n),siz(n,1),dis(n,0){
        for(int i=0;i<n;++i){
            fa[i]=i;
        }
    }
    int find(int x){
        if(x!=fa[x]){
            int t=fa[x];
            fa[x]=find(t);
            dis[x]+=dis[t];
        }
        return fa[x];
    }
    bool merge(int u,int v,T w){
        int fu=find(u);
        int fv=find(v);
        if(fu==fv)return dis[u]-dis[v]==w;
        if(siz[fu]<=siz[fv]){
            fa[fu]=fv;
            siz[fv]+=siz[fu];
            dis[fu]=w+dis[v]-dis[u];
        }
        else{
            fa[fv]=fu;
            siz[fu]+=siz[fv];
            dis[fv]=dis[u]-dis[v]-w;
        }
        return 1;
    }
    bool check(int u,int v){
        return find(u)==find(v);
    }
    T get(int x){
        find(x);
        return dis[x];
    }
};
```

## 可持久并查集

## 可撤销并查集
