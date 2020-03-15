---
typora-root-url: img/
---

## LeetCode 1.  两数之和

![](1.png)

![](1_1.png)

```c++
/*
暴力：两重循环

优化：开一个Hash表，hash表中是否存在target - nums[i]
	将nums[i]插入hash表
*/
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target)
    {
        vector<int> res;
        unordered_map<int,int> hash;
        for (int i = 0; i < nums.size(); i ++ )
        {
            int another = target - nums[i];
            if (hash.count(another))//是否存在
            {
                res = vector<int>({hash[another], i});
                break;
            }
            hash[nums[i]] = i;
        }
        return res;
    }
};
******************************************************
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> hash;
        for(int i = 0;i < nums.size();i ++)
        {
            if(hash.count(target - nums[i])) return {hash[target - nums[i]],i};
            hash[nums[i]] = i;
        }
        return {-1,-1};
    }
};
```

## LeetCode 187.  重复的DNA序列

![](187.png)

```c++
/*
用哈希表记录所有长度是10的子串的个数。
从前往后扫描，当子串出现第二次时，将其记录在答案中。
1.插入一个字符串
2.统计字符串出现的次数
*/
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        vector<string> res;
        unordered_map<string, int> S;
        for (int i = 0; i + 10 <= s.size(); i ++ )
        {
            string str = s.substr(i, 10);
            if (S[str] == 1) res.push_back(str);
            S[str] ++ ;
        }
        sort(res.begin(), res.end());
        return res;
    }
};
***********************************************************************************
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        unordered_map<string,int> hash;
        vector<string> res;

        for(int i = 0; i + 10 <= s.size();i ++)
        {
            string str = s.substr(i,10);
            hash[str] ++;
            if(hash[str] == 2) res.push_back(str);
        }
        return res;
    }
};
```

## LeetCode 706. 设计Hash表

![](706.png)

![](706_1.png)

```c++
/*

*/
class MyHashMap {
public:
    /** Initialize your data structure here. */
    const static int N = 20011;

    vector<list<pair<int,int>>> hash;

    MyHashMap() {
        hash = vector<list<pair<int,int>>>(N);
    }

    list<pair<int,int>>::iterator find(int key)
    {
        int t = key % N;
        auto it = hash[t].begin();
        for (; it != hash[t].end(); it ++ )
            if (it->first == key)
                break;
        return it;
    }

    /** value will always be non-negative. */
    void put(int key, int value) {
        int t = key % N;
        auto it = find(key);
        if (it == hash[t].end())
            hash[t].push_back(make_pair(key, value));
        else
            it->second = value;
    }

    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        auto it = find(key);
        if (it == hash[key % N].end())
            return -1;
        return it->second;
    }

    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int t = key % N;
        auto it = find(key);
        if (it != hash[t].end())
            hash[t].erase(it);
    }
};

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap obj = new MyHashMap();
 * obj.put(key,value);
 * int param_2 = obj.get(key);
 * obj.remove(key);
 */
```

![](706_2.png)

```c++
class MyHashMap {
public:
    /** Initialize your data structure here. */
    const static int N = 20011;
    int hash_key[N], hash_value[N];

    MyHashMap() {
        memset(hash_key, -1, sizeof hash_key);
    }

    int find(int key)
    {
        int t = key % N;
        while (hash_key[t] != key && hash_key[t] != -1)
        {
            if ( ++t == N) t = 0;
        }
        return t;
    }

    /** value will always be non-negative. */
    void put(int key, int value) {
        int t = find(key);
        hash_key[t] = key;
        hash_value[t] = value;
    }

    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int t = find(key);
        if (hash_key[t] == -1) return -1;
        return hash_value[t];
    }

    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int t = find(key);
        if (hash_key[t] != -1)
            hash_key[t] = -2;
    }
};

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap obj = new MyHashMap();
 * obj.put(key,value);
 * int param_2 = obj.get(key);
 * obj.remove(key);
 */
```

## LeetCode 652.寻找重复的子树

![](652.png)

![](652_1.png)

```c++
/*
先把一棵二叉树中序遍历成字符串
把结果的字符串hash成整数
再用一个hash求相同的
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
    string dfs(TreeNode *r, unordered_map<string, int> &hash, vector<TreeNode*> &ans) {
        if (r == NULL)
            return "";
        string cur = "";
        cur += to_string(r -> val) + ",";
        cur += dfs(r -> left, hash, ans) + ",";
        cur += dfs(r -> right, hash, ans);
        hash[cur]++;
        if (hash[cur] == 2)
            ans.push_back(r);
        return cur;
    }
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        unordered_map<string, int> hash;
        vector<TreeNode*> ans;
        dfs(root, hash, ans);
        return ans;
    }
};
***********************************************************************************
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

    int cnt = 0;//第几次遇到的字符串
    unordered_map<string,int> hash;
    unordered_map<int,int> count;
    vector<TreeNode*> ans;

    string dfs(TreeNode* root)
    {
        if(!root) return to_string(hash["#"]);

        auto left = dfs(root->left);
        auto right = dfs(root->right);
        string tree = to_string(root->val) + ',' + left + ',' + right;
        if(!hash.count(tree)) hash[tree] = ++ cnt;
        int t = hash[tree];//字符串hash之后的值
        count[t] ++;
        if(count[t] == 2) ans.push_back(root);

        return to_string(t);
    }
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        hash["#"] = ++ cnt; //空字符串用#代替
        dfs(root);
        return ans;
    }
};
```

## LeetCode 560. 和为K的子数组

![](560.png)

![](560_1.png)

```c++
/*
前缀和加Hash

暴力：枚举以i为终点，前面有多少个数等于s[i] - k
前缀和加Hash：前缀和算，Hash查
*/
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> hash;
        int tot = 0, ans = 0;
        hash[0] = 1;
        for (auto x : nums) {
            tot += x;
            ans += hash[tot - k];
            hash[tot]++;
        }
        return ans;
    }
};
*****************************************************************
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int,int> hash;
        hash[0] = 1;
        int res = 0;

        for(int i = 0,sum = 0;i < nums.size();i ++)
        {
            sum += nums[i];
            res += hash[sum - k];
            hash[sum] ++;
        }
        return res;
    }
};
```

## LeetCode 547. 朋友圈

![](547.png)

![](547_1.png)

```c++
/*
并查集作用O(1)
1.合并两个集合
2.判断连个点是否在同一集合中

*/
class Solution {
public:
    int n;
    vector<int> f;//父节点

    int find(int x) {
        return x == f[x] ? x : f[x] = find(f[x]);
    }
    int findCircleNum(vector<vector<int>>& M) {
        n = M.size();
        f = vector<int>(n);
        for (int i = 0; i < n; i++)
            f[i] = i;
        for (int i = 0; i < n; i++)
            for (int j = i + 1; j < n; j++)//无向图，遍历一半就可以
                if (M[i][j] == 1) {
                    int fx = find(i), fy = find(j);
                    if (fx != fy)
                        f[fx] = fy;
                }

        int ans = 0;
        for (int i = 0; i < n; i++)
            if (i == find(i))
                ans++;
        return ans;
    }
};
```

## LeetCode 684. 冗余连接

![](684.png)

```c++
/*
出现环的条件是某条边，边的两个端点原本就是连通的，那么加上了这条边以后就产生了环，因此我们在加入每条边的时候需要判断一下边的两个端点本身是不是连通的即可。

如果连通且不在一个集合之中就合并集合，否则这边就是要求的边
*/
class UnionFind{
public:
    vector<int>father;
    UnionFind(int num){//num表示元素的个数
        for(int i = 0; i < num; i++){
            father.push_back(i);//箭头指向自己
        }
    }
    int Find(int n){
        //递归
        if(father[n] == n)
            return n;
        father[n] = Find(father[n]);//路径压缩版本
        return father[n];
    }
    bool Union(int a, int b){//返回a和b是否本身在一个集合里
        int fa = Find(a);
        int fb = Find(b);
        bool res = fa==fb;
        father[fb] = fa;
        return res;
    }
};

class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int N = edges.size();
        UnionFind UF(N+1);
        vector<int>res(2, 0);
        for(int i = 0; i < edges.size(); i++){
            int u = edges[i][0];
            int v = edges[i][1];
            if(UF.Union(u, v)){//冗余
                res[0] = u;
                res[1] = v;
            }
        }
        return res;
    }
};
*******************************************************************
class Solution {
public:
    vector<int> p;

    int find(int x)
    {
        if(p[x] != x) p[x] = find(p[x]);
        return p[x];
    }
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();

        for(int i = 0;i <= n;i ++) p.push_back(i);

        for(auto e : edges)
        {
            int a = e[0],b = e[1];
            if(find(a) == find(b)) return {a,b};
            p[find(a)] = find(b);
        }
        return {-1,-1};
    }
};
```

## LeetCode 692.前K个高频单词

![](692.png)

![](692_1.png)

```c++
/*
找出出现次数最多的k个单词
用小根堆来委会出现次数最多的k个单词
*/
class Solution {
public:

    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string, int> hash;

        typedef pair<int, string> PIS;
        priority_queue<PIS> heap;
        for (auto word : words) hash[word] ++ ;

        for (auto item : hash)
        {
            PIS t(-item.second, item.first);
            if (heap.size() == k && t < heap.top()) heap.pop();
            if (heap.size() < k) heap.push(t);
        }

        vector<string> res(k);
        for (int i = k - 1; i >= 0; i -- )
        {
            res[i] = heap.top().second;
            heap.pop();
        }

        return res;
    }
};
******************************************************************
class Solution {
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string,int> hash;
        typedef pair<int,string> PIS;
        priority_queue<PIS> heap;

        for(auto word : words) hash[word] ++;

        for(auto item : hash)
        {
            PIS t(-item.second,item.first);
            heap.push(t);
            if(heap.size() > k) heap.pop(); 
        }

        vector<string> res(k);
        for(int i = k - 1;i >= 0;i --)
        {
            res[i] = heap.top().second;
            heap.pop();
        }
        return res;
    }
};
```

## LeetCode 295.数据流的中位数

![](295.png)

![](295_1.png)

![](295_2.png)

```c++
//维护下面的比上面的最多少一个！！！
class MedianFinder {
public:
    /** initialize your data structure here. */
    priority_queue<int> smaller;
    priority_queue<int, vector<int>, greater<int>> larger;
    MedianFinder() {

    }

    void addNum(int num) {
        if (smaller.empty() || num <= smaller.top())
            smaller.push(num);
        else
            larger.push(num);

        if (smaller.size() == larger.size() + 2) {
            int top = smaller.top();
            smaller.pop();
            larger.push(top);
        }
        else if (larger.size() == smaller.size() + 1) {
            int top = larger.top();
            larger.pop();
            smaller.push(top);
        }
    }

    double findMedian() {
        if (smaller.size() == larger.size())
            return (smaller.top() + larger.top()) / 2.0;

        return smaller.top();
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
*****************************************************************************
class MedianFinder {
public:
    /** initialize your data structure here. */
    priority_queue<int> down;//下面的
    priority_queue<int,vector<int>,greater<int>> up;//上面的
    MedianFinder() {

    }
    
    void addNum(int num) {
        if(down.empty() || num >= down.top()) up.push(num);
        else
        {
            down.push(num);//每次要往下面添加时，先添加，再往上拿一个，拿下面最大的
            up.push(down.top());
            down.pop();
        }
        if(up.size() > down.size() + 1)//上面多了，往下拿最小的
        {
            down.push(up.top());
            up.pop();
        }
    }
    
    double findMedian() {
        if(down.size() + up.size() & 1) return up.top();//总数是奇数
        else return (down.top() + up.top()) / 2.0;//总数是偶数
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

## LeetCode 352. 将数据流变为多个不相交区间

![](352.png)

![](352_1.png)

```c++
/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */
class SummaryRanges {
public:
    /** Initialize your data structure here. */
    map<int,int>L, R;
    SummaryRanges() {

    }

    void addNum(int val) {
        if (R.size())
        {
            auto it = R.upper_bound(val);
            if (it != R.begin())
            {
                -- it;
                if (it->second >= val) return;
            }
        }
        int right = R.count(val + 1), left = L.count(val - 1);
        if (left && right)
        {
            R[L[val - 1]] = R[val + 1];
            L[R[val + 1]] = L[val - 1];
            R.erase(val + 1), L.erase(val - 1);
        }
        else if (right)
        {
            L[R[val + 1]] = val;
            R[val] = R[val + 1];
            R.erase(val + 1);
        }
        else if (left)
        {
            R[L[val - 1]] = val;
            L[val] = L[val - 1];
            L.erase(val - 1);
        }
        else
        {
            R[val] = val;
            L[val] = val;
        }
    }

    vector<Interval> getIntervals() {
        vector<Interval> res;
        for (auto &p : R) res.push_back(Interval(p.first, p.second));
        return res;
    }
};

/**
 * Your SummaryRanges object will be instantiated and called as such:
 * SummaryRanges obj = new SummaryRanges();
 * obj.addNum(val);
 * vector<Interval> param_2 = obj.getIntervals();
 */
```