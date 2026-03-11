---
title: "牛客周赛129"
date: 2026-02-01
categories: 
  - "nowcode-contest"
tags: 
  - "周赛"
---

## A [小红的大小判断](https://ac.nowcoder.com/acm/contest/127264/A)

```
void solve(){
    int n;
    cin>>n;
    if(n<1)cout<<"left";
    else if(n==1)cout<<"equal";
    else cout<<"right";
}
```

## B [小红的大小再判断](https://ac.nowcoder.com/acm/contest/127264/B)

```
void solve(){
    string s;
    cin>>s;
    string t=s;
    reverse(t.begin(),t.end());
    if(s>t)cout<<"left";
    else if(s==t)cout<<"equal";
    else cout<<"right";
}
```

## C [小红的肥鹅健身房](https://ac.nowcoder.com/acm/contest/127264/C)

**_map_** 迭代

```
void solve(){
    int n,m,k;
    ll ans1=0;
    ll ans2=0;
    cin>>n>>m>>k;
    map<int,int>cnt;
    for(int i=0;i<n;++i){
        for(int j=0;j<m;++j){
            int t;
            cin>>t;
            if(t==0)continue;
            cnt[t]++;
        }
    }
    bool f=1;
    while(f){
        f=0;
        vector<int>temp;
        for(auto [i,_]:cnt){
            temp.push_back(i);
        }
        for(int i:temp){
            int t=cnt[i]/2;
            if(t){
                f=1;
                ans1+=t;
                ans2+=(i+1>=k)?t:0;
                cnt[i+1]+=t;
                cnt[i]-=t*2;
            }
        }
    }
    cout<<ans1<<' '<<ans2;
}
```

## D [小红的神秘密码解锁](https://ac.nowcoder.com/acm/contest/127264/D)

将一个颜色串分为三部分：_**\[ 左 , 中 , 右 \]**_ : 最左端 、中间、最右端

考虑 _**\[ l , r \]**_ 位于一个颜色串上

枚举可能的合法情况，有以下几种：

**_\[ l左，r左 \]_，** **_\[ l左 ，r中 \]， \[ l中，r右 \]， \[ l右 ，r右 \]_**

考虑 _**\[ l , r \]**_ 位于不同颜色串上：

对于完全包含于 _**\[ l , r \]**_ 的颜色串 ，对颜色串改变量的贡献为 **_0_**

等价于 _**\[ l , r \]**_ 位于相邻两个颜色串上

枚举可能的合法情况，有以下几种：

**_\[ l左，r左 \]_，** **_\[ l左 ，r中 \]， \[ l中，r右 \]， \[ l右 ，r右 \]_**

和上面完全相同，合并

答案为 ：**_L左 （ R左 + R中 ）+ R右 （ L 中 + L 右 ）_**

发现 **_L_** 与 **_R_** 对称

进一步化简为 **_一侧端点数 \* 非此端点的元素数量 / 2_** 累加

只算一端可省去 **_/ 2_**

```
void solve(){
    string s;
    cin>>s;
    int n=s.size();
    int a=0,b=0;
    for(int i=0;i+1<n;++i){
        if(s[i]==s[i+1])a++;
        else b++;
    }
    cout<<1LL*a*b+1;
}
```

## E [小红的多维空间冒险](https://ac.nowcoder.com/acm/contest/127264/E)

对于每一位 ： _**0**_ 和 **_1_** 都各有 _**2 ^ ( n - 1 )**_ 个

在 **_n_** 对**_（0，1）_**中找出 **_k_** 对， 有 **_C ( n , k )_** 种组合

对于 **_k_** ：答案为 _**2 ^ ( n - 1 ) \* C ( n , k )**_

```
const int P=1e9+7;
ll qpow(ll b,ll e){
    ll ans=1;
    while(e){
        if(e&1)ans=ans*b%P;
        b=b*b%P;
        e>>=1;
    }
    return ans;
}
void solve(){
    int n;
    cin>>n;
    ll t=1;
    for(int i=1;i<n;++i){
        t=(t<<1)%P;
    }
    ll cur=1;
    for(int i=1;i<=n;++i){
        cur=cur*(n-i+1)%P;
        cur=cur*qpow(i,P-2)%P;
        cout<<t*cur%P<<' ';
    }
}
```

## F [小红的魔法树探险](https://ac.nowcoder.com/acm/contest/127264/F)

到达节点 _**v**_ 的概率 = 到达节点 _**parent\[ v \]**_ 的概率 _**/**_ **_parent\[ v \]_** 的度数

累加到达每个节点的概率 = 到达结点数的期望

```
const int N=2e5+5;
const int P=1e9+7;
vector<int>gra[N];
ll qpow(ll b,ll e){
    ll res=1;
    while(e){
        if(e&1)res=res*b%P;
        b=b*b%P;
        e>>=1;
    }
    return res;
}

void solve(){
    int n;
    cin>>n;
    for(int i=1;i<n;++i){
        int u,v;
        cin>>u>>v;
        gra[v].push_back(u);
        gra[u].push_back(v);
    }
    queue<int>q;
    vector<ll>val(n+1);
    vector<char>vis(n+1);
    q.push(1);
    val[1]=1;
    vis[1]=1;
    while(q.size()){
        int u=q.front();
        q.pop();
        for(int v:gra[u]){
            if(vis[v]==0){
                vis[v]=1;
                val[v]=val[u]*qpow(gra[u].size(),P-2)%P;
                q.push(v);
            }
        }
    }
    ll ans=0;
    for(int i=1;i<=n;++i){
        ans=(ans+val[i])%P;
    }
    cout<<ans;
}

```

## 头文件

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;

void solve(){
    
}

int main(){
    cin.tie(0)->sync_with_stdio(0);
    int T=1;
    //cin>>T;
    while(T--)solve();
}
```
