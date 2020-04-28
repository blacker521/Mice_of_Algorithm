---
typora-root-url: img
---

## 283. 移动零

![](283.png)

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int i = 0;
        for (int j = 0; j < nums.size(); j++)
            if (nums[j] != 0)
                nums[i++] = nums[j];
        for (; i < nums.size(); i++)
            nums[i] = 0;
    }
};
```
## 284. 顶端迭代器

![](284.png)

```c++
// Below is the interface for Iterator, which is already defined for you.
// **DO NOT** modify the interface for Iterator.
class Iterator {
    struct Data;
    Data* data;
public:
    Iterator(const vector<int>& nums);
    Iterator(const Iterator& iter);
    virtual ~Iterator();
    // Returns the next element in the iteration.
    int next();
    // Returns true if the iteration has more elements.
    bool hasNext() const;
};


class PeekingIterator : public Iterator {
public:
    int _next;
    bool _has_next;

    PeekingIterator(const vector<int>& nums) : Iterator(nums) {
        // Initialize any member here.
        // **DO NOT** save a copy of nums and manipulate it directly.
        // You should only use the Iterator interface methods.
        _has_next = Iterator::hasNext();
        if (_has_next)
            _next = Iterator::next();
    }

    // Returns the next element in the iteration without advancing the iterator.
    int peek() {
        return _next;
    }

    // hasNext() and next() should behave the same as in the Iterator interface.
    // Override them if needed.
    int next() {
        int res = _next;
        _has_next = Iterator::hasNext();
        if (_has_next)
            _next = Iterator::next();
        return res;
    }

    bool hasNext() const {
        return _has_next;
    }
};
```
## 287. 寻找重复数

![](287.png)

```c++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int l = 1,r = nums.size() - 1;
        while(l < r)
        {
            int mid = l + r >> 1;
            int cnt = 0;
            for(auto x : nums)
                if(x >= l && x <= mid)
                    cnt ++;

            if(cnt > mid - l + 1) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```
## 289. 生命游戏

![](289.png)

```c++
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        int m = board.size();
        if(m==0)
            return;
        int n = board[0].size();
        for(int i = 0;i < m;i++){
            for(int j = 0;j < n;j++){
                int neighbour = 0;//记录周围八个网格的活细胞数
                for(int di = -1;di <= 1; di++){
                    for(int dj = -1;dj <= 1;dj++){
                        if(i + di >=0 && i+di < m && j+dj >= 0 && j+dj < n &&!(di == 0 && dj == 0)){
                            neighbour += board[i+di][j+dj] & 1;//取每个网格的最低位（当前状态）
                        }
                    }
                }
                    if(board[i][j] == 1){
                        if(neighbour < 2 || neighbour > 3)
                            board[i][j] = 1;//更新后状态为0，所以记为01（二进制）
                        else
                            board[i][j] = 3;//更新后状态为1，所以记为11（二进制）
                    }
                    else{
                        if(neighbour == 3)
                            board[i][j]=2;//更新后状态为1，所以记为10（二进制）
                        else
                            board[i][j] = 0;//更新后状态为0，所以记为00（二进制）
                    }
                }
            }

        for(int i = 0;i<m;i++){
            for(int j = 0;j<n;j++){
                board[i][j] =(board[i][j]&2)>>1;//取每个网格的第二位，得到更新后状态
            }
        }
    }
};
```
## 290. 单词规律

![](290.png)

```c++
class Solution {
public:
    bool wordPattern(string pattern, string str) {
        stringstream raw(str);
        vector<string> strs;
        string line;
        while (raw >> line) strs.push_back(line);
        if (pattern.size() != strs.size()) return false;
        unordered_map<char, string> PS;
        unordered_map<string, char> SP;
        for (int i = 0; i < pattern.size(); i ++ )
        {
            if (PS.count(pattern[i]) == 0) PS[pattern[i]] = strs[i];
            if (SP.count(strs[i]) == 0) SP[strs[i]] = pattern[i];
            if (PS[pattern[i]] != strs[i]) return false;
            if (SP[strs[i]] != pattern[i]) return false;
        }
        return true;
    }
};
```
## 292. Nim 游戏

![](292.png)

```c++
class Solution {
public:
    bool canWinNim(int n) {
        return n&3;
    }
};
```
## 295. 数据流的中位数

![](295.png)

```c++
class MedianFinder {
public:
    /** initialize your data structure here. */
    priority_queue<int> down;
    priority_queue<int,vector<int>,greater<int>> up;
    MedianFinder() {

    }
    
    void addNum(int num) {
        if(down.empty() || num >= down.top()) up.push(num);
        else
        {
            down.push(num);
            up.push(down.top());
            down.pop();
        }
        if(up.size() > down.size() + 1)
        {
            down.push(up.top());
            up.pop();
        }
    }
    
    double findMedian() {
        if(down.size() + up.size() & 1) return up.top();
        else return (down.top() + up.top()) / 2.0;
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```
## 297. 二叉树的序列化与反序列化

![](297.png)

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
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string res;
        dfs1(root,res);
        return res;
    }
    void dfs1(TreeNode *root,string &res)
    {
        if(!root)
        {
            res += "#,";
            return;
        }

        res += to_string(root->val) + ',';
        dfs1(root->left,res);
        dfs1(root->right,res);
    }
    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        int u = 0;
        return dfs2(data,u);
        
    }
    TreeNode* dfs2(string &data,int &u)
    {
        if(data[u] == '#')
        {
            u += 2;
            return NULL;
        }

        int t = 0;
        bool is_minus = false;
        if(data[u] == '-')
        {
            is_minus = true;
            u ++;
        }
        while(data[u] != ',')
        {
            t = t * 10 + data[u] - '0';
            u ++;
        }
        u ++;
        if(is_minus) t = -t;

        auto root = new TreeNode(t);
        root->left = dfs2(data,u);
        root->right = dfs2(data,u);

        return root;

    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.deserialize(codec.serialize(root));
```
## 299. 猜数字游戏

![](299.png)

```c++
class Solution {
public:
    string getHint(string secret, string guess) {
        int n = secret.length(), bulls = 0, cows = 0;
        vector<int> vis(10, 0);
        for (int i = 0; i < n; i++) {
            vis[secret[i] - '0']++;
            if (secret[i] == guess[i])
                bulls++;
        }
        for (int i = 0; i < n; i++)
            if (vis[guess[i] - '0'] > 0) {
                cows++;
                vis[guess[i] - '0']--;
            }
        return to_string(bulls) + "A" + to_string(cows - bulls) + "B";
    }
};
```
## 300. 最长上升子序列

![](300.png)

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(n);

        for(int i = 0;i < n;i ++)
        {
            f[i] = 1;
            for(int j = 0;j < i;j ++)
                if(nums[j] < nums[i])
                    f[i] = max(f[i],f[j] + 1);
        }

        int res = 0;
        for(int i = 0;i < n;i ++) res = max(res,f[i]);
        return res;
    }
};
```