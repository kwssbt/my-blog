---
title: "快速排序"
date: 2026-02-06
categories: 
  - "base-algorithms"
  - "algorithms"
tags: 
  - "基础算法"
coverImage: "010023301e41c000_2026-02-04_12-14-29-245-e1770305009656.png"
---

## 二叉树前序与快速排序

### 二叉树前序

先处理当前节点，再处理左右子树。

```
void preorder(TreeNode*root){
    if(root==nullptr){
        return;
    }

    //前序位置

    preorder(root->left);
    preorder(root->right);
}
```

### 快速排序（二路快排）

先对一个元素排序（确定其排序后的位置），再对其左右两侧排序。

```
void quick_sort(vector<int>&arr,int l,int r){
    //只有一个元素，本身就是有序的
    if(l>=r){
        return;
    }

    // 随机选择一个元素 x，对其排序，p 为 x 排序后的位置
    int x=arr[l+rand()%(r-l+1)];
    int p=partition(arr,l,r,x);
    // [l,p-1] 全部小于 x
    // [p,r] 全部大于等于 x

    // 对 x 左右两侧排序
    quick_sort(arr,l,p-1);
    quick_sort(arr,p,r);
}
```

**_partition_** 函数实现

维护左侧全部小于 **_x_** ，右侧全部大于等于 **_x_**

```
//返回第一个不小于 x 的位置
int partition(vector<int>&arr,int l,int r,int x){
    int i=l,j=r;
    while(i<=j){
        while(i<=j&&arr[i]<x)++i;
        while(i<=j&&arr[j]>x)--j;
        if(i<=j)swap(arr[i++],arr[j--]);
    }
    // [l,j]全部小于 x
    // [i,r]全部大于等于 x
    return i; //第一个 大与等于 x 的位置
}
```

时间复杂度：**_`_**O(NlogN)**_`_**

空间复杂度：`_**O(1)**_`

稳定性：**_非稳定排序_**

[测试链接](https://www.luogu.com.cn/problem/P1177)

```
//二路快排
int partition(vector<int>&arr,int l,int r,int x){
    while(l<=r){
        while(l<=r&&arr[l]<x)++l;
        while(l<=r&&arr[r]>x)--r;
        if(l<=r)swap(arr[l++],arr[r--]);
    }
    return l;
}
void quick_sort(vector<int>&arr,int l,int r){
    if(l>=r){
        return;
    }
    int x=arr[l+rand()%(r-l+1)];
    int p=partition(arr,l,r,x);
    quick_sort(arr,l,p-1);
    quick_sort(arr,p,r);
}
```

### 荷兰国旗优化（三路快排）

将待排序区间分成三部分：小于 **_x_**，等于 **_x_**，大于 **_x_**

```
void quick_sort(vector<int>&arr,int l,int r){
    if(l>=r){
        return;
    }
    int x=arr[l+rand()%(r-l+1)];
    int i=l,j=r;
    int p=l;
    while(p<=j){
        if(arr[p]<x){
            swap(arr[p++],arr[i++]);
        }
        else if(arr[p]>x){
            swap(arr[p],arr[j--]);
        }
        else p++;
    }
    quick_sort(arr,l,i-1);
    quick_sort(arr,j+1,r);
}
```

## 快速选择算法

查找无序数组中 **_第 k_ 小** 的数

通过 **_partition_** 函数 `p = partition ( x )`

可以得到 **_x_** 左侧有 **_p_** 个元素 小于 **_x_**

若 **_p > k_** ：**说明 _k 在 \[ l , p - 1 \]_** ，去左侧查找

否则：**_k 在 \[ p , r \]_**，去右侧查找

```
    int partition(vector<int>&arr,int l,int r,int x){
        while(l<=r){
            while(l<=r&&arr[l]<x)++l;
            while(l<=r&&arr[r]>x)--r;
            if(l<=r)swap(arr[l++],arr[r--]);
        }
        return l;
    }
    int quick_select(vector<int>&arr,int l,int r,int k){
        if(l>=r){
            return arr[l];
        }
        int x=arr[l+rand()%(r-l+1)];
        int p=partition(arr,l,r,x);
        if(p>k)return quick_select(arr,l,p-1,k);
        return quick_select(arr,p,r,k);
    }
```

时间复杂度 ：`_**O(n)**_`

空间复杂度：`_**O(1)**_`

[测试链接](https://leetcode.cn/problems/kth-largest-element-in-an-array/)
