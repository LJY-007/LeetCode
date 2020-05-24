## Table of Contents
|序号|题目|难度|
|:--:|:-|:-:|
|94|[Binary Tree Inorder Traversal \| 二叉树的中序遍历](#94-Binary-Tree-Inorder-Traversal--二叉树的中序遍历)|Medium|
|98|[Validate Binary Search Tree \| 验证二叉搜索树](#98-Validate-Binary-Search-Tree--验证二叉搜索树)|Medium|
|102|[Binary Tree Level Order Traversal \| 二叉树的层序遍历](#102-Binary-Tree-Level-Order-Traversal--二叉树的层序遍历)|Medium|
|104|[Maximum Depth of Binary Tree \| 二叉树的最大深度](#104-Maximum-Depth-of-Binary-Tree--二叉树的最大深度)|Easy|
|107|[Binary Tree Level Order Traversal II \| 二叉树的层次遍历 II](#107-Binary-Tree-Level-Order-Traversal-II--二叉树的层次遍历-II)|Easy|
|110|[Balanced Binary Tree \| 平衡二叉树](#110-Balanced-Binary-Tree--平衡二叉树)|Easy|
|144|[Binary Tree Preorder Traversal \| 二叉树的前序遍历](#144-Binary-Tree-Preorder-Traversal--二叉树的前序遍历)|Medium|
|145|[Binary Tree Postorder Traversal \| 二叉树的后序遍历](#145-Binary-Tree-Postorder-Traversal--二叉树的后序遍历)|Hard|
|297|[Serialize and Deserialize Binary Tree \| 二叉树的序列化与反序列化](#297-Serialize-and-Deserialize-Binary-Tree--二叉树的序列化与反序列化)|Hard|
|985|[Check Completeness of a Binary Tree \| 二叉树的完全性检验](#985-Check-Completeness-of-a-Binary-Tree--二叉树的完全性检验)|Medium|

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

### 98. Validate Binary Search Tree | 验证二叉搜索树
🥈给定一个二叉树，判断其是否是一个有效的二叉搜索树。假设一个二叉搜索树具有如下特征：
节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。
```
输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。 根节点的值为 5 ，但是其右子节点值为 4 。
```
---

标签: `二叉搜索树` `中序遍历`<br>
时间复杂度:`O(N)` 空间复杂度:`O(N)`
```c++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if (!root) return true;
        //🪁二叉搜索树的中序遍历为有序数组,即确保遍历过程中的当前值大于前一个值
        if (!isValidBST(root->left)) return false;
        if (temp.empty()) {
            temp.push_back(root->val);
        } else if (temp[0] < root->val) {
            temp[0] = root->val; 
        } else {
            return false;
        }
        return isValidBST(root->right);
    }

private:
    vector<int> temp; //用于存储遍历过程中结点的值
};
```

标签: `二叉搜索树` `栈` `中序遍历`<br>
时间复杂度:`O(N)` 空间复杂度:`O(N)`
```c++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if (!root) return true;
        //🪁利用栈结构迭代实现中序遍历,确保读取的数值不断递增即可
        while (root || !nodes.empty()) {
            if (root) {
                nodes.push(root);
                root = root->left;
            } else {
                if (temp.empty()) {
                    temp.push_back(nodes.top()->val);
                } else if (temp[0] < nodes.top()->val) {
                    temp[0] = nodes.top()->val; 
                } else {
                    return false;
                }
                root = nodes.top()->right;
                nodes.pop();
            }
        }
        return true;
    }

private:
    stack<TreeNode*> nodes;
    vector<int> temp;
};
```

标签: `二叉搜索树` `先序遍历`<br>
时间复杂度:`O(N)` 空间复杂度:`O(N)`
```c++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        //🪁对于int型变量,用long类型来初始化上下界
        return validBound(root, LONG_MIN, LONG_MAX);
    }

    //🪁先序遍历每个结点,确保每个结点的值在上下界之内
    bool validBound(TreeNode* root, long lower, long upper) {
        if (!root) return true;
        if (root->val <= lower || root->val >= upper) return false;
        return validBound(root->left, lower, root->val) && validBound(root->right, root->val, upper);
    }
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

### 105. Construct Binary Tree from Preorder and Inorder Traversal | 从前序与中序遍历序列构造二叉树
🥈根据一棵树的前序遍历与中序遍历构造二叉树。你可以假设树中没有重复的元素。
```
给出: 前序遍历 preorder = [3,9,20,15,7] 中序遍历 inorder = [9,3,15,20,7]
返回二叉树:

    3
   / \
  9  20
    /  \
   15   7
```
---

标签: `二叉树` `中序遍历` `后序遍历` `哈希表` `递归`<br>
时间复杂度:`O(N)` 空间复杂度:`O(N)`
```c++
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        //🪁用哈希表记录中序遍历的值与下标,便于在递归中快速查找到根的位置
        for (int i = 0; i != inorder.size(); ++i) indexs[inorder[i]] = i;
        return recursiveBuild(preorder, 0, 0, inorder.size() - 1);
    }

    TreeNode* recursiveBuild(const vector<int>& preorder, int pre_root, int in_left, int in_right) {
        if (in_left > in_right) return NULL;
        //🪁建立根结点,并确定左右子树在前序遍历与中序遍历中的分割,之后连接根结点与左右子树
        TreeNode *root = new TreeNode(preorder[pre_root]);
        int in_root = indexs[preorder[pre_root]];
        root->left = recursiveBuild(preorder, pre_root + 1, in_left, in_root - 1);
        root->right = recursiveBuild(preorder, pre_root + in_root - in_left + 1, in_root + 1, in_right);
        return root;
    }

private:
    unordered_map<int, int> indexs;
};
```

### 107. Binary Tree Level Order Traversal II | 二叉树的层次遍历 II
🥉给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）
```
给定二叉树: [3,9,20,null,null,15,7] 
    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果:
[
  
  [15,7],
  [9,20],
  [3]
]
```
---

标签: `二叉树` `BFS`<br>
时间复杂度:`O(N)` 空间复杂度:`O(N)`
```c++
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> res;
        if (!root) return res;
        //🪁使用队列对每一层进行迭代输出,并最终返回反向数组
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
        reverse(res.begin(), res.end());
        return res;
    }
};
```

### 110. Balanced Binary Tree | 平衡二叉树
🥉给定一个二叉树，判断它是否是高度平衡的二叉树。一棵高度平衡二叉树定义为：一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。
```c++
给定二叉树: [1,2,2,3,3,null,null,4,4] 返回: false

       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
```
---

标签: `二叉树` `自顶向下` `先序遍历`<br>
时间复杂度:`O(NlogN)` 空间复杂度:`O(N)`
```c++
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if (!root) return true;
        //🪁利用二叉树的深度计算,判断二叉树的每个结点的左子树和右子树是否平衡(高度差≤1)
        int diff = maxDepth(root->left) - maxDepth(root->right);
        if (diff > 1 || diff < -1) return false;
        return isBalanced(root->left) && isBalanced(root->right);
    }

    //🪁DFS递归求二叉树的深度
    int maxDepth(TreeNode* root) {
        if (!root) return 0;
        return max(maxDepth(root->left), maxDepth(root->right)) + 1;
    }
};
```

标签: `二叉树` `自底向上` `后序遍历`<br>
时间复杂度:`O(N)` 空间复杂度:`O(N)`
```c++
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if (!root) return true;
        return heightBalance(root) == -1 ? false : true;
    }
    
    int heightBalance(TreeNode* root) {
        if (!root) return 0;
        //🪁如果左子树或右子树不平衡则直接返回-1(当前结点为根的树也不平衡)
        int left = heightBalance(root->left);
        if (left == -1) return -1;
        int right = heightBalance(root->right);
        if (right == -1) return -1;
        //🪁如果当前结点的左右子树的高度差≤1则返回当前结点的高度,否则返回-1(不平衡)
        return abs(left - right) < 2 ? max(left, right) + 1 : -1;
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

### 297. Serialize and Deserialize Binary Tree | 二叉树的序列化与反序列化
🏅️序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。
请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。
```
你可以将以下二叉树:

    1
   / \
  2   3
     / \
    4   5

序列化为: "[1,2,3,null,null,4,5]"
```
---

标签: `二叉树` `BSF` `字符串`<br>
时间复杂度:`O(N)` 空间复杂度:`O(N)`
```c++
class Codec {
public:
    string serialize(TreeNode* root) {
        //🪁BFS层序遍历二叉树,以字符串的形式存储结点的值(空节点的值为"null",以','分割)
        string res;
        queue<TreeNode*> nodes;
        nodes.push(root);
        while (!nodes.empty()) {
            if (nodes.front()) {
                res += to_string(nodes.front()->val) + ',';
                nodes.push(nodes.front()->left);
                nodes.push(nodes.front()->right);
            } else {
                res += "null,";
            }
            nodes.pop();
        }
        return res;
    }
    
    TreeNode* deserialize(string data) {
        //🪁将字符串以特定符号为界分割,并以此建立对应的结点(用指针数组存储)
        vector<TreeNode*> nodes;
        string temp = "";
        for (auto c : data) {
            if (c == ',') {
                if (temp == "null") {
                    nodes.push_back(NULL);
                } else {
                    nodes.push_back(new TreeNode(stoi(temp)));
                }
                temp = "";
            } else {
                temp += c;
            }
        }
        
        //🪁使用i,j两个下标分别表示当前结点与其左右子结点(当i指向空结点时,j不变)的位置
        for (int i = 0, j = 1; j != nodes.size(); ++i) {
            if (nodes[i]) {
                nodes[i]->left = nodes[j++];
                nodes[i]->right = nodes[j++];
            } 
        }
        return nodes[0];
    }
};
```

标签: `二叉树` `BSF` `IO流`<br>
时间复杂度:`O(N)` 空间复杂度:`O(N)`
```c++
class Codec {
public:
    string serialize(TreeNode* root) {
        //🪁BFS层序遍历二叉树,以输出流的形式存储结点的值(空节点的值为"null",以' '分割)
        ostringstream output;
        queue<TreeNode*> nodes;
        nodes.push(root);
        while (!nodes.empty()) {
            if (nodes.front()) {
                output << nodes.front()->val << ' '; //巧妙运用输出流避免了类型转换
                nodes.push(nodes.front()->left);
                nodes.push(nodes.front()->right);
            } else {
                output << "null ";
            }
            nodes.pop();
        }
        return output.str();
    }

    TreeNode* deserialize(string data) {
        //🪁每次从输入流读取字符串(以' '分割),并以此建立对应的结点(用指针数组存储)
        istringstream input(data);
        string val;
        vector<TreeNode*> nodes;
        while (input >> val) {
            if (val == "null") {
                nodes.push_back(NULL);
            } else {
                nodes.push_back(new TreeNode(stoi(val)));
            }
        }
        
        //🪁使用i,j两个下标分别表示当前结点与其左右子结点(当i指向空结点时,j不变)的位置
        for (int i = 0, j = 1; j != nodes.size(); ++i) {
            if (nodes[i]) {
                nodes[i]->left = nodes[j++];
                nodes[i]->right = nodes[j++];
            } 
        }
        return nodes[0];
    }
};
```

### 985. Check Completeness of a Binary Tree | 二叉树的完全性检验
🥈给定一个二叉树，确定它是否是一个完全二叉树。
```
输入：[1,2,3,4,5,null,7] 输出：false
解释：值为 7 的结点没有尽可能靠向左侧。
```
---

标签: `完全二叉树` `BFS`<br>
时间复杂度:`O(N)` 空间复杂度:`O(N)`
```c++
class Solution {
public:
    bool isCompleteTree(TreeNode* root) {
        if (!root) return true;
        //🪁BFS层序遍历二叉树,当遇到空结点时其后续所有结点必须为空
        queue<TreeNode*> nodes;
        nodes.push(root);
        bool has_null = false;
        while (!nodes.empty()) {
            if (has_null && nodes.front()) return false; 
            if (nodes.front()) {
                nodes.push(nodes.front()->left);
                nodes.push(nodes.front()->right);
            } else {
                has_null = true;
            }
            nodes.pop();
        }
        return true;
    }
};
```
