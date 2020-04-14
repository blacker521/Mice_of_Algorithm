---
typora-root-url: img
---

## 101. 对称二叉树

![](101.png)

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
 /*递归写法
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(!root) return true;
        return dfs(root->left,root->right);
    }
    bool dfs(TreeNode *p,TreeNode *q)
    {
        if(!p || !q) return !q && !p;
        return p->val == q->val && dfs(p->left,q->right) && dfs(p->right,q->left);
    }
};
/*********************************************************/
/*迭代写法*/
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
## 102. 二叉树的层序遍历

![](102.png)

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
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;

        if(!root) return res;

        queue<TreeNode*> q;
        q.push(root);

        while(q.size())
        {
            int len = q.size();
            vector<int> level;
            
            for(int i = 0;i < len;i ++)
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
## 103. 二叉树的锯齿形层次遍历

![](103.png)

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

    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>>res;
        if (!root) return res;
        vector<TreeNode*>level;
        level.push_back(root);
        res.push_back(get_val(level));
        bool zigzag = true;
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
                vector<int>temp = get_val(newLevel);
                if (zigzag)
                    reverse(temp.begin(), temp.end());
                res.push_back(temp);
                level = newLevel;
            }
            else break;
            zigzag = !zigzag;
        }
        return res;
    }
};
```
## 104. 二叉树的最大深度

![](104.png)

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
    int maxDepth(TreeNode* root) {
        return root ? max(maxDepth(root->left),maxDepth(root->right)) + 1 : 0;
    }
};
```
## 105. 从前序与中序遍历序列构造二叉树

![](105.png)

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
        return dfs(preorder, inorder, 0, n - 1, 0, n - 1);
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
## 106. 从中序与后序遍历序列构造二叉树

![](106.png)

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

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        int n = inorder.size();
        for (int i = 0; i < n; i ++ )
            pos[inorder[i]] = i;
        return dfs(postorder, inorder, 0, n - 1, 0, n - 1);
    }

    // pl, pr 表示当前子树后序遍历在数组中的位置
    // il, ir 表示当前子树中序遍历在数组中的位置
    TreeNode* dfs(vector<int>&post, vector<int>&in, int pl, int pr, int il, int ir)
    {
        if (pl > pr) return NULL;
        int k = pos[post[pr]] - il;
        TreeNode* root = new TreeNode(post[pr]);
        root->left = dfs(post, in, pl, pl + k - 1, il, il + k - 1);
        root->right = dfs(post, in, pl + k, pr - 1, il + k + 1, ir);
        return root;
    }
};
```
## 107. 二叉树的层次遍历 II

![](107.png)

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

    vector<vector<int>> levelOrderBottom(TreeNode* root) {
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
        reverse(res.begin(), res.end());
        return res;
    }
};
```
## 108. 将有序数组转换为二叉搜索树

![](108.png)

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
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return build(0, nums.size() - 1, nums);
    }

    TreeNode *build(int l, int r, vector<int>&nums)
    {
        if (l > r) return 0;
        int mid = (l + r) / 2;
        TreeNode *root = new TreeNode(nums[mid]);
        root->left = build(l, mid - 1, nums);
        root->right = build(mid + 1, r, nums);
        return root;
    }
};
```
## 109. 有序链表转换二叉搜索树

![](109.png)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
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
    TreeNode* sortedListToBST(ListNode* head) {
        if (!head) return 0;
        int l = 0;
        for (auto i = head; i; i = i->next) l ++ ;
        l /= 2;

        ListNode*p = head;
        for (int i = 0; i < l; i ++ ) p = p->next;
        TreeNode *root = new TreeNode(p->val);

        root->right = sortedListToBST(p->next);
        if (l)
        {
            p = head;
            for (int i = 0; i < l - 1; i ++ ) p = p->next;
            p->next = 0;
            root->left = sortedListToBST(head);
        }
        return root;
    }
};
```
## 110. 平衡二叉树

![](110.png)

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
    bool isBalanced(TreeNode* root) {
        int h;
        return dfs(root, h);
    }

    bool dfs(TreeNode*root, int &height)
    {
        if (!root)
        {
            height = 0;
            return true;
        }

        int left, right;
        if (!dfs(root->left, left)) return false;
        if (!dfs(root->right, right)) return false;

        height = max(left, right) + 1;

        return abs(left - right) <= 1;
    }
};
```
## 111. 二叉树的最小深度

![](111.png)

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
    int minDepth(TreeNode* root) {
        if (!root) return 0;
        int res = INT_MAX;
        if (root->left) res = min(res, minDepth(root->left) + 1);
        if (root->right) res = min(res, minDepth(root->right) + 1);
        if (res == INT_MAX) res = 1;
        return res;
    }
};
```
## 112. 路径总和

![](112.png)

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
    bool hasPathSum(TreeNode* root, int sum) {
        if (!root) return false;
        if (!root->left && !root->right) return root->val == sum;
        if (root->left && hasPathSum(root->left, sum - root->val)) return true;
        if (root->right && hasPathSum(root->right, sum - root->val)) return true;
        return false;
    }
};
```
## 113. 路径总和 II

![](113.png)

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
    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> pathSum(TreeNode* root, int sum)
    {
        if (!root) return ans;
        dfs(root, sum);
        return ans;
    }

    void dfs(TreeNode* root, int sum)
    { 
        path.push_back(root->val);
        sum -= root->val;
        if (root->left || root->right)
        {
            if (root->left) dfs(root->left, sum);
            if (root->right) dfs(root->right, sum);
        }
        else
        {
            if (!sum)
            {
                ans.push_back(path);
            }
        }
        path.pop_back();
    }
};
```
## 114. 二叉树展开为链表

![](114.png)

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
    void flatten(TreeNode* root) {
        TreeNode *now = root;
        while (now)
        {
            if (now->left)
            {
                TreeNode *p = now->left;
                while (p->right) p = p->right;
                p->right = now->right;
                now->right = now->left;
                now->left = 0;
            }
            now = now->right;
        }
    }
};
```
## 115. 不同的子序列

![](115.png)

```c++
class Solution {
public:
    int numDistinct(string s, string t) {
        int n = s.size(), m = t.size();
        vector<vector<long long>> f(n + 1, vector<long long>(m + 1));

        for (int i = 0; i <= n; i ++ ) f[i][0] = 1;

        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
            {
                f[i][j] = f[i - 1][j];
                if (s[i - 1] == t[j - 1])
                    f[i][j] += f[i - 1][j - 1];
            }

        return f[n][m];
    }
};
```
## 116. 填充每个节点的下一个右侧节点指针

![](116.png)

```c++
/**
 * Definition for binary tree with next pointer.
 * struct TreeLinkNode {
 *  int val;
 *  TreeLinkNode *left, *right, *next;
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}
 * };
 */
class Solution {
public:
    void connect(TreeLinkNode *root)
    {
        if (!root) return;
        TreeLinkNode *last = root;
        while (last->left) // 直到遍历到叶节点
        {
            for (TreeLinkNode *p = last; p; p = p->next)
            {
                p->left->next = p->right;
                if (p->next) p->right->next = p->next->left;
                else p->right->next = 0;
            }
            last = last->left;
        }
    }
};
```
## 117. 填充每个节点的下一个右侧节点指针 II

![](117.png)

```c++
/**
 * Definition for binary tree with next pointer.
 * struct TreeLinkNode {
 *  int val;
 *  TreeLinkNode *left, *right, *next;
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}
 * };
 */
class Solution {
public:
    void connect(TreeLinkNode *root) {
        while (root)
        {
            TreeLinkNode *dummy = new TreeLinkNode(0);
            TreeLinkNode *tail = dummy;
            while (root)
            {
                if (root->left)
                {
                    tail->next = root->left;
                    tail = tail->next;
                }
                if (root->right)
                {
                    tail->next = root->right;
                    tail = tail->next;
                }
                root = root->next;
            }
            tail->next = 0;
            root = dummy->next;
        }
    }
};
```
## 118. 杨辉三角

![](118.png)

```c++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> res;
        if (!numRows) return res;
        res.push_back(vector<int>(1, 1));
        for (int i = 1; i < numRows; i ++ )
        {
            vector<int> &last = res.back();
            vector<int> temp;
            temp.push_back(1);
            for (int i = 0; i + 1 < last.size(); i ++ )
                temp.push_back(last[i] + last[i + 1]);
            temp.push_back(1);
            res.push_back(temp);
        }
        return res;
    }
};
```
## 119. 杨辉三角 II

![](119.png)

```c++
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<int> last(rowIndex + 1), now(rowIndex + 1);
        last[0] = 1;
        for (int i = 0; i < rowIndex; i ++ )
        {
            now[0] = last[0];
            for (int j = 0; j + 1 <= i; j ++ )
                now[j + 1] = last[j] + last[j + 1];
            now[i + 1] = last[i];
            last = now;
        }
        return last;
    }
};
```
## 120. 三角形最小路径和

![](120.png)

```c++
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<vector<long long>> f(n,vector<long long>(n));
        f[0][0] = triangle[0][0];
        for(int i = 1;i < n;i ++)
            for(int j = 0;j <= i; j ++)
            {
                f[i & 1][j] = INT_MAX;
                if(j > 0) f[i & 1][j] = min(f[i & 1][j],f[i - 1 & 1][j - 1] + triangle[i][j]);//可以从左边更新
                if(j < i) f[i & 1][j] = min(f[i & 1][j],f[i - 1 & 1][j] + triangle[i][j]);//可以从右边更新
            }
        long long res= INT_MAX;
        for(int i = 0;i < n;i ++) res = min(res,f[n - 1 & 1][i]);//枚举最后一行的状态
        return res;
    }
};
```