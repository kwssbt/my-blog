---
title: "记数排序与基数排序"
date: 2026-02-07
categories: 
  - "base-algorithms"
  - "algorithms"
tags: 
  - "基础算法"
coverImage: "010023301e41c000_2026-02-04_12-16-58-507-e1770426513668.png"
---

记数排序，基数排序都是不基于比较的排序算法

一般只用于整数排序

## 记数排序

用于数值范围较小的排序

```
class Cnt_Sort{
    vector<int>temp;
    void cnt_sort(vector<int>&arr,int n,int len){
        vector<int>cnt(len);
        for(int i=0;i<n;++i){
            cnt[arr[i]]++;
        }
        for(int i=1;i<len;++i){
            cnt[i]+=cnt[i-1];
        }
        for(int i=n-1;i>=0;--i){
            temp[--cnt[arr[i]]]=arr[i];
        }
        for(int i=0;i<n;++i){
            arr[i]=temp[i];
        }
    }
public:
    void sort(vector<int>&arr){
        int n=arr.size();
        if(n<2)return;
        if(temp.size()<n)temp.resize(n);
        int m=arr[0];
        for(int i=0;i<n;++i){
            if(arr[i]<m)m=arr[i];
        }
        int M=0;
        for(int i=0;i<n;++i){
            arr[i]-=m;
            if(arr[i]>M)M=arr[i];
        }
        cnt_sort(arr,n,M-m);
        for(int i=0;i<n;++i){
            arr[i]+=m;
        }
    }
};
```

时间复杂度：**_`O(N)`_**

空间复杂度：**_`O(M + N)`_**

_**`N`**_ 为数组长度，_**`M`**_ 为数值范围

稳定性：**_稳定排序_**

## 基数排序

逐位记数排序

```
class Base_Sort{
    int BASE;
    vector<int>cnt;
    vector<int>temp;
    int bits(int x){
        int res=0;
        while(x){
            res++;
            x/=BASE;
        }
        return res;
    }
    void radix_sort(vector<int>&arr,int n,int T){
        int offset=1;
        while(T--){
            for(int i=0;i<BASE;++i)cnt[i]=0;
            for(int i=0;i<n;++i){
                cnt[(arr[i]/offset)%BASE]++;
            }
            for(int i=1;i<BASE;++i){
                cnt[i]+=cnt[i-1];
            }
            for(int i=n-1;i>=0;--i){
                temp[--cnt[(arr[i]/offset)%BASE]]=arr[i];
            }
            for(int i=0;i<n;++i){
                arr[i]=temp[i];
            }
            offset*=BASE;
        }
    }
public:
    Base_Sort(int base):BASE(base){
        cnt.resize(base);
    }
    void sort(vector<int>&arr){
        int n=arr.size();
        if(n<=1)return;
        if(temp.size()<n)temp.resize(n);
        int m=arr[0];
        for(int i=1;i<n;++i){
            if(arr[i]<m)m=arr[i];
        }
        int M=0;
        for(int i=0;i<n;++i){
            arr[i]-=m;
            if(arr[i]>M)M=arr[i];
        }
        radix_sort(arr,n,bits(M));
        for(int i=0;i<n;++i){
            arr[i]+=m;
        }
    }
};
```

时间复杂度：**_`O(NlogN)`_**

空间复杂度：**_`O(N)`_**

稳定性：**_稳定排序_**
