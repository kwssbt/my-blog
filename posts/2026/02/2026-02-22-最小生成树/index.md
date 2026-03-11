---
title: "最小生成树"
date: 2026-02-22
categories: 
  - "graph"
tags: 
  - "graph"
coverImage: "010023301e41c000_2026-02-06_18-06-27-241-e1771746722988.png"
---

## 定义：

- 无向带权图

- **n** 个节点， **n - 1** 条边

- 所有节点**连通**

- 边权和最小

## Kruskal（加边）

- 对边排序

- 选 **n - 1** 条边

- 并查集避免成环

- `**_O(n+m) + O(m*logm)_**`

```
// 并查集
int fa[];
int find(int x){
    return fa[x]==x?x:fa[x]=find(fa[x]);
}
bool merge(int a,int b){
    a=find(a);
    b=find(b);
    if(a==b)return 0;
    fa[a]=b;
    return 1;
}
// 边
struct edge{
    int u,v;
    int w;
};
vector<edge>E;
// 无法连通全部 n 个节点：返回-1
int Kruskal(int n){
    sort(E.begin(),E.end(),[&](auto&a,auto&b){return a.w<b.w;});
    int cnt=0;
    int sum=0;
    for(auto&[u,v,w]:E){
        if(merge(u,v)){
            cnt++;
            sum+=w;
        }
    }
    return cnt==n-1?sum:-1;
}
```

## Prim（加点）

- 邻接表存图

- 选 **n** 个节点

- _**`vis[]`**_ 避免成环

- `**_O(n+m) + O(m*logm)_**`

```
// 图
struct st{
    int v;
    int cost;
};
vector<st>gra[];
// 无法连通全部 n 个节点：返回-1
int prim(int start,int n){
    vector<bool>vis(n);
    auto cmp=[](st a,st b){return a.cost>b.cost;};
    priority_queue<st,vector<st>,decltype(cmp)>pq(cmp);
    int sum=0;
    int cnt=1;
    vis[1]=1;
    for(auto nex:gra[1]){
        pq.push(nex);
    }
    while(pq.size()){
        auto [v,cost]=pq.top();
        pq.pop();
        if(vis[v]==0){
            vis[v]=1;
            cnt++;
            sum+=cost;
            for(auto nex:gra[v]){
                pq.push(nex);
            }
        }
    }
    return cnt==n?sum:-1;
}
```

## Prim 堆优化

- 链式前向星存图

- 手写堆

- 加点后更新堆里的信息

- `_**O(n+m)+O((m+n)*logn)**_`

```
const int N=5005;
const int M=400005;
 
int head[N];
int nex[M];
int to[M];
int weight[M];
int tot;
 
int heap[N][2];
int tag[N];
int siz;
int cnt;
 
void built(int n){
    tot=1;
    siz=0;
    cnt=0;
    for(int i=1;i<=n;++i){
        head[i]=0;
        tag[i]=-1;
    }
}
void addEdge(int u,int v,int w){
    nex[tot]=head[u];
    to[tot]=v;
    weight[tot]=w;
    head[u]=tot++;
}

void swap(int i,int j){
    int a=heap[i][0];
    int b=heap[j][0];
    tag[a]=j;
    tag[b]=i;
    int t0=heap[i][0];
    heap[i][0]=heap[j][0];
    heap[j][0]=t0;
    int t1=heap[i][1];
    heap[i][1]=heap[j][1];
    heap[j][1]=t1;
}
void swim(int i){
    while(heap[i][1]<heap[(i-1)/2][1]){
        swap(i,(i-1)/2);
        i=(i-1)/2;
    }
}
void sink(int i){
    int l=2*i+1;
    while(l<siz){
        int best=l+1<siz&&heap[l+1][1]<heap[l][1]?l+1:l;
        if(heap[best][1]>=heap[i][1])return;
        swap(i,best);
        i=best;
        l=2*i+1;
    }
}
pair<int,int>pop(){
    int u=heap[0][0];
    int w=heap[0][1];
    swap(0,--siz);
    sink(0);
    tag[u]=-2;
    cnt++;
    return {u,w};
}
void update(int e){
    int v=to[e];
    int w=weight[e];
    if(tag[v]==-1){
        heap[siz][0]=v;
        heap[siz][1]=w;
        tag[v]=siz++;
        swim(tag[v]);
    }
    else if(tag[v]!=-2){
        heap[tag[v]][1]=min(w,heap[tag[v]][1]);
        swim(tag[v]);
    }
}
int prim(int n){
    int sum=0;
    cnt=1;
    tag[1]=-2;
    for(int e=head[1];e;e=nex[e]){
        update(e);
    }
    while(siz){
        auto[u,w]=pop();
        sum+=w;
        for(int e=head[u];e;e=nex[e]){
            update(e);
        }
    }
    return cnt==n?sum:-1;
}
```

[**测试链接**](https://www.luogu.com.cn/problem/P3366)
