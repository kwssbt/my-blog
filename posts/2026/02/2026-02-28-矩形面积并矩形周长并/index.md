---
title: "矩形面积并+矩形周长并"
date: 2026-02-28
categories: 
  - "other"
  - "template"
tags: 
  - "others"
  - "模板"
coverImage: "010023301e41c000_2026-02-25_10-05-40-374.png"
---

**矩形面积并**

```
typedef long long ll;
const int N=3e5+5;
// 矩形
struct rectangle{
    int x1,y1,x2,y2; 
}rec[N/2];
// 扫描线
struct lines{
    int x,y1,y2,k;
}L[N];
int Ysort[N];
int total[N<<2];
int cover[N<<2];
int cnt[N<<2];
 
void up(int x){
    if(cnt[x]>0)cover[x]=total[x];
    else cover[x]=cover[x<<1]+cover[x<<1|1];
}
void built(int x,int l,int r){
    if(l<r){
        int mid=(l+r)>>1;
        built(x<<1,l,mid);
        built(x<<1|1,mid+1,r);
    }
    total[x]=Ysort[r+1]-Ysort[l];
    cover[x]=0;
    cnt[x]=0;
}
void change(int x,int l,int r,int ql,int qr,int k){
    if(ql<=l&&r<=qr){
        cnt[x]+=k;
    }else{
        int mid=(l+r)>>1;
        if(ql<=mid)change(x<<1,l,mid,ql,qr,k);
        if(qr>mid)change(x<<1|1,mid+1,r,ql,qr,k);
    }
    up(x);
}
int init(int n){
    for(int i=1,j=i+n;i<=n;++i,++j){
        L[i]={rec[i].x1,rec[i].y1,rec[i].y2,1};
        L[j]={rec[i].x2,rec[i].y1,rec[i].y2,-1};
        Ysort[i]=rec[i].y1;
        Ysort[j]=rec[i].y2;
    }
    int siz=n<<1;
    sort(L+1,L+siz+1,[&](auto a,auto b){return a.x<b.x;});
    sort(Ysort+1,Ysort+siz+1);
    int m=unique(Ysort+1,Ysort+siz+1)-(Ysort+1);
    Ysort[m+1]=Ysort[m];
    return m;
}
ll merge_S(int n){
    int m=init(n);
    built(1,1,m);
    ll ans=0;
    int pre=0;
    int siz=n<<1;
    for(int i=1;i<=siz;i++){
        ans+=1LL*cover[1]*(L[i].x-pre);
        pre=L[i].x;
        int l=lower_bound(Ysort+1,Ysort+m+1,L[i].y1)-Ysort;
        int r=lower_bound(Ysort+1,Ysort+m+1,L[i].y2)-Ysort-1;
        change(1,1,m,l,r,L[i].k);
    }
    return ans;
}
```

**矩形周长并**

```
typedef long long ll;
const int N=1e5+5;
// 矩形
struct rectangle{
    int x1,y1,x2,y2; 
}rec[N/2];
// 扫描线
struct lines_y{
    int x,y1,y2,k;
}Ly[N];
struct lines_x{
    int y,x1,x2,k;
}Lx[N];
int Ysort[N],Xsort[N];
int total[N<<2];
int cover[N<<2];
int cnt[N<<2];
 
void up(int x){
    if(cnt[x]>0)cover[x]=total[x];
    else cover[x]=cover[x<<1]+cover[x<<1|1];
}
void change(int x,int l,int r,int ql,int qr,int k){
    if(ql<=l&&r<=qr){
        cnt[x]+=k;
    }else{
        int mid=(l+r)>>1;
        if(ql<=mid)change(x<<1,l,mid,ql,qr,k);
        if(qr>mid)change(x<<1|1,mid+1,r,ql,qr,k);
    }
    up(x);
}
void built_y(int x,int l,int r){
    if(l<r){
        int mid=(l+r)>>1;
        built_y(x<<1,l,mid);
        built_y(x<<1|1,mid+1,r);
    }
    total[x]=Ysort[r+1]-Ysort[l];
    cover[x]=0;
    cnt[x]=0;
}
int init_y(int n){
    for(int i=1,j=i+n;i<=n;++i,++j){
        Ly[i]={rec[i].x1,rec[i].y1,rec[i].y2,1};
        Ly[j]={rec[i].x2,rec[i].y1,rec[i].y2,-1};
        Ysort[i]=rec[i].y1;
        Ysort[j]=rec[i].y2;
    }
    int siz=n<<1;
    sort(Ly+1,Ly+siz+1,[&](auto a,auto b){return a.x<b.x;});
    sort(Ysort+1,Ysort+siz+1);
    int m=unique(Ysort+1,Ysort+siz+1)-(Ysort+1);
    Ysort[m+1]=Ysort[m];
    return m;
}
ll scan_y(int n){
    int m=init_y(n);
    built_y(1,1,m);
    ll ans=0;
    int pre;
    int siz=n<<1;
    for(int i=1;i<=siz;i++){
        pre=cover[1];
        int l=lower_bound(Ysort+1,Ysort+m+1,Ly[i].y1)-Ysort;
        int r=lower_bound(Ysort+1,Ysort+m+1,Ly[i].y2)-Ysort-1;
        change(1,1,m,l,r,Ly[i].k);
        ans+=abs(pre-cover[1]);
    }
    return ans;
}
void built_x(int x,int l,int r){
    if(l<r){
        int mid=(l+r)>>1;
        built_x(x<<1,l,mid);
        built_x(x<<1|1,mid+1,r);
    }
    total[x]=Xsort[r+1]-Xsort[l];
    cover[x]=0;
    cnt[x]=0;
}
int init_x(int n){
    for(int i=1,j=i+n;i<=n;++i,++j){
        Lx[i]={rec[i].y1,rec[i].x1,rec[i].x2,1};
        Lx[j]={rec[i].y2,rec[i].x1,rec[i].x2,-1};
        Xsort[i]=rec[i].x1;
        Xsort[j]=rec[i].x2;
    }
    int siz=n<<1;
    sort(Lx+1,Lx+siz+1,[&](auto a,auto b){return a.y<b.y;});
    sort(Xsort+1,Xsort+siz+1);
    int m=unique(Xsort+1,Xsort+siz+1)-(Xsort+1);
    Xsort[m+1]=Xsort[m];
    return m;
}
ll scan_x(int n){
    int m=init_x(n);
    built_x(1,1,m);
    ll ans=0;
    int pre;
    int siz=n<<1;
    for(int i=1;i<=siz;i++){
        pre=cover[1];
        int l=lower_bound(Xsort+1,Xsort+m+1,Lx[i].x1)-Xsort;
        int r=lower_bound(Xsort+1,Xsort+m+1,Lx[i].x2)-Xsort-1;
        change(1,1,m,l,r,Lx[i].k);
        ans+=abs(pre-cover[1]);
    }
    return ans;
}
ll merge_C(int n){
    return scan_x(n)+scan_y(n);
}
```
