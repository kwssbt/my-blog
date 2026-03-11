---
title: "懒删除堆"
date: 2026-02-06
categories: 
  - "data-struct"
  - "template"
tags: 
  - "data-struct"
  - "模板"
coverImage: "010023301e41c000_2026-02-04_12-14-13-794-e1770383252382.png"
---

## 在堆上动态删除元素

**c++** 标注库提供的 **_priority\_queue_** 只能通过 `.pop()` 弹出堆顶元素。

代价： **_`O(logN)`_**

手写的堆可以在数组中查找需要删除的元素，删除后维护堆的性质。

代价：

二分查找 **_`O(logN)`_**

与堆的最后一个元素交换位置，堆的大小减一，向下归位 **_`O(logN)`_**

## 懒删除

对需要删除的元素打上标记。

访问堆顶前，若堆顶元素有标记就提前弹出，直至堆顶元素没有标记。

均摊代价： **_`O(1)`_**

```
template<typename T,typename cmp=less<T>>
    class LazyHeap{
        priority_queue<T,vector<T>,cmp>pq;
        unordered_map<T,int>tag;
        size_t siz=0;//真实大小
        void up(){
            while(pq.size()&&tag[pq.top()]>0){
                tag[pq.top()]--;
                pq.pop();
            }
        }
    public:
        size_t size(){
            return siz;
        }
        void erase(T x){//懒删除
            tag[x]++;
            siz--;
        }
        T top(){
            up();
            return pq.top(); 
        }
        void pop(){
            up();
            pq.pop();
            siz--;
        }
        void push(T x){
            pq.push(x);
            siz++;
        }
    };
```

## 应用

[滑窗中位数](https://leetcode.cn/problems/sliding-window-median/)

```
class Solution {
    template<typename T,typename cmp=less<T>>
    class LazyHeap{
        priority_queue<T,vector<T>,cmp>pq;
        unordered_map<T,int>tag;
        size_t siz=0;
        void up(){
            while(pq.size()&&tag[pq.top()]>0){
                tag[pq.top()]--;
                pq.pop();
            }
        }
    public:
        size_t size(){
            return siz;
        }
        void erase(T x){
            tag[x]++;
            siz--;
        }
        T top(){
            up();
            return pq.top(); 
        }
        void pop(){
            up();
            pq.pop();
            siz--;
        }
        void push(T x){
            pq.push(x);
            siz++;
        }
    };
    LazyHeap<int>L;
    LazyHeap<int,greater<>>R;
    vector<double>ans;
    void add(int val){
        if(L.size()<=R.size()){
            R.push(val);
            L.push(R.top());
            R.pop();
        }
        else{
            L.push(val);
            R.push(L.top());
            L.pop();
        }
    }
    void remove(int val){
        if(val<=L.top()){
            L.erase(val);
            if(L.size()<R.size()){
                L.push(R.top());
                R.pop();
            }
        }
        else{
            R.erase(val);
            if(L.size()-1>R.size()){
                R.push(L.top());
                L.pop();
            }
        }
    }
public:
    vector<double> medianSlidingWindow(vector<int>& nums, int k) {
        auto get_mid=[&]()->double{
            if(k&1)return L.top();
            return ((double)L.top()+R.top())/2.0;
        };
        int n=nums.size();
        for(int i=0;i<k-1;++i){
            add(nums[i]);
        }
        for(int i=k-1;i<n;++i){
            add(nums[i]);
            if(i!=k-1)remove(nums[i-k]);
            ans.push_back(get_mid());
        }
        return ans;
    }
};
```
