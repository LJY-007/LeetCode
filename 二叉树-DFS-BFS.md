## Table of Contents
|序号|题目|难度|
|:--:|:-|:-:|
|94|[Binary Tree Inorder Traversal \| 二叉树的中序遍历](#94-Binary-Tree-Inorder-Traversal--二叉树的中序遍历)|Medium|
|102|[Binary Tree Level Order Traversal \| 二叉树的层序遍历](#102-Binary-Tree-Level-Order-Traversal--二叉树的层序遍历)|Medium|
|104|[Maximum Depth of Binary Tree \| 二叉树的最大深度](#104-Maximum-Depth-of-Binary-Tree--二叉树的最大深度)|Easy|
|144|[Binary Tree Preorder Traversal \| 二叉树的前序遍历](#144-Binary-Tree-Preorder-Traversal--二叉树的前序遍历)|Medium|
|145|[Binary Tree Postorder Traversal \| 二叉树的后序遍历](#145-Binary-Tree-Postorder-Traversal--二叉树的后序遍历)|Hard|


### 94. Binary Tree Inorder Traversal | 二叉树的中序遍历
🥈给定一个二叉树，返回它的中序遍历。
```
输入: [1,null,2,3]  输出: [1,3,2]
   1
    \
     2
    /
   3 
```
---

标签: `二叉树` `递归`<br>
时间复杂度:`O(N)` 空间复杂度:`O(H)`
```c++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        if (!root) return res;
        //🪁二叉树中序遍历的递归过程
        inorderTraversal(root->left);
        res.push_back(root->val);
        inorderTraversal(root->right);  
        return res;
    }

private:
    vector<int> res;
};
```

标签: `二叉树` `栈` `迭代`<br>
时间复杂度:`O(N)` 空间复杂度:`O(N)`
```c++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        if (!root) return res;
        //🪁不断往栈内压入左结点,若遇到空结点则弹出栈顶结点,并从右结点开始继续此操作
        while (!nodes.empty() || root) {
            if (root) {
                nodes.push(root);
                root = root->left;
            } else {
                root = nodes.top()->right;
                res.push_back(nodes.top()->val);
                nodes.pop();
            }
        }
        return res;
    }

private:
    vector<int> res;
    stack<TreeNode*> nodes; //利用栈结构实现迭代遍历
};
```

### 102. Binary Tree Level Order Traversal | 二叉树的层序遍历
🥈给你一个二叉树，请你返回其按层序遍历得到的节点值。（即逐层地，从左到右访问所有节点）。
```
给定二叉树: [3,9,20,null,null,15,7] 
    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果:
[
  [3],
  [9,20],
  [15,7]
]
```

---

标签: `二叉树` `BFS`<br>
时间复杂度:`O(N)` 空间复杂度:`O(N)`
```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        if (!root) return res;
        //🪁利用队列FIFO的特点对每一层进行迭代输出
        queue<TreeNode*> nodes;
        nodes.push(root);
        while (!nodes.empty()) {
            int len = nodes.size();
            vector<int> temp;
            while (len--) {
                temp.push_back(nodes.front()->val);
                if (nodes.front()->left) nodes.push(nodes.front()->left);
                if (nodes.front()->right) nodes.push(nodes.front()->right);
                nodes.pop();
            }
            res.push_back(temp);
        }
        return res;
    }
};
```

### 104. Maximum Depth of Binary Tree | 二叉树的最大深度
🥉给定一个二叉树，找出其最大深度。二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
```
给定二叉树: [3,9,20,null,null,15,7] 返回: 3 
    3
   / \
  9  20
    /  \
   15   7
```
---

标签: `二叉树` `DFS`<br>
时间复杂度:`O(N)` 空间复杂度:`O(N)`
```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (!root) return 0;
        //🪁树的深度等于其左子树深度与右子树深度的最大值+1
        return max(maxDepth(root->left), maxDepth(root->right)) + 1;
    }
};
```

标签: `二叉树` `BFS`<br>
时间复杂度:`O(N)` 空间复杂度:`O(N)`
```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (!root) return 0;
        //🪁使用BFS进行层序遍历时记录最终的层数即为最大深度
        int res = 0;
        queue<TreeNode*> nodes;
        nodes.push(root);
        while (!nodes.empty()) {
            res++;
            int len = nodes.size();
            while (len--) {
                if (nodes.front()->left) nodes.push(nodes.front()->left);
                if (nodes.front()->right) nodes.push(nodes.front()->right);
                nodes.pop();
            }
        }
        return res;
    }
};
```

### 144. Binary Tree Preorder Traversal | 二叉树的前序遍历
🥈给定一个二叉树，返回它的前序遍历。
```
输入: [1,null,2,3]  输出: [1,2,3]
   1
    \
     2
    /
   3 
```
---

标签: `二叉树` `递归`<br>
时间复杂度:`O(N)` 空间复杂度:`O(H)`
```c++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if (!root) return res;
        //🪁二叉树前序遍历的递归过程
        res.push_back(root->val);
        preorderTraversal(root->left);
        preorderTraversal(root->right);
        return res;
    }
    
private:
    vector<int> res;
};
```

标签: `二叉树` `栈` `迭代`<br>
时间复杂度:`O(N)` 空间复杂度:`O(N)`
```c++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if (!root) return res;
        nodes.push(root);
        //🪁每弹出一个栈内的结点,输出val并压入子结点(先右再左)
        while (!nodes.empty()) {
            TreeNode *temp = nodes.top();
            nodes.pop();
            res.push_back(temp->val);
            if (temp->right) nodes.push(temp->right);
            if (temp->left) nodes.push(temp->left);
        }
        return res;
    }

private:
    vector<int> res;
    stack<TreeNode*> nodes; //利用栈结构实现迭代遍历
};
```

### 145. Binary Tree Postorder Traversal | 二叉树的后序遍历
🏅️给定一个二叉树，返回它的后序遍历。
```
输入: [1,null,2,3]  输出: [3,2,1]
   1
    \
     2
    /
   3 
```
---

标签: `二叉树` `递归`<br>
时间复杂度:`O(N)` 空间复杂度:`O(H)`
```c++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        if (!root) return res;
        //🪁二叉树后序遍历的递归过程
        postorderTraversal(root->left);
        postorderTraversal(root->right);
        res.push_back(root->val);
        return res;
    }

private:
    vector<int> res;
};
```

标签: `二叉树` `栈` `迭代`<br>
时间复杂度:`O(N)` 空间复杂度:`O(N)`
```c++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        if (!root) return res;
        nodes.push(root);
        while (!nodes.empty()) {
            TreeNode *temp = nodes.top();
            res.push_back(temp->val);
            nodes.pop();
            if (temp->left) nodes.push(temp->left);
            if (temp->right) nodes.push(temp->right);
        }
        //🪁将类前序遍历(中左右->中右左)的结果翻转即为后序遍历(左右中)的结果
        reverse(res.begin(), res.end());
        return res;
    }

private:
    vector<int> res;
    stack<TreeNode*> nodes;
};
```
