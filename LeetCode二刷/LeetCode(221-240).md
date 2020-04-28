---
typora-root-url: img
---

## 221. 最大正方形

![](221.png)

```c++
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        if(matrix.size()==0)
            return 0;
        if(matrix[0].size()==0)
            return 0;
        int m = matrix.size();
        int n = matrix[0].size();
        vector<vector<int> > dp(m, vector<int>(n, 0));
        int res = 0;
        for(int i = 0;i<m;i++){
            for(int j  = 0;j<n;j++){
                if(matrix[i][j]=='0')
                    dp[i][j] = 0;
                else{
                    dp[i][j] = 1;
                    if(i>=1&&j>=1)
                        dp[i][j]+=min(dp[i-1][j],min(dp[i-1][j-1], dp[i][j-1]));
                    if(dp[i][j]>res)
                        res=dp[i][j];
                }
            }
        }
        return res*res;
    }
};
```
## 222. 完全二叉树的节点个数

![](222.png)

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
    int countNodes(TreeNode* root) {
        if(!root)
            return 0;
        int l = 0;
        int r = 0;
        TreeNode* leftp = root;
        TreeNode* rightp = root;
        while(leftp){
            l++;
            leftp = leftp->left;
        }
        while(rightp){
            r++;
            rightp = rightp->right;
        }
        if(l==r)
            return (1<<l)-1;
        return countNodes(root->left) + countNodes(root->right) + 1;
    }
};
```
## 223. 矩形面积

![](223.png)

```c++
class Solution {
public:
    int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        long long X = min(C, G) + 0ll - max(A, E);
        long long Y = min(D, H) + 0ll - max(B, F);
        return (C - A) * (D - B) - max(0ll, X) * max(0ll, Y) + (G - E) * (H - F);
    }
};
```
## 224. 基本计算器

![](224.png)

```c++
class Solution {
public:

    void calc(stack<char> &op, stack<int> &num) {
        int y = num.top();
        num.pop();
        int x = num.top();
        num.pop();
        if (op.top() == '+') num.push(x + y);
        else num.push(x - y);
        op.pop();
    }

    int calculate(string s) {
        stack<char> op;
        stack<int> num;
        for (int i = 0; i < s.size(); i ++ ) {
            char c = s[i];
            if (c == ' ') continue;
            if (c == '+' || c == '-' || c == '(') op.push(c);
            else if (c == ')') {
                op.pop();
                if (op.size() && op.top() != '(') {
                    calc(op, num);
                }
            }
            else {
                int j = i;
                while (j < s.size() && isdigit(s[j])) j ++ ;
                num.push(atoi(s.substr(i, j - i).c_str()));
                i = j - 1;
                if (op.size() && op.top() != '(') {
                    calc(op, num);
                }
            }
        }
        return num.top();
    }
};
```
## 225. 用队列实现栈

![](225.png)

```c++
class MyStack {
public:
    /** Initialize your data structure here. */
    queue<int> q;
    MyStack() {

    }

    /** Push element x onto stack. */
    void push(int x) {
        q.push(x);
    }

    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        int sz = q.size();
        while (sz -- > 1) q.push(q.front()), q.pop();
        int x = q.front();
        q.pop();
        return x;
    }

    /** Get the top element. */
    int top() {
        int sz = q.size();
        while (sz -- > 1) q.push(q.front()), q.pop();
        int x = q.front();
        q.pop(), q.push(x);
        return x;
    }

    /** Returns whether the stack is empty. */
    bool empty() {
        return q.empty();
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * bool param_4 = obj.empty();
 */
```
## 226. 翻转二叉树

![](226.png)

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
    TreeNode* invertTree(TreeNode* root) {
        if (!root) return 0;
        swap(root->left, root->right);
        invertTree(root->left), invertTree(root->right);
        return root;
    }
};
```
## 227. 基本计算器 II

![](227.png)

```c++
class Solution {
public:
    int calculate(string s) {
        if(s.size()==0)
            return 0;
        int res = 0;
        stack<int>stk;
        int num = 0;
        char sign = '+';
        for(int i = 0;i<s.size();i++){
            if(s[i] >= '0' && s[i] <= '9'){
                num *= 10;
                num = num - '0' + s[i];
            }
            if((s[i]=='+'||s[i]=='-'||s[i]=='*'||s[i]=='/')||i==s.size()-1){
                if(sign=='+')
                    stk.push(num);
                if(sign=='-')
                    stk.push(-num);
                if(sign=='*'){
                    int topnum = stk.top();
                    stk.pop();
                    stk.push(num*topnum);
                }
                if(sign=='/'){
                    int topnum = stk.top();
                    stk.pop();
                    stk.push(topnum/num);
                }
                sign=s[i];
                num = 0;
            }
        }
        while(!stk.empty()){
            int topnum = stk.top();
            res += topnum;
            stk.pop();
        }
        return res;
    }
};
```
## 228. 汇总区间

![](228.png)

```c++
class Solution {
public:
    vector<string> summaryRanges(vector<int>& nums) {
        vector<string> res;
        int st, ed;
        for (int i = 0; i < nums.size(); i ++ )
        {
            int x = nums[i];
            if (!i) st = ed = x;
            else if (x == ed + 1) ed ++ ;
            else
            {
                res.push_back(to_string(st) + (st == ed ? "" : "->" + to_string(ed)));
                st = ed = x;
            }
        }
        if (nums.size()) res.push_back(to_string(st) + (st == ed ? "" : "->" + to_string(ed)));
        return res;
    }
};
```
## 229. 求众数 II

![](229.png)

```c++
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        int numsSize = nums.size();
        vector<int>ret;
        if(numsSize == 0) return ret;
        int count1 = 0;
        int count2 = 0;
        int candidate1 = 0;
        int candidate2 = 0;
        for(auto i : nums)
        {//第一轮扫描确定candidate
            if(candidate1 == i) count1++;
            else if (candidate2 == i) count2++;
            else if (count1 == 0)
            {
                candidate1 = i;
                count1 = 1;
            }
            else if (count2 == 0)
            {
                candidate2 = i;
                count2 = 1;
            }
            else
            {
                count1--;
                count2--;
            }
        }
        count1 = 0;
        count2 = 0;
        for(auto i : nums)
        {//第二轮扫描确定candidate是否符合要求
            if(candidate1 == i) count1++;
            else if(candidate2 == i) count2++;
        }
        if(count1 > numsSize/3) ret.push_back(candidate1);
        if(count2 > numsSize/3) ret.push_back(candidate2);
        return ret;
    }
};
```
## 230. 二叉搜索树中第K小的元素

![](230.png)

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
    void dfs(TreeNode* cur, vector<TreeNode*>& nodeList) {
        if (cur -> left != NULL)
            dfs(cur -> left, nodeList);
        nodeList.push_back(cur);
        if (cur -> right != NULL)
            dfs(cur -> right, nodeList);
    }
    int kthSmallest(TreeNode* root, int k) {
        vector<TreeNode*> nodeList;
        dfs(root, nodeList);
        return nodeList[k - 1] -> val;
    }
};
```
## 231. 2的幂

![](231.png)

```c++
class Solution {
public:
    bool isPowerOfTwo(int n) {
        return (n > 0) ? ((n & -n) == n) : false;
    }
};
```
## 232. 用栈实现队列

![](232.png)

```c++
class MyQueue {
public:
    /** Initialize your data structure here. */
    stack<int>sta, cache;
    MyQueue() {

    }

    /** Push element x to the back of queue. */
    void push(int x) {
        sta.push(x);
    }

    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        while (!sta.empty()) cache.push(sta.top()), sta.pop();
        int x = cache.top();
        cache.pop();
        while (!cache.empty()) sta.push(cache.top()), cache.pop();
        return x;
    }

    /** Get the front element. */
    int peek() {
        while (!sta.empty()) cache.push(sta.top()), sta.pop();
        int x = cache.top();
        while (!cache.empty()) sta.push(cache.top()), cache.pop();
        return x;
    }

    /** Returns whether the queue is empty. */
    bool empty() {
        return sta.empty();
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * bool param_4 = obj.empty();
 */
```
## 233. 数字 1 的个数

![](233.png)

```c++
class Solution {
public:
    int countDigitOne(int n) {
        if (n <= 0)
            return 0;

        vector<int> f(10, 0), pow(10, 0);
        f[0] = 0;
        pow[0] = 1;
        for (int i = 1; i <= 9; i++) {
            f[i] = f[i - 1] * 10 + pow[i - 1];
            pow[i] = pow[i - 1] * 10;
        }
        string num = to_string(n);
        reverse(num.begin(), num.end());
        int ans = 0, ones = 0;
        for (int i = num.length() - 1; i >= 0; i--) {
            ans += ones * ((num[i] - '0') * pow[i]);
            if (num[i] == '1') {
                ones++;
                ans += f[i];
            }
            else if (num[i] != '0')
                ans += pow[i] + f[i] * (num[i] - '0');
        }
        return ans + ones;
    }
};
```
## 234. 回文链表

![](234.png)

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
    bool isPalindrome(ListNode* head) {
        int n = 0;
        for (ListNode *p = head; p; p = p->next) n ++ ;
        if (n <= 1) return true;
        ListNode *a = head;
        for (int i = 0; i < (n + 1) / 2 - 1; i ++ ) a = a->next;
        ListNode *b = a->next;
        while (b)
        {
            ListNode *c = b->next;
            b->next = a;
            a = b;
            b = c;
        }
        b = head;
        ListNode *tail = a;
        bool res = true;
        for (int i = 0; i < n / 2; i ++ )
        {
            if (a->val != b->val)
            {
                res = false;
                break;
            }
            a = a->next;
            b = b->next;
        }
        a = tail, b = a->next;
        for (int i = 0; i < n / 2; i ++ )
        {
            ListNode *c = b->next;
            b->next = a;
            a = b;
            b = c;
        }
        tail->next = 0;
        return res;
    }
};
```
## 235. 二叉搜索树的最近公共祖先

![](235.png)

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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode *cur = root;
        while (1) {
            if (p -> val < cur -> val && q -> val < cur -> val)
                cur = cur -> left;
            else if (p -> val > cur -> val && q -> val > cur -> val)
                cur = cur -> right;
            else
                break;
        }
        return cur;
    }
};
```
## 236. 二叉树的最近公共祖先

![](236.png)

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
## 237. 删除链表中的节点

![](237.png)

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
    void deleteNode(ListNode* node) {
        node->val = node->next->val;
        node->next = node->next->next;
    }
};
```
## 238. 除自身以外数组的乘积

![](238.png)

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> output(n, 1);
        for (int i = 1; i < n; i++)
            output[i] = output[i - 1] * nums[i - 1];

        int end = 1;
        for (int i = n - 1; i >= 0; i--) {
            output[i] *= end;
            end *= nums[i];
        }
        return output;
    }
};
```
## 239. 滑动窗口最大值

![](239.png)

```c++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> res;
        deque<int> q;
        for(int i = 0;i < nums.size();i ++)
        {
            if(q.size() && i - k + 1 > q.front()) q.pop_front();
            while(q.size() && nums[q.back()] <= nums[i]) q.pop_back();
            q.push_back(i);
            if(i >= k - 1) res.push_back(nums[q.front()]);
        }
        return res;
    }
};
```
## 240. 搜索二维矩阵 II

![](240.png)

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        if (m == 0)
            return false;
        int n = matrix[0].size();

        int up = 0, right = n - 1;
        while (up < m && right >= 0) {
            if (matrix[up][right] == target)
                return true;
            else if (matrix[up][right] < target)
                up++;
            else
                right--;
        }

        return false;
    }
};
```