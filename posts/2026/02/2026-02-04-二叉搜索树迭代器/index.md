---
title: "二叉搜索树迭代器"
date: 2026-02-04
categories: 
  - "other"
tags: 
  - "others"
coverImage: "010023301e41c000_2026-02-04_12-11-13-099-e1770218805992.png"
---

[leetcode 173](https://leetcode.cn/problems/binary-search-tree-iterator/description/)

## API：

- `BSTIterator(TreeNode* root)`初始化

- `int next()` 将指针向右移动，然后返回指针处的数字

- `bool hasNext()`如果向指针右侧遍历存在数字，则返回 `true` 否则返回 `false`

- `int peak()` 返回当前指针处的数字

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class BSTIterator {
    stack<TreeNode*>st;//用栈模拟递归
 
    void pushleft(TreeNode*p){//一路向左，全部进栈，最后栈顶是最小元素
        while(p){
            st.push(p);
            p=p->left;
        }
    }
public:
    BSTIterator(TreeNode* root) {
        pushleft(root);
    }
    
    int next() {
        auto p=st.top();
        st.pop();
        pushleft(p->right);//出栈后右孩子进栈
        return p->val;
    }
    
    bool hasNext() {
        return st.size();
    }
 
    int peak() {
        return st.top()->val;
    }
 
};
```

## 简单应用

给你 `root1` 和 `root2` 这两棵二叉搜索树  
请你返回一个列表，其中包含 **两棵树** 中的所有整数并按 **升序** 排序

[leetcode 1305](https://leetcode.cn/problems/all-elements-in-two-binary-search-trees/description/)

- 利用迭代器将树转化成链表

- 合并有序链表

```
class Solution {
    class BSTIterator {

    };
 
public:
    vector<int> getAllElements(TreeNode* root1, TreeNode* root2) {
        BSTIterator s1(root1);
        BSTIterator s2(root2);
        
        vector<int>ans;
 
        while(s1.hasNext()&&s2.hasNext()){
            if(s1.peak()<s2.peak()){
                ans.push_back(s1.next());
            }
            else ans.push_back(s2.next());
        }
 
        while(s2.hasNext())ans.push_back(s2.next());
 
        while(s1.hasNext())ans.push_back(s1.next());
        
        return ans;
    }
};
```
