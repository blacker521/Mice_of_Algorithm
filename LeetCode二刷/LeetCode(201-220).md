---
typora-root-url: img
---

## 81. 搜索旋转排序数组 II

![](81.png)

```c++
class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        int ans = 0;
        for (int i = 30; i >= 0; i--) {
            if ((m >> i & 1) ^ (n >> i & 1))
                break;
            ans |= (m >> i & 1) << i;
        }
        return ans;
    }
};
```
## 82. 删除排序链表中的重复元素 II

![](82.png)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode* p = dummy;
        while (p->next)
        {
            ListNode* q = p->next;
            while (q && q->val == p->next->val)
                q = q->next;
            if (p->next->next == q) p = p->next;
            else p->next = q;
        }
        return dummy->next;
    }
};
```
## 83. 删除排序链表中的重复元素

![](83.png)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(!head) return NULL;
        ListNode *first,*second;
        first = second = head;
        while(first)
        {
            if(first->val != second->val)
            {
                second->next = first;
                second = first;
            }
            first = first->next;
        }
        second->next = NULL;
        return head;
    }
};
```
## 84. 柱状图中最大的矩形

![](84.png)

```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        vector<int> left(n),rihgt(n);//左右边界

        stack<int> stk;
        //左边
        for(int i = 0;i < n;i ++)
        {
            while(stk.size() && heights[stk.top()] >= heights[i]) stk.pop();
            if(stk.empty()) left[i] = -1;
            else left[i] = stk.top();
            stk.push(i);
        }
        while(stk.size()) stk.pop();
        //右边
        for(int i = n - 1;i >= 0;i --)
        {
            while(stk.size() && heights[stk.top()] >= heights[i]) stk.pop();
            if(stk.empty()) rihgt[i] = n;
            else rihgt[i] = stk.top();
            stk.push(i);
        }

        int res = 0;
        //枚举每个边界，取最大值
        for(int i = 0;i < n;i ++) res = max(res,heights[i] * (rihgt[i] - left[i] - 1));
        return res;
    }
};
```
## 85. 最大矩形

![](85.png)

```c++
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        int n = matrix.size(), m, ans = 0;
        if (n == 0)
            return 0;
        m = matrix[0].size();
        vector<int> heights(m + 1, 0);
        heights[m] = -1;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++)
                if (matrix[i][j] == '0')
                    heights[j] = 0;
                else
                    heights[j]++;

            stack<int> st;
            for (int j = 0; j <= m; j++) {
                while (!st.empty() && heights[j] < heights[st.top()]) {
                    int cur = st.top();
                    st.pop();
                    if (st.empty())
                        ans = max(ans, heights[cur] * j);
                    else
                        ans = max(ans, heights[cur] * (j - st.top() - 1));
                }
                st.push(j);
            }
        }
        return ans;
    }
};
```
## 86. 分隔链表

![](86.png)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        ListNode *before = new ListNode(0);
        ListNode *after = new ListNode(0);
        ListNode *pb = before, *pa = after;

        for (ListNode *p = head; p; p = p->next)
            if (p->val < x)
            {
                pb->next = p;
                pb = p;
            }
            else
            {
                pa->next = p;
                pa = p;
            }

        pb->next = after->next;
        pa->next = 0;

        return before->next;
    }
};
```
## 87. 扰乱字符串

![](87.png)

![](87-1.png)

```c++
class Solution {
public:
    bool isScramble(string s1, string s2) {
        if (s1 == s2) return true;
        string ss1 = s1, ss2 = s2;
        sort(ss1.begin(), ss1.end()), sort(ss2.begin(), ss2.end());
        if (ss1 != ss2) return false;
        for (int i = 1; i < s1.size(); i ++ )
        {
            if (isScramble(s1.substr(0, i), s2.substr(0, i))
                    && isScramble(s1.substr(i), s2.substr(i)))
                return true;
            if (isScramble(s1.substr(0, i), s2.substr(s2.size() - i))
                    && isScramble(s1.substr(i), s2.substr(0, s2.size() - i)))
                return true;
        }
        return false;
    }
};
```
## 88. 合并两个有序数组

![](88.png)

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int p = m - 1,q = n - 1,cur = m + n - 1;
        while(p >= 0 && q >= 0)
        {
            if(nums1[p] >= nums2[q]) nums1[cur --] = nums1[p --];
            else nums1[cur --] = nums2[q --];
        }
        while(p >= 0) nums1[cur --] = nums1[p --];
        while(q >= 0) nums1[cur --] = nums2[q --];
    }
};
```
## 89. 格雷编码

![](89.png)

```c++
class Solution {
public:
    vector<int> grayCode(int n) {
        vector<int> res;
        res.push_back(0);
        int t = 1;
        while (n -- )
        {
            vector<int> newRes;
            for (int i = 0; i < res.size(); i ++ )
                newRes.push_back(res[i]);
            for (int i = res.size() - 1; i >= 0; i -- )
                newRes.push_back(t + res[i]);
            res = newRes;
            t *= 2;
        }
        return res;
    }
};
```
## 90. 子集 II

![](90.png)

```c++
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        dfs(nums,0);
        return ans;
    }

    void dfs(vector<int> &nums,int u)
    {
        if(u == nums.size())
        {
            ans.push_back(path);
            return;
        }

        int k = 0;
        while(u + k < nums.size() && nums[u + k] == nums[u]) k ++;

        for(int i = 0; i <= k;i ++)
        {
            dfs(nums,u + k);
            path.push_back(nums[u]);
        }

        for(int i = 0;i <= k;i ++)path.pop_back();

    }
};
```
## 91. 解码方法

![](91.png)

```c++
class Solution {
public:
    int numDecodings(string s) {
        int n = s.size();
        vector<int> f(n + 1);
        f[0] = 1;
        for(int i = 1;i <= n;i ++)
        {
            if(s[i - 1] != '0') f[i] += f[i - 1];
            if(i >= 2)
            {
                int t = (s[i - 2] - '0') * 10 + s[i - 1] - '0';
                if(t >= 10 && t <= 26) f[i] += f[i - 2];
            }
        }
        return f[n];
    }
};
```
## 92. 反转链表 II

![](92.png)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        if(m == n) return head;
        ListNode *dummy = new ListNode(-1);
        dummy->next = head;
        ListNode *b = dummy;
        for(int i = 0;i < m - 1;i ++) b = b->next;
        ListNode *a = b;
        b = b->next;
        ListNode *c = b->next;
        for(int i = 0;i < n - m;i ++)
        {
            ListNode *t = c->next;
            c->next = b;
            b = c,c = t;
        }
        ListNode *mp = a->next;
        ListNode *np = b;
        a->next = np,mp->next = c;
        return dummy->next;
    }
};
```
## 93. 复原IP地址

![](93.png)

```c++
class Solution {
public:
    vector<string> ans;
    vector<int> path;

    vector<string> restoreIpAddresses(string s) {
        dfs(0, 0, s);
        return ans;
    }

    // u表示枚举到的字符串下标，k表示当前截断的IP个数，s表示原字符串
    void dfs(int u, int k, string &s)
    {
        if (u == s.size())
        {
            if (k == 4)
            {
                string ip = to_string(path[0]);
                for (int i = 1; i < 4; i ++ )
                    ip += '.' + to_string(path[i]);
                ans.push_back(ip);
            }
            return;
        }
        if (k > 4) return;

        unsigned t = 0;
        for (int i = u; i < s.size(); i ++ )
        {
            t = t * 10 + s[i] - '0';
            if (t >= 0 && t < 256)
            {
                path.push_back(t);
                dfs(i + 1, k + 1, s);
                path.pop_back();
            }
            if (!t) break;
        }
    }
};
```
## 94. 二叉树的中序遍历

![](94.png)

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
        stack<TreeNode*> stk;

        auto p = root;
        while(p || stk.size())
        {
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
## 95. 不同的二叉搜索树 II

![](95.png)

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
    vector<TreeNode*> generateTrees(int n) {
        if (!n) return vector<TreeNode*>();
        return dfs(1, n);
    }

    vector<TreeNode*> dfs(int l, int r)
    {
        vector<TreeNode*>res;
        if (l > r)
        {
            res.push_back(NULL);
            return res;
        }
        for (int i = l; i <= r; i ++ )
        {
            vector<TreeNode*>left = dfs(l, i - 1)
                , right = dfs(i + 1, r);
            for (auto &lc : left)
                for (auto &rc : right)
                {
                    TreeNode *root = new TreeNode(i);
                    root->left = lc;
                    root->right = rc;
                    res.push_back(root);
                }
        }
        return res;
    }
};
```
## 96. 不同的二叉搜索树

![](96.png)

```c++
class Solution {
public:
    int numTrees(int n) {
        vector<int>f(n + 1);
        f[0] = 1;
        for (int i = 1; i <= n; i ++ )
        {
            f[i] = 0;
            for (int j = 1; j <= i; j ++ )
                f[i] += f[j - 1] * f[i - j];
        }
        return f[n];
    }
};
```
## 97. 交错字符串

![](97.png)

```c++
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int n = s1.size(), m = s2.size(), k = s3.size();
        if (k != n + m) return false;
        vector<vector<int>> f = 
            vector<vector<int>>(n + 1, vector<int>(m + 1));
        f[0][0] = 1;
        for (int i = 1; i <= n; i ++ )
            f[i][0] = f[i - 1][0] && s1[i - 1] == s3[i - 1];
        for (int i = 1; i <= m; i ++ )
            f[0][i] = f[0][i - 1] && s2[i - 1] == s3[i - 1];
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
            {
                f[i][j] = 0;
                if (s1[i - 1] == s3[i + j - 1])
                    f[i][j] |= f[i - 1][j];
                if (s2[j - 1] == s3[i + j - 1])
                    f[i][j] |= f[i][j - 1];
            }
        return f[n][m];
    }
};
```
## 98. 验证二叉搜索树

![](98.png)

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
    bool isValidBST(TreeNode* root) {
        return dfs(root,INT_MIN,INT_MAX);
    }
    bool dfs(TreeNode *root,long long minv,long long maxv)
    {
        if(!root) return true;
        if(root->val < minv || root->val > maxv) return false;

        return dfs(root->left,minv,root->val - 1ll) && dfs(root->right,root->val + 1ll,maxv);
    }
};
```
## 99. 恢复二叉搜索树

![](99.png)

```c++
/*
这道题目如果用递归做，递归的层数最坏是 O(n)O(n) 级别的，所以系统栈的空间复杂度是 O(n)O(n)，与题目要求的 O(1)O(1) 额外空间不符。
同理用栈模拟递归的迭代方式的空间复杂度也是 O(n)O(n)，不符合题目要求。

这道题目可以用Morris-traversal算法，该算法可以用额外 O(1)O(1) 的空间，以及 O(n)O(n) 的时间复杂度，中序遍历一棵二叉树。

Morris-traversal 算法流程：
下图给了一个具体示例：
```
![](99-1.jpg)
```c++
/*从根节点开始遍历，直至当前节点为空为止：

如果当前节点没有左儿子，则打印当前节点的值，然后进入右子树；
如果当前节点有左儿子，则找当前节点的前驱。
(1) 如果前驱节点的右儿子为空，说明左子树没遍历过，则进入左子树遍历，并将前驱节点的右儿子置成当前节点，方便回溯；
(2) 如果前驱节点的右儿子为当前节点，说明左子树已被遍历过，则将前驱节点的右儿子恢复为空，然后打印当前节点的值，然后进入右子树继续遍历；
中序遍历的结果就是二叉树搜索树所表示的有序数列。有序数列从小到大排序，但有两个数被交换了位置。共有两种情况：

交换的是相邻两个数，例如 1 3 2 4 5 6，则第一个逆序对，就是被交换的两个数，这里是3和2；
交换的是不相邻的数，例如 1 5 3 4 2 6，则第一个逆序对的第一个数，和第二个逆序对的第二个数，就是被交换的两个数，这里是5和2；
找到被交换的数后，我们将它们换回来即可。
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
    void recoverTree(TreeNode* root) {
        TreeNode *first = NULL, *second, *prep = NULL;
        while (root)
        {
            if (!root->left)
            {
                if (prep && prep->val > root->val)
                {
                    if (!first) first = prep, second = root;
                    else second = root;
                }
                prep = root;
                root = root->right;
            }
            else
            {
                TreeNode *p = root->left;
                while (p->right && p->right != root) p = p->right;
                if (!p->right)
                {
                    p->right = root;
                    root = root->left;
                }
                else
                {
                    p->right = NULL;
                    if (prep && prep->val > root->val)
                    {
                        if (!first) first = prep, second = root;
                        else second = root;
                    }
                    prep = root;
                    root = root->right;
                }
            }
        }
        swap(first->val, second->val);
    }
};
```
## 100. 相同的树

![](100.png)

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
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(!p || !q) return !p && !q;
        return p->val == q->val && isSameTree(p->left,q->left) && isSameTree(p->right,q->right);
    }
};
```