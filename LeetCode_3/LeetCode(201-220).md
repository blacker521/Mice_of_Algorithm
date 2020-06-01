---
typora-root-url: img
---

## 201. 数字范围按位与

![](201.png)

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
## 202. 快乐数

![](202.png)

```c++
class Solution {
public:
    bool isHappy(int n) {
        unordered_set<int> got;
        int m = n; // m是每次用来被拆解的数
        while(true){
            int sum = 0;
            while(m != 0){
                sum += (m % 10) * (m % 10);
                m /= 10;
            }
            if(sum == 1)
                return true;
            else if(got.find(sum)!=got.end())
                return false;
            else
                got.insert(sum);
            m = sum;
        }
    }
};
```
## 203. 移除链表元素

![](203.png)

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
    ListNode* removeElements(ListNode* head, int val) {
        ListNode *dummy = new ListNode(-1);
        dummy->next = head;
        for (ListNode *p = dummy; p;)
        {
            if (p->next && p->next->val == val)
                p->next = p->next->next;
            else
                p = p->next;
        }
        return dummy->next;
    }
};
```
## 204. 计数质数

![](204.png)

```c++
class Solution {
public:
    int countPrimes(int n) {
        vector<int> primes;
        vector<bool> st(n + 1);
        for (int i = 2; i < n; i ++ )
        {
            if (!st[i]) primes.push_back(i);
            for (int j = 0; i * primes[j] < n; j ++ )
            {
                st[i * primes[j]] = true;
                if (i % primes[j] == 0) break;
            }
        }
        return primes.size();
    }
};
```
## 205. 同构字符串

![](205.png)

```c++
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        unordered_map<char, char> st, ts;
        if (s.size() != t.size()) return false;
        for (int i = 0; i < s.size(); i ++ )
        {
            if (st.count(s[i]))
            {
                if (st[s[i]] != t[i]) return false;
            }
            else st[s[i]] = t[i];

            if (ts.count(t[i]))
            {
                if (ts[t[i]] != s[i]) return false;
            }
            else ts[t[i]] = s[i];
        }
        return true;
    }
};
```
## 206. 反转链表

![](206.png)

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
    ListNode* reverseList(ListNode* head) {
        if(!head) return NULL;
        if(!head->next) return head;
        ListNode *p = reverseList(head->next);
        head->next->next = head;
        head->next = NULL;
        return p;
    }
};
```
## 207. 课程表

![](207.png)

```c++
class Solution {
public:
    bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
        vector<vector<int>> graph(numCourses);
        vector<int> in_degree(numCourses, 0);
        for (int i = 0; i < prerequisites.size(); i++) {
            in_degree[prerequisites[i].first]++;
            graph[prerequisites[i].second].push_back(prerequisites[i].first);
        }

        queue<int> q;
        vector<bool> vis(numCourses, false);

        for (int i = 0; i < numCourses; i++)
            if (in_degree[i] == 0)
                q.push(i);
        while (!q.empty()) {
            int sta = q.front();
            q.pop();
            vis[sta] = true;
            for (int i = 0; i < graph[sta].size(); i++) {
                in_degree[graph[sta][i]]--;
                if (in_degree[graph[sta][i]] == 0)
                    q.push(graph[sta][i]);
            }
        }

        for (int i = 0; i < numCourses; i++)
            if (vis[i] == false)
                return false;
        return true;
    }
};
```
## 208. 实现 Trie (前缀树)

![](208.png)

```c++
class Trie {
public:
    struct Node
    {
        bool is_end;
        Node *son[26];
        Node()
        {
            is_end = false;
            for(int i = 0;i < 26;i ++) son[i] = NULL;
        }
    }*root;
    /** Initialize your data structure here. */
    Trie() {
        root = new Node();
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        auto p = root;
        for(auto c : word)
        {
            int u = c - 'a';
            if(p->son[u] == NULL) p->son[u] = new Node();
            p = p->son[u];
        }
        p->is_end = true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        auto p = root;
        for(auto c : word)
        {
            int u = c - 'a';
            if(p->son[u] == NULL) return false;
            p = p->son[u];
        }
        return p->is_end;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        auto p = root;
        for(auto c : prefix)
        {
            int u = c - 'a';
            if(p->son[u] == NULL) return false;
            p = p->son[u];
        }
        return true;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```
## 209. 长度最小的子数组

![](209.png)

```c++
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        int n = nums.size();
        int start = 0, end = 0, tot = 0, ans = n + 1;
        while (end < n) {
            tot += nums[end];
            while (tot >= s) {
                ans = min(ans, end - start + 1);
                tot -= nums[start];
                start++;
            }
            end++;
        }
        if (ans == n + 1)
            ans = 0;
        return ans;
    }
};
```
## 210. 课程表 II

![](210.png)

```c++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<pair<int, int>>& prerequisites) {
        vector<vector<int>> graph(numCourses);
        vector<int> in_degree(numCourses, 0);
        for (int i = 0; i < prerequisites.size(); i++) {
            in_degree[prerequisites[i].first]++;
            graph[prerequisites[i].second].push_back(prerequisites[i].first);
        }

        queue<int> q;
        vector<bool> vis(numCourses, false);
        vector<int> ans;

        for (int i = 0; i < numCourses; i++)
            if (in_degree[i] == 0)
                q.push(i);
        while (!q.empty()) {
            int sta = q.front();
            q.pop();
            ans.push_back(sta);
            vis[sta] = true;
            for (int i = 0; i < graph[sta].size(); i++) {
                in_degree[graph[sta][i]]--;
                if (in_degree[graph[sta][i]] == 0)
                    q.push(graph[sta][i]);
            }
        }

        for (int i = 0; i < numCourses; i++)
            if (vis[i] == false)
                return vector<int>{};
        return ans;
    }
};
```
## 211. 添加与搜索单词 - 数据结构设计

![](211.png)

```c++
class WordDictionary {
public:
    struct Node
    {
        bool is_end;
        Node *next[26];
        Node()
        {
            is_end = false;
            for (int i = 0; i < 26; i ++ )
                next[i] = 0;
        }
    };

    Node *root;

    /** Initialize your data structure here. */
    WordDictionary() {
        root = new Node();
    }

    /** Adds a word into the data structure. */
    void addWord(string word) {
        Node *p = root;
        for (char c : word)
        {
            int son = c - 'a';
            if (!p->next[son]) p->next[son] = new Node();
            p = p->next[son];
        }
        p->is_end = true;
    }

    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    bool search(string word) {
        return dfs(word, 0, root);
    }

    bool dfs(string &word, int k, Node *u)
    {
        if (k == word.size()) return u->is_end;
        if (word[k] != '.')
        {
            if (u->next[word[k] - 'a']) return dfs(word, k + 1, u->next[word[k] - 'a']);
        }
        else
        {
            for (int i = 0; i < 26; i ++ )
                if (u->next[i])
                {
                    if (dfs(word, k + 1, u->next[i])) return true;
                }
        }
        return false;
    }
};

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * bool param_2 = obj.search(word);
 */
```
## 212. 单词搜索 II

![](212.png)

```c++
class Solution {
public:

    struct Node
    {
        int id;
        Node *next[26];
        Node()
        {
            id = -1;
            for (int i = 0; i < 26; i ++ )
                next[i] = 0;
        }
    };

    Node *root;

    void insert(string word, int id) {
        Node *p = root;
        for (char c : word)
        {
            int son = c - 'a';
            if (!p->next[son]) p->next[son] = new Node();
            p = p->next[son];
        }
        p->id = id;
    }

    unordered_set<string> hash;
    vector<string> ans;
    vector<vector<bool>> st;
    vector<string> _words;
    int n, m;

    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        if (board.empty()) return ans;
        _words = words;
        root = new Node();
        for (int i = 0; i < words.size(); i ++ ) insert(words[i], i);
        n = board.size(), m = board[0].size();
        st = vector<vector<bool>>(n, vector<bool>(m, false));
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                dfs(board, i, j, root->next[board[i][j]-'a']);
        return ans;
    }

    void dfs(vector<vector<char>>& board, int x, int y, Node *u)
    {
        if (!u) return;
        st[x][y] = true;
        if (u->id != -1)
        {
            string match = _words[u->id];
            if (!hash.count(match))
            {
                hash.insert(match);
                ans.push_back(match);
            }
        }
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && !st[a][b])
            {
                char c = board[a][b];
                dfs(board, a, b, u->next[c-'a']);
            }
        }
        st[x][y] = false;
    }
};
```
## 213. 打家劫舍 II

![](213.png)

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) return 0;
        if (n == 1) return nums[0];
        if (n == 2) return max(nums[0], nums[1]);

        int ans = 0, f, g;
        // choose the first one
        f = nums[2]; g = 0;
        for (int i = 3; i < n; i++) {
            int last_f = f, last_g = g;
            f = last_g + nums[i];
            g = max(last_f, last_g);
        }
        ans = g + nums[0];

        // not choose the first one
        f = nums[1]; g = 0;
        for (int i = 2; i < n; i++) {
            int last_f = f, last_g = g;
            f = last_g + nums[i];
            g = max(last_f, last_g);
        }
        ans = max(ans, max(f, g));
        return ans;
    }
};
```
## 214. 最短回文串

![](214.png)

```c++
class Solution {
public:
    string shortestPalindrome(string s) {
        int n = s.length();
        if (n == 0)
            return "";
        string t(s.rbegin(), s.rend());
        vector<int> nxt(n);
        nxt[0] = -1;
        int j = -1;
        for (int i = 1; i < n; i++) {
            while (j > -1 && s[i] != s[j + 1]) j = nxt[j];
            if (s[i] == s[j + 1])
                j++;
            nxt[i] = j;
        }

        j = -1;
        for (int i = 0; i < n; i++) {
            while (j > -1 && t[i] != s[j + 1]) j = nxt[j];
            if (t[i] == s[j + 1])
                j++;
        }
        return t + s.substr(j + 1, n - j - 1);
    }
};
```
## 215. 数组中的第K个最大元素

![](215.png)

```c++
class Solution {
public:
    int solve(int l, int r, vector<int>& nums, int k) {
        if (l == r)
            return nums[l];
        int pivot = nums[l], i = l, j = r;
        while (i < j) {
            while (i < j && nums[j] < pivot) j--;
            if (i < j)
                nums[i++] = nums[j];

            while (i < j && nums[i] >= pivot) i++;
            if (i < j)
                nums[j--] = nums[i];
        }
        if (i + 1 == k)
            return pivot;
        else if (i + 1 > k)
            return solve(l, i - 1, nums, k);
        else
            return solve(i + 1, r, nums, k);

    }
    int findKthLargest(vector<int>& nums, int k) {
        int n = nums.size();
        return solve(0, n - 1, nums, k);
    }
};
```
## 216. 组合总和 III

![](216.png)

```c++
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;
    vector<vector<int>> combinationSum3(int k, int n) {
        dfs(k,1,n);
        return ans;
    }

    void dfs(int k,int start,int n)
    {
        if(!k)
        {
            if(!n) ans.push_back(path);
            return;
        }

        for(int i = start;i <= 9;i ++)
        {
            path.push_back(i);
            dfs(k - 1,i + 1,n - i);
            path.pop_back();
        }
    }
};
```
## 217. 存在重复元素

![](217.png)

```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> hash;
        for (int i = 0; i < nums.size(); i++) {
            if (hash.find(nums[i]) != hash.end())
                return true;
            hash.insert(nums[i]);
        }

        return false;
    }
};
```
## 218. 天际线问题

![](218.png)

## 219. 存在重复元素 II

![](219.png)

```c++
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int, int> hash;
        for (int i = 0; i < nums.size(); i++) {
            if (hash.find(nums[i]) != hash.end()) {
                if (i - hash[nums[i]] <= k)
                    return true;
            }
            hash[nums[i]] = i;
        }

        return false;
    }
};
```
## 220. 存在重复元素 III

![](220.png)

```c++
#define LL long long
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
        multiset<LL> hash;
        multiset<LL>::iterator it;
        for (int i = 0; i < nums.size(); i++) {
            it = hash.lower_bound((LL)(nums[i]) - t);
            if (it != hash.end() && *it <= (LL)(nums[i]) + t)
                return true;
            hash.insert(nums[i]);
            if (i >= k)
                hash.erase(hash.find(nums[i - k]));
        }
        return false;
    }
};
```