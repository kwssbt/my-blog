---
title: "二分"
date: 2026-01-31
categories: 
  - "base-algorithms"
  - "algorithms"
tags: 
  - "bserach"
coverImage: "010023301e41c000_2025-12-21_00-20-07-734-scaled-e1770036255923.png"
---

> ## 二分查找
> 
> 在给定 **递增**数组 中查找是否存在某元素
> 
> - 存在：返回下标
> 
> - 不存在：返回 **\-1**

```

int binarySearch(vector<int>&nums,int target){
    int l=0,r=nums.size()-1,mid;
    while(l<=r){
        mid=(l+r)>>1;//位运算：二进制下右移一位,等价于mid=(l+r)/2
        if(nums[mid]==target)return mid;
        else if(nums[mid]>target)r=mid-1;
        else l=mid+1;
    }
    return -1;
}
```

在给定数组(**不一定有序**)中查找是否存在某元素

- 存在：返回 1

- 不存在：返回 0

```
bool binarySearch(vector<int>&nums,int target){
    sort(nums.begin(),nums.end());
    int l=0,r=nums.size()-1,mid;
    while(l<=r){
        mid=(l+r)>>1;
        if(nums[mid]==target)return 1;
        else if(nums[mid]>target)r=mid-1;
        else l=mid+1;
    }
    return 0;
}
//为了避免(l+r)造成的整型溢出
//优化：mid=l+(r-l)>>1
```

**二分的应用依赖有序性**

**基于有序性，查找失败后可以缩小一半的查找范围，实现O(logn)查找**

## 抽象有序性

给定一个 _**bool**_ 数组 **_\[ 0 , 0 , 0 , 0 , 0 , 0 , 0 , 1 , 1 , 1 , 1 , 1 , 1 \]_**

要求返回第一个1的下标

题目保证存在至少一个 **1**

```
int solve(vector<bool>&abstract){
    int l=0,r=abstract.size()-1,mid,ans;
    while(l<=r){
        mid=l+(r-l)>>1;
        if(abstract[mid])ans=mid,r=mid-1;
        else l=mid+1;
    }
    return ans;
}
```

同理我们可以找到最后一个0的下标

我们如果遇到这样的问题：

- 存在某种**有序性**

- 求临界条件（一般是求满足条件的**最大的最小值，最小的最大值**）

一般都可以抽象出一个bool函数

**框架：**

```
bool check(int x){
    
}
void solve(){
    //[min_ans,max_ans]：初始查找范围
    int l=min_ans,r=max_ans,mid,ans;
    while(l<=r){
        mid=l+(r-l)/2;
        if(check(mid))ans=mid,…………;
        else …………;
    }
    printf("%d\n",ans);
}
```

根据题目逻辑实现 **check 函数**和**范围修改**

**例题：序列划分**

**题目描述**:

给定 **n** 个正整数 a\_1,a\_2,……,a\_n，将这个序列从左到右划分成 **m** 段，使得每段至少有一个数。

你需要让数字之和最大的那一段的数字和尽可能得小。

**Input**

第一行包含一个正整数 **T** (1<=T<=10)，表示测试数据的组数。

每组数据第一行包含两个正整数 **n,m** (1<= m<= n<= 100000)。

第二行包含 **n** 个正整数 a\_1,a\_2,……,a\_n (1<= a\_i<= 10^9)。

**Output**

对于每组数据输出一行一个整数，即你找到的方案中，数字之和最大的那一段的数字和。

**Sample Input**

1

6 4

1 5 4 6 2 3

**Sample Output**

6

**思路：**

1. **发现单调性：** 设答案为 **_ans_** 。随着 **_ans_** 减小，能分得的最大段数增多（不减少）

3. **抽象 bool 函数：** **段数小于等于 m 时合法**（数字之和小于 **_ans_** 的一段分成的两段的数字之和仍然小于 **_ans_** ，这意味着当段数小于 **_m_** 时可以通过分裂操作将段数调整至 **_m_** ,这是合法的） **段数大于 _m_ 时非法**

5. **确定初始范围： _ans_** 不小于 **_a\[1,2,……,n\]_** 的最大值（ **_m = n_** 时） **_ans_** 不大于 **_a \[1,2,……,n\]_** 的总和（ **_m =1_**时）

7. **设计 _check_ 函数：** 贪心： 从左到右累加，一旦累加和大于**_ans_**就把当前元素当作下一个区间的第一个元素 。**_O( n )_** 算出**_ans_** 情况下能分得的最大段数

9. **二分答案：** **_O( n logn )_** 找到最小的 **_ans_**

```

typedef long long ll;
const int N=100005;
int n,m;
ll a[N];
bool check(ll x){
    ll cnt=1;
    ll cur_sum=0;
    for(int i=0;i<n;++i){
        if(cur_sum+a[i]<=x){
            cur_sum+=a[i];
        }
        else{
            cur_sum=a[i];
            cnt++;
        }
    }
    return cnt<=m;
} 
void solve(){
    cin>>n>>m;
    ll l=-1;
    ll r=0;
    for(int i=0;i<n;++i){
        cin>>a[i];
        r+=a[i];
        if(a[i]>l)l=a[i];
    }
    ll mid,ans=r;
    while(l<=r){
        mid=l+(r-l)/2;
        if(check(mid))ans=mid,r=mid-1;
        else l=mid+1;
    }
    cout<<ans<<'\n';
}
```
