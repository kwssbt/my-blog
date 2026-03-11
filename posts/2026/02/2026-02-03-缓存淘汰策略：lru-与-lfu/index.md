---
title: "缓存淘汰策略：LRU 与 LFU"
date: 2026-02-03
categories: 
  - "other"
tags: 
  - "others"
coverImage: "010023301e41c000_2026-01-09_10-00-31-392.png"
---

## [LRU 缓存](https://leetcode.cn/problems/lru-cache/description/)

_**LRU**_ 的全称是 **_Least Recently Used_**

计算机的缓存**_容量有限_**，如果缓存满了就要删除 **_最久没有访问的数据_**。

### 代码实现

双向链表

每次访问将数据更新至头部

新增数据时，若容量已满则删除尾部数据

```
class LRUCache {
    struct Node{
        int key;
        int val;
        Node *pre;
        Node *next;
        Node():key(0),val(0){}
        Node(int k,int v):key(k),val(v){}
    };
    int cap;
    Node *dumm;
    unordered_map<int,Node*>data;
    void remove(Node *node){
        node->next->pre=node->pre;
        node->pre->next=node->next;
    }
    void add_front(Node *node){
        node->next=dumm->next;
        node->pre=dumm;
        node->next->pre=node;
        node->pre->next=node;
    }
    void move_front(Node *node){
        remove(node);
        add_front(node);
    }
    void up(){
        if(data.size()==cap){
            Node *del=dumm->pre;
            remove(del);
            data.erase(del->key);
            delete del;
        }
    }
public:
    LRUCache(int capacity):cap(capacity),dumm(new Node()) {
        dumm->pre=dumm;
        dumm->next=dumm;
    }
    int get(int key) {
        if(data.count(key)){
            Node *tar=data[key];
            move_front(tar);
            return tar->val;
        }
        return -1;
    }
    void put(int key, int value) {
        if(data.count(key)){
            Node *tar=data[key];
            move_front(tar);
            tar->val=value;
        }
        else{
            up();
            Node *node=new Node(key,value);
            data[key]=node;
            add_front(node);
        }
    }
};
```

## [LFU 缓存](https://leetcode.cn/problems/lfu-cache/description/\)

**_LFU_** 的全称是 **_Least Frequently Used_**

缓存容量满了删除 **_访问次数最少的数据_**。

如果有多个**_访问次数最少的数据_**，删除其中**_最久没有访问的_**。

### 代码实现

分层链表，按照访问次数分层

每次访问：访问次数加一，从当前层链表中删除，移至下一层链表的头部

新增数据时：若容量已满，则删除最小层链表的尾部

```
class LFUCache {
    struct Node{
        int key;
        int val;
        int freq;
        Node *pre;
        Node *next;
        Node():key(0),val(0),freq(1){}
        Node(int k,int v):key(k),val(v),freq(1){}
    };
    int cap;
    int min_freq;
    unordered_map<int,Node*>key_to_node;
    unordered_map<int,Node*>freq_to_dumm;
    void remove(Node *node){
        node->next->pre=node->pre;
        node->pre->next=node->next;
    }
    Node *new_dumm(){
        Node *dumm=new Node();
        dumm->pre=dumm;
        dumm->next=dumm;
        return dumm;
    }
    void add_front(int freq,Node *node){
        Node *dumm;
        if(freq_to_dumm.count(freq)){
            dumm=freq_to_dumm[freq];
        }
        else{
            dumm=new_dumm();
            freq_to_dumm[freq]=dumm;
        }
        node->pre=dumm;
        node->next=dumm->next;
        node->next->pre=node;
        node->pre->next=node;
    }
    void up(){
        if(key_to_node.size()==cap){
            Node *dumm=freq_to_dumm[min_freq];
            Node *del=dumm->pre;
            key_to_node.erase(del->key);
            remove(del);
            delete del;
            if(dumm->next==dumm){
                freq_to_dumm.erase(min_freq);
                delete dumm;
            }
        }
    }
public:
    LFUCache(int capacity):cap(capacity),min_freq(0){
        
    }
    
    int get(int key) {
        if(key_to_node.count(key)){
            Node *tar=key_to_node[key];
            remove(tar);
            Node *dumm=freq_to_dumm[tar->freq];
            if(dumm->next==dumm){
                freq_to_dumm.erase(tar->freq);
                delete dumm;
                if(tar->freq==min_freq){
                    min_freq++;
                }
            }
            add_front(++tar->freq,tar);
            return tar->val;
        }
        return -1;
    }
    
    void put(int key, int value) {
        if(key_to_node.count(key)){
            Node *tar=key_to_node[key];
            remove(tar);
            Node *dumm=freq_to_dumm[tar->freq];
            if(dumm->next==dumm){
                freq_to_dumm.erase(tar->freq);
                delete dumm;
                if(tar->freq==min_freq){
                    min_freq++;
                }
            }
            tar->freq++;
            add_front(tar->freq,tar);
            tar->val=value;
        }
        else{
            up();
            Node *node=new Node(key,value);
            key_to_node[key]=node;
            add_front(1,node);
            min_freq=1;
        }
    }
};
```
