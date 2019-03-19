---
title: 剑指_CommonParentInTree
date: 2019-03-18
tags: 
- code 
- tree
---
#### 树中两个结点的最低公共祖先
---
题目：输入两个树结点，求它们的最低公共祖先。
<!--more-->
```
/*******************************************************************
Copyright(c) 2016, Harry He
All rights reserved.

Distributed under the BSD license.
(See accompanying file LICENSE.txt at
https://github.com/zhedahht/CodingInterviewChinese2/blob/master/LICENSE.txt)
*******************************************************************/


#include "..\Utilities\Tree.h"
#include <cstdio>
#include <list>

using namespace std;

bool GetNodePath(const TreeNode* pRoot, const TreeNode* pNode, list<const TreeNode*>& path)
{
    if (pRoot == pNode)
        return true;

    path.push_back(pRoot);

    bool found = false;

    vector<TreeNode*>::const_iterator i = pRoot->m_vChildren.begin();
    while (!found && i < pRoot->m_vChildren.end()) {
        found = GetNodePath(*i, pNode, path);
        ++i;
    }

    if (!found)
        path.pop_back();

    return found;
}

const TreeNode* GetLastCommonNode(
    const list<const TreeNode*>& path1,
    const list<const TreeNode*>& path2)
{
    list<const TreeNode*>::const_iterator iterator1 = path1.begin();
    list<const TreeNode*>::const_iterator iterator2 = path2.begin();

    const TreeNode* pLast = nullptr;

    while (iterator1 != path1.end() && iterator2 != path2.end()) {
        if (*iterator1 == *iterator2)
            pLast = *iterator1;

        iterator1++;
        iterator2++;
    }

    return pLast;
}

const TreeNode* GetLastCommonParent(const TreeNode* pRoot, const TreeNode* pNode1, const TreeNode* pNode2)
{
    if (pRoot == nullptr || pNode1 == nullptr || pNode2 == nullptr)
        return nullptr;

    list<const TreeNode*> path1;
    GetNodePath(pRoot, pNode1, path1);

    list<const TreeNode*> path2;
    GetNodePath(pRoot, pNode2, path2);

    return GetLastCommonNode(path1, path2);
}
```