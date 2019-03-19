---
title: Traversal
date: 2019-03-17
tags: ds
---

### 二叉树遍历
---

#### 层序遍历

```
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if(!root) return {};
        vector<vector<int>> an;
        vector<TreeNode*> cur,next;
        cur.push_back(root);
        while(!cur.empty()){
            an.push_back({});
            for(TreeNode* i:cur){
                an.back().push_back(i->val);
                if(i->left)next.push_back(i->left);
                if(i->right)next.push_back(i->right);
            }
            cur.swap(next);
            next.clear();
        }
        return an;
    }
};
```
<!--more-->

用队列
```
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if (!root) return {};
        vector<vector<int>> result{1, vector<int>{}};
        queue<TreeNode*> nodes{};
        nodes.push(root);
        nodes.push(nullptr);
        int resultIndex = 0;
        while(!nodes.empty()) {
            auto next = nodes.front();
            nodes.pop();
            if (!next) {
                if (nodes.empty()) break;
                nodes.push(nullptr);
                ++resultIndex;
                result.push_back(vector<int>{});
            } else {
                result[resultIndex].push_back(next->val);
                if (next->left) nodes.push(next->left);
                if (next->right) nodes.push(next->right);
            }
        }
        return result;
    }
};
```
---
#### 前序遍历Preorder Traversal (DLR)
```
class Solution {
public:
    vector<int> preorder(TreeNode* root,vector<int> an) {
        if(root) {
            an.push_back(root->val);            
            if(root->left!=nullptr)
                an=preorder(root->left,an);
            if(root->right!=nullptr)
                an=preorder(root->right,an);
        }
        return an;
    }
    vector<int> preorderTraversal(TreeNode* root) {
        if(!root) return{};
        vector<int> an;
        return preorder(root,an);
    }
};
```

空间小一点
```
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if (!root) return {};
        vector<int> res;
        vector<TreeNode *> todo {root};
        while (!todo.empty()) {
            root = todo.back(), todo.pop_back();
            res.push_back(root->val);
            if (root->right) todo.push_back(root->right);
            if (root->left)  todo.push_back(root->left);
        }
        return res;
    }
};
```
---
#### 中序遍历Inorder Traversal(LDR)
```
class Solution {
public:
    vector<int> inorder(TreeNode* root,vector<int> an) {
        if(root) {
            if(root->left!=nullptr)
                an=inorder(root->left,an);
            an.push_back(root->val);
            if(root->right!=nullptr)
                an=inorder(root->right,an);
        }
        return an;
    }
    vector<int> inorderTraversal(TreeNode* root) {
        if(!root) return{};
        vector<int> an;
        return inorder(root,an);
    }
};
```

用栈
```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        if(!root)   return {};
        vector<int> res;
        TreeNode* temp = root;
        stack<TreeNode*> s;
        while(1){
            while(temp){
                s.push(temp);
                temp = temp->left;
            }
            if(s.empty())   break;
            temp = s.top();
            s.pop();
            res.push_back(temp->val);
            temp = temp->right;
        }
        return res;
    }
};
```

空间小一点
```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> out;
        TreeNode* curr=root;
        TreeNode *pre,*temp;
        while(curr!=NULL)
        {
            if(curr->left==NULL)
            {
                out.push_back(curr->val);
                curr=curr->right;
            }
            else
            {
                pre = curr->left;
                while(pre->right!=NULL)
                    pre = pre -> right;
                pre -> right = curr;
                temp = curr;
                curr = curr->left;
                temp->left=NULL;
            }
        }
        return out;
    } 
};
```
---
#### 后序遍历Postorder Traversal (LRD)
```
class Solution {
public:
    vector<int> postorder(TreeNode* root,vector<int> an) {
        if(root) {        
            if(root->left!=nullptr)
                an=postorder(root->left,an);
            if(root->right!=nullptr)
                an=postorder(root->right,an);
            an.push_back(root->val);  
        }
        return an;
    }
    vector<int> postorderTraversal(TreeNode* root) {
        if(!root) return{};
        vector<int> an;
        return postorder(root,an);
    }
};
```

用栈
```
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> nodes;
        vector<int> vals;
        if(!root) {
            return vals;
        }
        nodes.push(root);
        while(!nodes.empty()) {
            TreeNode* curr = nodes.top();
            nodes.pop();
            if(curr -> left) {
                nodes.push(curr);                
                nodes.push(curr -> left);
                curr -> left = NULL;
            } else if (curr -> right) {
                nodes.push(curr);
                nodes.push(curr -> right);
                curr -> right = NULL;
            } else {
                vals.push_back(curr -> val);
            }
        }
        return vals;
    }
};
```
