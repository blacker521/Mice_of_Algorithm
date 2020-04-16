---
typora-root-url: img
---

## 121. 买卖股票的最佳时机

![](121.png)

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() < 2 ) return 0;
        int profit = 0,low = prices[0];
        for(int i = 0;i < prices.size();i ++)
        {
            profit = max(profit,prices[i] - low);
            low = min(low,prices[i]);
        }
        return profit;
    }
};
```
## 122. 买卖股票的最佳时机 II

![](122.png)

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int sum = 0;
        for(int i = 1;i < prices.size();i ++)
            if(prices[i] > prices[i - 1])
                sum += prices[i] - prices[i - 1];
        return sum;
    }
};
/*
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        int f, g;
        f = 0;
        g = -1000000000;
        for (int i = 0; i < n; i++) {
            int last_f = f, last_g = g;
            f = max(last_f, last_g + prices[i]);
            g = max(last_g, last_f - prices[i]);
        }
        return f;
    }
};
*/
```
## 123. 买卖股票的最佳时机 III

![](123.png)

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if (!n) return 0;
        vector<int> f(n, 0);
        int minv = INT_MAX;
        for (int i = 0; i < n; i ++ )
        {
            if (i) f[i] = f[i - 1];
            if (prices[i] > minv)
                f[i] = max(f[i], prices[i] - minv);
            minv = min(minv, prices[i]);
        }
        int res = f[n - 1];
        int maxv = INT_MIN;
        for (int i = n - 1; i > 0; i -- )
        {
            if (prices[i] < maxv)
                res = max(res, maxv - prices[i] + f[i - 1]);
            maxv = max(maxv, prices[i]);
        }
        return res;
    }
};
```
## 124. 二叉树中的最大路径和

![](124.png)

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
## 125. 验证回文串

![](125.png)

```c++
class Solution {
public:
    bool check(char c)
    {
        return c >= '0' && c <= '9' || c >= 'a' && c <= 'z' || c >= 'A' && c <= 'Z';
    }

    bool isPalindrome(string s) {
        int i = 0,j = (int)s.size() - 1;
        while(i < j)
        {
            while(i < j && !check(s[i])) i ++;
            while(i < j && !check(s[j])) j --;
            if(s[i] != s[j] && s[i] != (s[j] ^ 32)) return false;
            i ++,j --;
        }
        return true;
    }
};
```
## 126. 单词接龙 II

![](126.png)

## 127. 单词接龙

![](127.png)

```c++
class Solution {
public:
    bool check(string a, string b)
    {
        int res = 0;
        for (int i = 0; i < a.size(); i ++ ) res += a[i] != b[i];
        return res == 1;
    }

    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_map<string, int>dist;
        queue<string> que;
        que.push(beginWord), dist[beginWord] = 1;
        while (!que.empty())
        {
            string t = que.front();
            que.pop();

            if (t == endWord) return dist[t];
            for (auto &word : wordList)
                if (check(t, word) && !dist[word])
                {
                    dist[word] = dist[t] + 1;
                    que.push(word);
                }
        }
        return 0;
    }
};
```
## 128. 最长连续序列

![](128.png)

```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int res = 0;
        unordered_map<int,int> tr_left,tr_right;
        for(auto & x : nums)
        {
            int left = tr_right[x - 1];
            int right = tr_left[x + 1];
            tr_left[x - left] = max(tr_left[x - left],left + 1 + right);
            tr_right[x + right] = max(tr_right[x + right],left + 1 + right);
            res = max(res,left + 1 + right);
        }
        return res;
    }
};
```
## 129. 求根到叶子节点数字之和

![](129.png)

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
    int ans = 0;
    int sumNumbers(TreeNode* root) {
        dfs(root,0);
        return ans;
    }

    void dfs(TreeNode* u,int s)
    {
        if(!u) return;
        s = s * 10 + u->val;
        if(u->left) dfs(u->left,s);
        if(u->right) dfs(u->right,s);
        if(!u->left && !u->right) ans += s;
    }
};
```
## 130. 被围绕的区域

![](130.png)

```c++
class Solution {
public:
    vector<vector<bool>> st;
    int n, m;

    void solve(vector<vector<char>>& board) {
        if (board.empty()) return;
        n = board.size(), m = board[0].size();
        for (int i = 0; i < n; i ++ )
        {
            vector<bool> temp;
            for (int j = 0; j < m; j ++ )
                temp.push_back(false);
            st.push_back(temp);
        }

        for (int i = 0; i < n; i ++ )
        {
            if (board[i][0] == 'O') dfs(i, 0, board);
            if (board[i][m - 1] == 'O') dfs(i, m - 1, board);
        }
        for (int i = 0; i < m; i ++ )
        {
            if (board[0][i] == 'O') dfs(0, i, board);
            if (board[n - 1][i] == 'O') dfs(n - 1, i, board);
        }

        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (!st[i][j])
                    board[i][j] = 'X';
    }

    void dfs(int x, int y, vector<vector<char>>&board)
    {
        st[x][y] = true;
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && !st[a][b] && board[a][b] == 'O')
                dfs(a, b, board);
        }
    }
};

```
## 131. 分割回文串

![](131.png)

```c++
class Solution {
public:
    vector<vector<string>> ans;
    vector<string> path;

    vector<vector<string>> partition(string s) {
        dfs("", 0, s);
        return ans;
    }

    bool check(string &now)
    {
        if (now.empty()) return false;
        for (int i = 0, j = now.size() - 1; i < j; i ++, j -- )
            if (now[i] != now[j])
                return false;
        return true;
    }

    void dfs(string now, int u, string &s)
    {
        if (u == s.size())
        {
            if (check(now))
            {
                path.push_back(now);
                ans.push_back(path);
                path.pop_back();
            }
            return;
        }

        if (check(now))
        {
            path.push_back(now);
            dfs("", u, s);
            path.pop_back();
        }

        dfs(now + s[u], u + 1, s);
    }
};

```
## 132. 分割回文串 II

![](132.png)

```c++
class Solution {
public:
    int minCut(string s) {
        int n = s.size();
        vector<int> f(n + 1);
        vector<vector<bool>> st(n,vector<bool>(n,false));

        for(int i = 0;i < n;i ++)
            for(int j = i;j >= 0;j --)
                if(i - j <= 1) st[j][i] = s[j] == s[i];
                else st[j][i] = s[j] == s[i] && st[j + 1][i - 1];
        
        f[0] = 0;
        for(int i = 1;i <= n;i ++)
        {
            f[i] = INT_MAX;
            for(int j = 0;j < i;j ++)
                if(st[j][i - 1])
                    f[i] = min(f[i],f[j] + 1);
        }
        return max(0,f[n] - 1);
    }
};
```
## 133. 克隆图

![](133.png)

```c++
/**
 * Definition for undirected graph.
 * struct UndirectedGraphNode {
 *     int label;
 *     vector<UndirectedGraphNode *> neighbors;
 *     UndirectedGraphNode(int x) : label(x) {};
 * };
 */
class Solution {
public:
    unordered_map<UndirectedGraphNode*, UndirectedGraphNode*> hash;

    UndirectedGraphNode *cloneGraph(UndirectedGraphNode *node) {
        if (!node) return 0;
        auto root = new UndirectedGraphNode(node->label);
        hash[node] = root;
        dfs(node);
        return root;
    }

    void dfs(UndirectedGraphNode* node)
    {
        for (auto &neighbor : node->neighbors)
        {
            if (!hash.count(neighbor))
            {
                hash[neighbor] = new UndirectedGraphNode(neighbor->label);
                dfs(neighbor);
            }
            hash[node]->neighbors.push_back(hash[neighbor]);
        }
    }
};
```
## 134. 加油站

![](134.png)

```c++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int n = gas.size();
        vector<int> sum = vector<int>(n * 2, 0);
        for (int i = 0; i < n * 2; i++)
            sum[i] = gas[i % n] - cost[i % n];

        int start = 0, end = 0, tot = 0;
        while (start < n && end <= 2 * n) {
            tot += sum[end];
            while (tot < 0) {
                tot -= sum[start];
                start++;
            }
            if (end - start + 1 == n)
                return start;
            end++;
        }
        return -1;
    }
};
```
## 135. 分发糖果

![](135.png)

## 136. 只出现一次的数字 !!!

![](136.png)

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        for (int i = 1; i < nums.size(); i++)
            nums[0] ^= nums[i];
        return nums[0];
    }
};
```
## 137. 只出现一次的数字 II

![](137.png)

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0;
        for (int bit = 0; bit < 32; bit++) {
            int counter = 0;
            for (int i = 0; i < nums.size(); i++)
                counter += (nums[i] >> bit) & 1;
            ans += (counter % 3) << bit;
        }
        return ans;
    }
};
```
## 138. 复制带随机指针的链表

![](138.png)

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;

    Node() {}

    Node(int _val, Node* _next, Node* _random) {
        val = _val;
        next = _next;
        random = _random;
    }
};
*/
class Solution {
public:
    Node *copyRandomList(Node *head) {
        if (!head) return 0;
        unordered_map<Node*, Node*> hash;
        Node *root = new Node(head->val, NULL, NULL);
        hash[head] = root;
        while (head->next)
        {
            if (hash.count(head->next) == 0)
                hash[head->next] = new Node(head->next->val, NULL, NULL);
            hash[head]->next = hash[head->next];

            if (head->random && hash.count(head->random) == 0)
                hash[head->random] = new Node(head->random->val, NULL, NULL);
            hash[head]->random = hash[head->random];

            head = head->next;
        }

        if (head->random && hash.count(head->random) == 0)
            hash[head->random] = new Node(head->random->val, NULL, NULL);
        hash[head]->random = hash[head->random];

        return root;
    }
};
```
## 139. 单词拆分

![](139.png)

```c++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> dict(wordDict.begin(),wordDict.end());

        vector<bool> dp(s.size() + 1,false);//dp表示字符之间的隔板，n个字符有n+1个隔板
        dp[0] = true;//dp[0]是s[0]前面的隔板

        for(int i = 1;i <= s.size(); i++)
        {
            for(int j = i;j >= 0;j --)
                if(dict.find(s.substr(j,i - j)) != dict.end() && dp[j])
                {
                    dp[i] = true;
                    break;
                }
                    
        }
        return dp[s.size()];
    }
    
};
```
## 140. 单词拆分 II

![](140.png)
