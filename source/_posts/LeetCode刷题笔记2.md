---
title: LeetCode刷题笔记2
date: 2022-09-11 16:35:56
tags:
---

## 226. 翻转二叉树

**方法一**：递归

其实就是交换一下左右节点，然后再递归的交换左节点，右节点我们可以总结出递归的两个条件如下：

终止条件：当前节点为 null 时返回
交换当前节点的左右节点，再递归的交换当前节点的左节点，递归的交换当前节点的右节点

```c++
TreeNode *invertTree(TreeNode *root) {
    if (root == nullptr) {
        return nullptr;
    }
    TreeNode *temp = root->left;
    root->left = root->right;
    root->right = temp;
    invertTree(root->left);
    invertTree(root->right);
    return root;
}
```

**方法二**：迭代

递归实现也就是深度优先遍历的方式，迭代使用广度优先遍历，我们需要先将根节点放入到队列中，然后不断的迭代队列中的元素。
对当前元素调换其左右子树的位置，然后：

判断其左子树是否为空，不为空就放入队列
判断其右子树是否为空，不为空就放入队列

这样其实也是实现了二叉树的**层序遍历**

<!-- more -->

## 234. 回文链表

**思路**

1. 复制链表值到数组列表中。
2. 使用双指针法判断是否为回文。

```c++
bool isPalindrome(ListNode *head) {
    vector<int> vals;
    while (head!= nullptr){
        vals.push_back(head->val);
        head=head->next;
    }
    //注意这里是i<j，当i=j时一定是回文（都能使i和j重合证明前面的字母都是回文，
    //最后剩中间一个字母一定是回文的），如果i>j那么就是偶数，此前的判断已经证明是回文了
    for (int i = 0,j=(int)vals.size()-1; i < j; ++i,--j) {
        if (vals[i]!=vals[j]){
            return false;
        }
    }
    return true;
}
```

## 236. 二叉树的最近公共祖先

**方法一**：递归

**思路**

若 root 是 p, q的 最近公共祖先 ，则只可能为以下情况之一：

1. p 和 q 在 root 的子树中，且分列 root 的 异侧（即分别在左、右子树中）；
2. p=root ，且 q 在 root 的左或右子树中；
3. q=root ，且 p 在 root 的左或右子树中；

定义fx 表示 x 节点的子树中是否包含 p 节点或 q 节点，如果包含为 true，否则为 false。那么符合条件的最近公共祖先 x 一定满足如下条件（下面这个式子也可对应上面说的三种情况）：

`(lson && rson) || ((root==p||root==q)&&(lson||rson))`

其中 lson 和 rson 分别代表 x 节点的左孩子和右孩子。初看可能会感觉条件判断有点复杂，我们来一条条看，(lson && rson) 说明左子树和右子树均包含 p 节点或 q 节点，如果左子树包含的是 p 节点，那么右子树只能包含 q 节点，反之亦然，因为 p 节点和 q 节点都是不同且唯一的节点，因此如果满足这个判断条件即可说明 x 就是我们要找的最近公共祖先。再来看第二条判断条件，这个判断条件即是考虑了 x 恰好是 p 节点或 q 节点且它的左子树或右子树有一个包含了另一个节点的情况，因此如果满足这个判断条件亦可说明 x 就是我们要找的最近公共祖先。

你可能会疑惑这样找出来的公共祖先深度是否是最大的。其实是最大的，因为**我们是自底向上从叶子节点开始更新的**，所以在所有满足条件的公共祖先中一定是深度最大的祖先先被访问到，且由于 fx 本身的定义很巧妙，在找到最近公共祖先 x 以后，按定义被设置为 true ，在向上回溯的过程中，其他结点一定不会满足最近公共祖先的条件了，因此其他公共祖先不会再被判断为符合条件。

dfs成立的条件比较容易想到，就不再赘述。

```c++
TreeNode *ans;
// dfs 表示以 root 节点为根结点的树中是否包含 p 节点或 q 节点，如果包含为 true，否则为 false
bool dfs(TreeNode *root, TreeNode *p, TreeNode *q) {
    if (root == nullptr) {
        return false;
    }
    bool lson = dfs(root->left, p, q);
    bool rson = dfs(root->right, p, q);
    //这是最近公共祖先的条件
    if ((lson && rson) || ((root==p||root==q)&&(lson||rson))){
        ans=root;
    }
    //下面是dfs成立的条件
    return lson||rson||root->val==p->val||root->val==q->val;
}
TreeNode *lowestCommonAncestor(TreeNode *root, TreeNode *p, TreeNode *q) {
    dfs(root,p,q);
    return ans;
}
```

**方法二**：存储父节点

**思路**

我们可以用哈希表存储所有节点的父节点，然后我们就可以利用节点的父节点信息从 p  结点开始不断往上跳，并记录已经访问过的节点，再从 q 节点开始不断往上跳，如果碰到已经访问过的节点，那么这个节点就是我们要找的最近公共祖先。

**算法**

1. 从根节点开始遍历整棵二叉树，用哈希表记录每个节点的父节点指针。
2. 从 p 节点开始不断往它的祖先移动，并用数据结构记录已经访问过的祖先节点。
3. 同样，我们再从 q 节点开始不断往它的祖先移动，如果有祖先已经被访问过，即意味着这是 p 和 q 的深度最深的公共祖先，即 LCA 节点。

```c++
unordered_map<int, TreeNode *> father;
unordered_map<int, bool> visited;
void dfs(TreeNode *root) {
    if (root->left != nullptr) {
        father[root->left->val] = root;
        dfs(root->left);
    }
    if (root->right != nullptr) {
        father[root->right->val] = root;
        dfs(root->right);
    }
}
TreeNode *lowestCommonAncestor(TreeNode *root, TreeNode *p, TreeNode *q) {
    //注意这里把根结点的父节点设为nullptr
    father[root->val] = nullptr;
    dfs(root);
    while (p != nullptr) {
        visited[p->val] = true;
        p = father[p->val];
    }
    while (q != nullptr) {
        if (visited[q->val]){
            return q;
        }
        q=father[q->val];
    }
    return nullptr;
}
```

