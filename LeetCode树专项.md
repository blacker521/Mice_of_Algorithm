---
typora-root-url: img/
---

## LeetCode 98

![](98.png)

```c++
/*
自底向上
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) :
            val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if (!root) return true;
        int maxv, minv;
        return dfs(root, maxv, minv);
    }

    bool dfs(TreeNode* root, int &maxv, int &minv)
    {
        maxv = minv = root->val;
        if (root->left)
        {
            int nowMaxv, nowMinv;
            if (!dfs(root->left, nowMaxv, nowMinv))
                return false;
            if (nowMaxv >= root->val)
                return false;
            maxv = max(maxv, nowMaxv);
            minv = min(minv, nowMinv);
        }
        if (root->right)
        {
            int nowMaxv, nowMinv;
            if (!dfs(root->right, nowMaxv, nowMinv))
                return false;
            if (nowMinv <= root->val)
                return false;
            maxv = max(maxv, nowMaxv);
            minv = min(minv, nowMinv);
        }
        return true;
    }
};
****************************************************************************************
/*
自顶向下
左右子树满足二叉搜索树的定义，一直缩小区间，不出现矛盾就正确
*/
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return dfs(root,INT_MIN,INT_MAX);
    }
    bool dfs(TreeNode *root,long long minv,long long maxv)
    {
        if(!root) return true;
        if(root->val < minv || root->val > maxv) return false;

        return dfs(root->left,minv,root->val - 1ll) && dfs(root->right,root->val + 1ll,maxv);//左子树的区间范围[minv,root->val - 1],右子树区间范围[root->val + 1,maxv]
    }
};
```

## LeetCode 94. 二叉树的中序遍历

![](94.png)

![](94_1.png)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<pair<TreeNode*, int>>sta;
        sta.push(make_pair(root, 0));
        while (!sta.empty())
        {
            if (sta.top().first == NULL)
            {
                sta.pop();
                continue;
            }
            int t = sta.top().second;
            if (t == 0)
            {
                sta.top().second = 1;
                sta.push(make_pair(sta.top().first->left, 0));
            }
            else if (t == 1)
            {
                res.push_back(sta.top().first->val);
                sta.top().second = 2;
                sta.push(make_pair(sta.top().first->right, 0));
            }
            else sta.pop();
        }
        return res;
    }
};
********************************************************************************
/*
用栈
1.将整棵树的最左边一条链压入栈中
2.每次取出栈顶元素，如果它有右子树，则将右子树压入栈中
 /**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stk;

        auto p = root;
        while(p || stk.size())
        {
            //把整个左边的一条链压入栈中
            while(p)
            {
                stk.push(p);
                p = p->left;
            }

            p = stk.top();
            stk.pop();

            res.push_back(p->val);
            p = p->right;
        }

        return res;
    }
};
```

## LeetCode 101.对称二叉树

![](101.png)

![](101_1.png)

```c++
/*
1.两个根节点的值要相等
2.左边的左子树和右边的右子树对称
3.左边的右子树和右边的左子树对称
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        return !root || dfs(root->left, root->right);
    }

    bool dfs(TreeNode*p, TreeNode*q)
    {
        if (!p || !q) return !p && !q;//同时空才对称
        return p->val == q->val && dfs(p->left, q->right) && dfs(p->right, q->left);
    }
};
```

![](101_2.png)

```c++
/*
按照左中右遍历左子树
按照右中左遍历右子树
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(!root) return true;
        stack<TreeNode*> left,right;
        auto l = root->left;
        auto r = root->right;
        while(l || r || left.size() || right.size())
        {
            while(l && r)
            {
                left.push(l);
                right.push(r);
                l = l->left;
                r = r->right;
            }

            if(l || r) return false;
            l = left.top(),left.pop();
            r = right.top(),right.pop();

            if(l->val != r->val) return false;
            l = l->right,r = r->left;
        }
        return true;
    }
};
```
## LeetCode 105. 从前序与中序遍历序列构造二叉树 !!!

![](105.png)

![](105_1.png)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:

    unordered_map<int,int> pos;

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();
        for (int i = 0; i < n; i ++ )
            pos[inorder[i]] = i;
        return dfs(preorder, inorder, 0, n - 1, 0, n - 1);//递归处理
    }

    TreeNode* dfs(vector<int>&pre, vector<int>&in, int pl, int pr, int il, int ir)
    {
        if (pl > pr) return NULL;
        int k = pos[pre[pl]] - il;
        TreeNode* root = new TreeNode(pre[pl]);
        root->left = dfs(pre, in, pl + 1, pl + k, il, il + k - 1);
        root->right = dfs(pre, in, pl + k + 1, pr, il + k + 1, ir);
        return root;
    }
};
```

## LeetCode 102. 二叉树的层次遍历

![](102.png)

![](102_1.png)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> get_val(vector<TreeNode*> level)
    {
        vector<int> res;
        for (auto &u : level)
            res.push_back(u->val);
        return res;
    }

    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>>res;
        if (!root) return res;
        vector<TreeNode*>level;
        level.push_back(root);
        res.push_back(get_val(level));
        while (true)
        {
            vector<TreeNode*> newLevel;
            for (auto &u : level)
            {
                if (u->left) newLevel.push_back(u->left);
                if (u->right) newLevel.push_back(u->right);
            }
            if (newLevel.size())
            {
                res.push_back(get_val(newLevel));
                level = newLevel;
            }
            else break;
        }
        return res;
    }
};
*****************************************************************************
 /**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;

        if(!root) return res;

        queue<TreeNode*> q;
        q.push(root);

        while(q.size())
        {
            int len = q.size();//当前层的节点个数
            vector<int> level;
            
            for(int i = 0;i < len;i ++)//扩展当前层
            {
                auto t = q.front();
                q.pop();
                level.push_back(t->val);
                if(t->left) q.push(t->left);
                if(t->right) q.push(t->right);
            }

            res.push_back(level);
        }
        return res;
    }
};
```
## LeetCode 236.二叉树的最近公共祖先

![](236.png)

```c++
/*
返回值的情况
1.如果以root为根的子树子树中包含p和q，则返回他们的最近公共祖先
2.如果只包含p，则返回p
3.如果只包含q，则返回q
4.如果都不包含，则返回NULL

若左边为空时
1.若右边都不包含，则right = NULL 最终需要返回NULL；
2.若右边只包含p或q，则right = p或q，最终需要返回p或q；
3.若右边同时包含p和q，则right是最近公共祖先，最终返回最近公共祖先

右边也同理

左右都不空(左右各一个)那就是根节点
*/
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(!root || root == p || root == q) return root;

        auto left = lowestCommonAncestor(root->left,p,q);
        auto right = lowestCommonAncestor(root->right,p,q);

        if(!left) return right;
        if(!right) return left;
        return root;
    }
};
```

## LeetCode 543. 二叉树的直径

![](543.png)

![](543_1.png)

```c++
/*枚举所有最高点
最高的点左右也是最高点

递归求左右子树最大深度的长度之和
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int dfs(TreeNode *r, int &ans) {
        if (r == NULL)
            return -1;
        int d1 = dfs(r -> left, ans);
        int d2 = dfs(r -> right, ans);
        ans = max(ans, d1 + d2 + 2);
        return max(d1, d2) + 1;
    }
    int diameterOfBinaryTree(TreeNode* root) {
        int ans = 0;
        dfs(root, ans);
        return ans;
    }
};
*************************************************************************************
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int ans = 0;
    int diameterOfBinaryTree(TreeNode* root) {
        dfs(root);
        
        return ans;
    }
    int dfs(TreeNode* root)
    {
        if(!root) return 0;
        auto left = dfs(root->left);
        auto right = dfs(root->right);

        ans = max(ans,left + right);
        return max(left + 1,right + 1);
    }
};
```

## LeetCode 124.  二叉树中的最大路径和

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int ans = INT_MIN;
    int maxPathSum(TreeNode* root) {
        dfs(root);
        return ans;
    }

    int dfs(TreeNode *root)
    {
        if(!root) return 0;

        auto left = dfs(root->left);
        auto right = dfs(root->right);
        ans = max(ans,left + root->val + right);
        return max(0,root->val + max(left,right));
    }
};
```

