---
title: "二叉树非递归遍历"
date: 2026-02-18
categories: 
  - "base-algorithms"
  - "template"
tags: 
  - "基础算法"
  - "模板"
coverImage: "010023301e41c000_2026-02-04_12-17-18-634.png"
---

## 树结点(leetcode)

```
struct TreeNode{
    int val;
    TreeNode*left;
    TreeNode*right;
    TreeNode(int x):val(x),left(nullptr),right(nullptr){}
};
```

## 前序

```
void preorder(TreeNode*root){
    if(root==nullptr){
        return;
    }
    stack<TreeNode*>st;
    st.push(root);
    while(st.size()){
        TreeNode* cur=st.top();
        st.pop();
        /*
        前序位置
        */
       if(cur->right!=nullptr){
        st.push(cur->right);
       }
       if(cur->left!=nullptr){
        st.push(cur->left);
       }
    }
}
```

## 中序

```
void inorder(TreeNode*root){
    if(root==nullptr){
        return;
    }
    stack<TreeNode*>st;
    TreeNode*cur=root;
    while(st.size()||cur!=nullptr){
        if(cur!=nullptr){
            st.push(cur);
            cur=cur->left;
        }
        else{
            cur=st.top();
            st.pop();
            /*
            中序位置
            */
           cur=cur->right;
        }
    }
}
```

## 后序

```
void postorder(TreeNode*root){
    if(root==nullptr){
        return;
    }
    stack<TreeNode*>st;
    st.push(root);
    while(st.size()){
        TreeNode*cur=st.top();
        if(cur->left!=nullptr&&cur->left!=root&&cur->right!=root){
            st.push(cur->left);
        }
        else if(cur->right!=nullptr&&cur->right!=root){
            st.push(cur->right);
        }
        else{
            /*
            后序位置
            */
           root=st.top();
           st.pop();
        }
    }
}
```
