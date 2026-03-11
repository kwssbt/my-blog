---
title: "最短路算法"
date: 2026-02-25
categories: 
  - "graph"
  - "template"
tags: 
  - "graph"
  - "模板"
coverImage: "010023301e41c000_2026-02-25_10-43-11-739-e1771991536570.png"
---

## 链式前向星

空间复杂度 **_`O(N+M)`_**

```
const int N=1e5+5;// 结点
const int M=1e6+5;// 边

int tot;
int head[N];
int nex[M];
int to[M];
int weight[M];
// 初始化
void built(int n){
    tot=1;
    for(int i=1;i<=n;++i){
        head[i]=0;
    }
}
// 加边
void addEdge(int u,int v,int w){
    nex[tot]=head[u];
    to[tot]=v;
    weight[tot]=w;
    head[u]=tot++;
}
// 遍历
for(int ei=head[u];ei;ei=nex[ei]){
    int v=to[ei];
    int w=weight[ei];
}
```

## Floyd

- 得到图中 **任意两点** 的最点距离

- 可处理负边，不能处理负环

- 时间复杂度 **_`O(N^3)`_** 

```
const int N=105;
const int INF=0x3f3f3f3f;
// dis[u][v] : u -> v 的距离
int dis[N][N];
// 初始化
void init(int n){
    for(int i=1;i<=n;++i){
        for(int j=1;j<=n;++j){
            dis[i][j]=INF;
        }
    }
}
void floyd(int n){
    for(int k=1;k<=n;++k){
        for(int i=1;i<=n;++i){
            for(int j=1;j<=n;++j){
                if(dis[i][k]!=INF&&dis[k][j]!=INF&&dis[i][j]>dis[i][k]+dis[k][j]){
                    dis[i][j]=dis[i][k]+dis[k][j];
                }
            }
        }
    }
}
```

## Dijkstra

- 给定起点，得到起点到每个点的最短距离

- 不能处理负边

**普通堆实现**

时间复杂度 **_`O(MlogM)`_**

```
const int N=1e5+5;
const int INF=0x3f3f3f3f;
// 堆里存：起点 -> cur 的距离为 cost
struct st{
    int cur;
    int cost;
};
// 边
struct e{
    int v,w;
};
vector<e>gra[N];
bool vis[N];
int dis[N]; // dis[v] : 起点 -> v 的最短距离
// 初始化
void init(int n){
    for(int i=1;i<=n;++i){
        gra[i].clear();
        dis[i]=INF;
        vis[i]=0;
    }
}
void dijkstra(int s){
    auto cmp=[](st a,st b){return a.cost>b.cost;};
    priority_queue<st,vector<st>,decltype(cmp)>pq(cmp);
    dis[s]=0;
    pq.push({s,0});
    while(pq.size()){
        auto [u,c]=pq.top();
        pq.pop();
        if(vis[u]==0){
            vis[u]=1;
            for(auto [v,w]:gra[u]){
                int nc=c+w;
                if(dis[v]>nc){
                    dis[v]=nc;
                    pq.push({v,nc});
                }
            }
        }
    }
}
```

**反向索引堆实现**

时间复杂度 _**`O(MlogN)`**_

```
const int N=1e5+5;
const int M=2e5+5;
const int INF=0x3f3f3f3f;
int dis[N];
 
//链式前向星建图
int tot;
int head[N];
int nex[M];
int to[M];
int val[M];
 
//手写堆
int heap[N];
int siz;
int tag[N];
//tag=-1 : 没有进过堆 (vis=0)
//tag=-2 : 已出堆结算 (vis=1)
//tag>=0 : 反向索引，指出在堆里的位置
 
void init(int n){
    tot=1;
    siz=0;
    for(int i=1;i<=n;++i){
        dis[i]=INF;
        head[i]=0;
        tag[i]=-1;
    }
}
void addEdge(int u,int v,int w){
    nex[tot]=head[u];
    to[tot]=v;
    val[tot]=w;
    head[u]=tot++;
}
void swap(int i,int j){
    int t=heap[i];
    heap[i]=heap[j];
    heap[j]=t;
    tag[heap[i]]=i;
    tag[heap[j]]=j;
}
void up(int i){
    while(dis[heap[(i-1)/2]]>dis[heap[i]]){
        swap(i,(i-1)/2);
        i=(i-1)/2;
    }
}
void down(int i){
    int l=i*2+1;
    while(l<siz){
        int best=l+1<siz&&dis[heap[l+1]]<dis[heap[l]]?l+1:l;
        if(dis[heap[i]]<=dis[heap[best]])return;
        swap(best,i);
        i=best;
        l=2*i+1;
    }
}
int pop(){
    int u=heap[0];
    swap(0,--siz);
    down(0);
    tag[u]=-2;
    return u;
}
void push(int v,int c){
    if(tag[v]==-2)return;
    if(tag[v]==-1){
        heap[siz]=v;
        tag[v]=siz++;
        dis[v]=c;
    }
    else dis[v]=min(dis[v],c);
    up(tag[v]);
}
void dijkstra(int s){
    push(s,0);
    while(siz){
        int u=pop();
        for(int ei=head[u];ei;ei=nex[ei]){
            push(to[ei],dis[u]+val[ei]);
        }
    }
}
```

## Bellman-Frod 与 SPFA

- 给定起点，得到起点到每个点的最短距离

- 可以处理负边，可以检查负环

**Bellman-Frod**

时间复杂度 **_`O(NM)`_**

```
const int N=10005;
const int INF=0x3f3f3f3f;
// 边
struct e{
    int v,w;
};
vector<e>gra[N];
int dis[N];
 
void init(int n){
    for(int i=1;i<=n;++i){
        gra[i].clear();
        dis[i]=INF;
    }
}
void bellman_frod(int s,int n){
    dis[s]=0;
    for(int _=1;_<n;++_){//松弛 n-1 次
        for(int u=1;u<=n;++u){
            if(dis[u]==INF)continue;
            for(auto [v,w]:gra[u]){
                if(dis[v]>dis[u]+w){
                    dis[v]=dis[u]+w;
                }
            }
        }
    }
}
```

**SPFA**

时间复杂度 **_`O(NM)`_** ，常数优化效果明显

```
const int N=10005;
const int INF=0x3f3f3f3f;
// 边
struct e{
    int v,w;
};
vector<e>gra[N];
int dis[N];
bool in[N]; // in[u]=1：结点 u 在队列中
 
void init(int n){
    for(int i=1;i<=n;++i){
        gra[i].clear();
        in[i]=0;
        dis[i]=INF;
    }
}
void spfa(int s){
    queue<int>q;
    dis[s]=0;
    q.push(s);
    in[s]=1;
    while(q.size()){
        int u=q.front();
        q.pop();
        in[u]=0;
        for(auto [v,w]:gra[u]){
            if(dis[v]>dis[u]+w){
                dis[v]=dis[u]+w;
                if(in[v]==0){
                    q.push(v);
                    in[v]=1;
                }
            }
        }
    }
}
```

**检查负环**

一共有 **n** 个节点，若无负环，每个节点最多松弛 **n-1** 次

用一个 **`cnt[]`** 数组，记录每个节点松弛了多少次

若发现某个节点松弛次数大于 **n-1** ，说明有负环

若有规定起点 **s** ，以 **s** 为起点跑 **SPFA**

若没有规定起点，添加一个 **虚拟起点 S**

**虚拟起点 S** 连接其它所有的节点，边权为 **0**

```
//记录每个节点的松弛次数
int cnt[N];
//有负环返回 1 ,否则返回 0 
bool spfa(int s,int n){
    queue<int>q;
    q.push(s);
    in[s]=1;
    cnt[s]=1;
    dis[s]=0;
    while(q.size()){
        int u=q.front();
        q.pop();
        in[u]=0;
        for(auto [w,v]:gra[u]){
            if(dis[v]>dis[u]+w){
                if(cnt[v]==n)return 1;
                cnt[v]++;
                dis[v]=dis[u]+w;
                if(!in[v]){
                    q.push(v);
                    in[v]=1;
                }
            }
        }
    }
    return 0;
}
```

## Astra

- 给定起点，给定终点，求两点间最短距离

- **_`Dijkstra`_** + **预估函数** 实现 **启发式搜索**

- `**_Astar_**` 需要允许走回头路，相比 `_**Dijkstra**_` 删除 `**vis[]**` 机制

- 在堆中根据 **从 起点 到 cur 的距离 + 从 cur 到 终点 的预估距离** 进行排序

- 常用于网格图寻路：对实现预估函数友好

- 常用于抽象状态压缩

- 启发性算法讨论复杂度意义不大

**预估函数**

必须满足： 从 cur 到 终点 的**预估距离** <= 从 cur 到 终点 的**实际最短距离**

**预估距离** 与 **实际最短距离** 越接近，启发性越好

**常用预估方法（二维矩阵）**

- 曼哈顿距离

```
int f1(int i,int j,int x,int y){
    return abs(x-i)+abs(y-j);
}
```

- 欧氏距离

```
//向下取整
int f2(int i,int j,int x,int y){
    return sqrt((x-i)*(x-i)+(y-j)*(y-j));
}
```

- 对角线距离

```
int f3(int i,int j,int x,int y){
    return max(abs(x-i),abs(y-j));
}
```
