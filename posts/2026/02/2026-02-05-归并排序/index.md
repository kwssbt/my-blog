---
title: "归并排序"
date: 2026-02-05
categories: 
  - "base-algorithms"
  - "algorithms"
tags: 
  - "基础算法"
coverImage: "010023301e41c000_2026-02-04_12-12-50-305-e1770301762516.png"
---

## 二叉树后序与归并排序

### 二叉树后序：

先处理左右子树，获取左右子树的信息

再借助左右子树的信息处理根节点

```
void postorder(TreeNode*root){
    if(root==nullptr){
        return;
    }
    //先处理左右子树
    postorder(root->left);
    postorder(root->right);
 
    //后序位置
}
```

### 归并排序：

先将整个区间一分为二，分别对左右区间排序，得到左右两个有序区间

再借助左右两个有序区间**归并**得到整个有序区间

```
void merge_sort(vector<int>&arr,int l,int r){
    //只有一个元素，本身就是有序的
    if(l>=r){
        return;
    }
    //先对左右区间排序
    int mid=(l+r)>>1;
    merge_sort(arr,l,mid);
    merge_sort(arr,mid+1,r);
    //归并
    my_merge(arr,l,r);
}
```

合并两个有序数组

```
vector<int>temp;//与 arr 等大

void my_merge(vector<int>&arr,int l,int mid,int r){
        int i=l,j=mid+1;
        int p=l;
        while(i<=mid&&j<=r){
            if(arr[i]<arr[j])temp[p++]=arr[i++];
            else temp[p++]=arr[j++];
        }
        while(i<=mid)temp[p++]=arr[i++];
        while(j<=r)temp[p++]=arr[j++];
        for(int k=l;k<=r;++k)arr[k]=temp[k];
    }
```

**时间复杂度：_O(NlogN)_**

**空间复杂度：_O(N)_**

**稳定性：_稳定排序_**

## 封装

```
class MergeSort{
    vector<int>temp;
    void merge_sort(vector<int>&arr,int l,int r){
        if(l>=r)return;
        int mid=(l+r)>>1;
        merge_sort(arr,l,mid);
        merge_sort(arr,mid+1,r);
        my_merge(arr,l,mid,r);
    }
    void my_merge(vector<int>&arr,int l,int mid,int r){
        int i=l,j=mid+1;
        int p=l;
        while(i<=mid&&j<=r){
            if(arr[i]<arr[j])temp[p++]=arr[i++];
            else temp[p++]=arr[j++];
        }
        while(i<=mid)temp[p++]=arr[i++];
        while(j<=r)temp[p++]=arr[j++];
        for(int k=l;k<=r;++k)arr[k]=temp[k];
    }
public:
    void sort(vector<int>&arr){
        int n=arr.size();
        if(n<=1)return;
        if(temp.size()<n){
            temp.resize(n);
        }
        merge_sort(arr,0,n-1);
    }
};
```

[测试链接](https://leetcode.cn/problems/sort-an-array/description/)

```
class Solution {
    class MergeSort{
    //
    };
public:
    vector<int> sortArray(vector<int>& nums) {
        MergeSort sorter;
        sorter.sort(nums);
        return nums;
    }
};
```

## 单函数写法

```
int a[],t[];
void merge_sort(int l,int r){
    if(l>=r)return;
    int mid=(l+r)>>1;
    merge_sort(l,mid);
    merge_sort(mid+1,r);
    int i=l,j=mid+1;
    int p=l;
    while(i<=mid&&j<=r){
        if(a[i]>a[j])t[p++]=a[j++];
        else t[p++]=a[i++];
    }
    while(i<=mid)t[p++]=a[i++];
    while(j<=r)t[p++]=a[j++];
    for(int k=l;k<=r;++k)a[k]=t[k];
}
```

## 归并分治

若发现：

整体的答案 = 左部分的答案 + 右部分的答案 + 跨越左右产生的答案

利用左、右部分的有序性可以快速计算 “ 跨越左右产生的答案 ”

那么：

此类问题通常可以通过归并分治求解

## 一些经典应用

[逆序对](https://www.luogu.com.cn/problem/P1908)

```
typedef long long ll;
const int N=5e5+5;
int a[N],t[N],n;
ll cnt=0;
void merge_sort(int l,int r){
    if(l>=r)return;
    int mid=(l+r)>>1;
    merge_sort(l,mid);
    merge_sort(mid+1,r);
    int i=l,j=mid+1;
    int p=l;
    while(i<=mid&&j<=r){
        if(a[i]>a[j]){
            cnt+=mid-i+1;
            t[p++]=a[j++];
        }
        else t[p++]=a[i++];
    }
    while(i<=mid)t[p++]=a[i++];
    while(j<=r)t[p++]=a[j++];
    for(int k=l;k<=r;++k)a[k]=t[k];
}
void solve(){
    cin>>n;
    for(int i=0;i<n;++i){
        cin>>a[i];
    }
    merge_sort(0,n-1);
    cout<<cnt;
}
```

[计算右侧小于当前元素的个数](https://leetcode.cn/problems/count-of-smaller-numbers-after-self/description/)

```
class Solution {
    vector<pair<int,int>>arr;
    vector<pair<int,int>>temp;
    vector<int>ans;
public:
    vector<int> countSmaller(vector<int>& nums) {
        int n=nums.size();
        arr.resize(n);
        temp.resize(n);
        ans.resize(n);
        for(size_t i=0;i<n;++i){
            arr[i].first=i;
            arr[i].second=nums[i];
        }
        sort(0,n-1);
        return ans;
    }
    void sort(int l,int r){
        if(l>=r)return;
        int mid=(l+r)>>1;
        sort(l,mid);
        sort(mid+1,r);
        int i=l,j=mid+1,p=l;
        while(i<=mid&&j<=r){
            if(arr[i].second<=arr[j].second){
                temp[p++]=arr[i];
                ans[arr[i].first]+=j-mid-1;
                ++i;
            }
            else temp[p++]=arr[j++];
        }
        while(i<=mid){
            temp[p++]=arr[i];
            ans[arr[i].first]+=r-mid;
            ++i;
        }
        while(j<=r)temp[p++]=arr[j++];
        for(int k=l;k<=r;++k)arr[k]=temp[k];
    }
};
```

[翻转对](https://leetcode.cn/problems/reverse-pairs/description/)

```
class Solution {
    vector<int>t;
    int ans=0;
public:
    int reversePairs(vector<int>& nums) {
        t.resize(nums.size());
        sort(nums,0,nums.size()-1);
        return ans;
    }
    void sort(vector<int>&n,int l,int r){
        if(l>=r)return;
        int mid=(l+r)>>1;
        sort(n,l,mid);
        sort(n,mid+1,r);
        int rr=mid+1;
        for(int i=l;i<=mid;++i){
            while(rr<=r&&(long)n[i]>(long)n[rr]*2)rr++;
            ans+=rr-mid-1;
        }
        int i=l,j=mid+1,p=l;
        while(i<=mid&&j<=r){
            if(n[i]<n[j])t[p++]=n[i++];
            else t[p++]=n[j++];
        }
        while(i<=mid)t[p++]=n[i++];
        while(j<=r)t[p++]=n[j++];
        for(int k=l;k<=r;k++)n[k]=t[k];
    }
};
```

[区间和的个数](https://leetcode.cn/problems/count-of-range-sum/description/)

```
class Solution {
    vector<long long>pre;
    vector<long long>t;
    int lower,upper,ans=0;
public:
    int countRangeSum(vector<int>& nums, int lower, int upper) {
        this->lower=lower;
        this->upper=upper;
        int n=nums.size();
        pre.resize(n+1);
        t.resize(n+1);
        for(int i=1;i<=n;++i)pre[i]=pre[i-1]+nums[i-1];
        sort(pre,0,n);
        return ans;
    }
    void sort(vector<long long>&n,int l,int r){
        if(l>=r)return;
        int mid=(l+r)>>1;
        sort(n,l,mid);
        sort(n,mid+1,r);
        int ll=mid+1,rr=mid+1;
        for(int i=l;i<=mid;++i){
            while(ll<=r&&n[ll]-n[i]<lower)ll++;
            while(rr<=r&&n[rr]-n[i]<=upper)rr++;
            ans+=rr-ll;
        }
        int i=l,j=mid+1,p=l;
        while(i<=mid&&j<=r){
            if(n[i]<n[j])t[p++]=n[i++];
            else t[p++]=n[j++];
        }
        while(i<=mid)t[p++]=n[i++];
        while(j<=r)t[p++]=n[j++];
        for(int k=l;k<=r;k++)n[k]=t[k];
    }
};
```
