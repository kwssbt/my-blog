---
title: "牛客周赛 132"
date: 2026-02-22
categories: 
  - "nowcode-contest"
tags: 
  - "周赛"
---

## A [ICPC Time](https://ac.nowcoder.com/acm/contest/128672/A)

```
void solve(){
    int n;
    cin>>n;
    cout<<(n+5)%24;
}
```

## B [倍数](https://ac.nowcoder.com/acm/contest/128672/B)

```
void solve(){
    int n;
    cin>>n;
    while(n){
        int t=n%10;
        if(t==5||t==0){
            cout<<"YES\n";
            return;
        }
        n/=10;
    }
    cout<<"NO\n";
}
```

## C [邻和](https://ac.nowcoder.com/acm/contest/128672/C)

**动态规划**

**_`dp[i][0]`_** ：考虑前 i 个数，不保留第 i 个数，的最大保留数

**_`dp[i][1]`_** ： 考虑前 i 个数，保留第 i 个数，的最大保留数

**状态转移**：

不保留：

`_dp[i][0] = max( dp[i-1][0], dp[i-1][1] )_`

保留：

**_`a[i]`_** 为 **1**，不可能保留，_**`dp[i][1] = -1`**_

**_`gcd(a[i-1],a[i])`_** 为 **1**，选择修改 **_`a[i-1]`_**、保留 **_`a[i]`_**，_**`dp[i][1] = dp[i-1][0] + 1`**_

**_`gcd(a[i-1],a[i])`_** > **1**， **_`a[i-1]`_**、 **_`a[i]`_** 均可保留，_**``dp[i][1] = `_max( dp[i-1][0], dp[i-1][1] )_` + 1``**_

```
int gcd(int a,int b){
    return b==0?a:gcd(b,a%b);
}
void solve(){
    int n;
    cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;++i)cin>>a[i];
    vector<vector<int>>dp(n,vector<int>(2));
    dp[0][0]=0;
    dp[0][1]=a[0]==1?-1:1;
    for(int i=1;i<n;++i){
        dp[i][0]=max(dp[i-1][0],dp[i-1][1]);
        if(a[i]==1){
            dp[i][1]=-1;
        }
        else{
            int g=gcd(a[i-1],a[i]);
            if(g==1){
                dp[i][1]=dp[i-1][0]+1;
            }
            else{
                dp[i][1]=dp[i][0]+1;
            }
        }
    }
    cout<<n-max(dp[n-1][0],dp[n-1][1])<<'\n';
}
```

## D [对和](https://ac.nowcoder.com/acm/contest/128672/D)

**数学**

⌊a+b2⌋\=a+b−r2\\left\\lfloor \\frac{a+b}{2} \\right\\rfloor = \\frac{a+b-r}{2}

每个数都会贡献 **n-1** 次 ，**a+b** 求和 = **sum\*(n-1)**

**a+b** 为奇 ：**r = 1**

**a+b** 为偶 ：**r = 0**

只有 **a,b** 奇偶性不同时 **r = 1**

**r** 求和 = 奇数项数量 \* 偶数项数量

```
void solve(){
    int n;
    cin>>n;
    ll sum=0;
    ll cnt=0;
    for(int i=0;i<n;++i){
        int t;
        cin>>t;
        sum+=t;
        cnt+=t&1;
    }
    cout<<(sum*(n-1)-cnt*(n-cnt))/2<<'\n';
}
```

## E [镜像](https://ac.nowcoder.com/acm/contest/128672/E)

**BFS**

```
int re(int x){
    int res=0;
    while(x){
        res=res*10+x%10;
        x/=10;
    }
    return res;
}
void solve(){
    int a,b,k;
    cin>>a>>b>>k;
    queue<int>q;
    q.push(a);
    int ans=-1;
    int M=b*10;//上界
    vector<char>vis(M);
    vis[a]=1;
    while(q.size()){
        ans++;
        int siz=q.size();
        for(int i=0;i<siz;++i){
            int u=q.front();
            q.pop();
            if(u==b){
                cout<<ans<<'\n';
                return;
            }
            int t=u+k;
            if(t<M&&vis[t]==0){
                vis[t]=1;
                q.push(t);
            }
            if(u%10){
                int t=re(u);
                if(t<M&&vis[t]==0){
                    vis[t]=1;
                    q.push(t);
                }
            }
        }
    }
    cout<<-1<<'\n';
}
```

## F [差异](https://ac.nowcoder.com/acm/contest/128672/F)

**我已急哭**
