---
title: "堆结构与堆排序"
date: 2026-02-06
categories: 
  - "base-algorithms"
  - "algorithms"
tags: 
  - "基础算法"
coverImage: "010023301e41c000_2026-02-04_12-14-55-401-e1770348548114.png"
---

## 用数组模拟完全二叉树

**对于下标为 i 的节点：**

父节点： **_( i - 1 ) / 2_**

左孩子：**_2 \* i + 1_**

右孩子：_**2 \* i + 2**_

下标 **_0_** 为根节点，其父节点为自己。

## 维护大根堆

**向上归位**

若 节点 **_i_** 大于其父节点 ，交换位置，继续尝试向上。

当 节点 **_i_** 小于等于其父节点，说明当前节点已归位，停止。

或者来到根节点 **_0_** ，它等于其父节点(自己)，停止。

```
void up(int arr[],int i){
    while(arr[i]>arr[(i-1)/2]){
        swap(arr[i],arr[(i-1)/2]);
        i=(i-1)/2;
    }
}
```

**向下归位**

若 节点 **_i_** 小于其最大的孩子，交换位置，继续向下尝试。

当 节点 **_i_** 大于其所有孩子，说明当前节点已归位，停止。

或者来到叶子节点，停止。

```
void down(int arr[],int i,int siz){
    int l=2*i+1;
    while(l<siz){
        int best=l+1<siz&&arr[l+1]>arr[l]?l+1:l;
        if(arr[best]<=arr[i])return;
        swap(arr[i],arr[best]);
        i=best;
        l=i*2+1;
    } 
}
```

## 数组原地建堆

从最后一个非叶子节点尝试向下归位

```
for(int i=(arr.size()-1)/2;i>=0;--i){
    down(i);
}
```

## 堆排序

建立大根堆。

不断取出堆顶（最大值），与堆的末位元素交换位置。

堆的大小减一，新的堆顶向下归位。

```
void heap_sort(vector<int>&nums){
    for(int i=(nums.size()-1)/2;i>=0;--i){
        down(i);
    }
    for(int i=nums.size()-1;i>=0;--i){
        swap(nums[i],nums[0]);
        down(0);
    }
}
```

时间复杂度：**_`O(NlogN)`_**

空间复杂度：**_`O(1)`_**

稳定性：**_非稳定排序_**

## 手写动态大根堆 Max\_Heap

```
class Max_Heap{
    vector<int>arr;
    void up(int i){
        while(arr[i]>arr[(i-1)/2]){
            swap(arr[i],arr[(i-1)/2]);
            i=(i-1)/2;
        }
    }
    void down(int i){
        int l=2*i+1;
        while(l<arr.size()){
            int best=l+1<arr.size()&&arr[l+1]>arr[l]?l+1:l;
            if(arr[best]<=arr[i])return;
            swap(arr[i],arr[best]);
            i=best;
            l=i*2+1;
        } 
    }
public:
    Max_Heap(const vector<int>&nums):arr(nums){
        for(int i=(arr.size()-1)/2;i>=0;--i){
            down(i);
        }
    }
    int top(){
        return arr[0];
    }
    void push(int x){
        arr.push_back(x);
        up(arr.size()-1);
    }
    void pop(){
        swap(arr[0],arr.back());
        arr.pop_back();
        down(0);
    }
    int size(){
        return arr.size();
    }
    bool empty(){
        return arr.size()==0;
    }
    void heap_sort(vector<int>&nums){
        Max_Heap heap(nums);
        for(int i=nums.size();i>=0;--i){
            nums[i]=heap.top();
            heap.pop();
        }
    }
};
```
