---
typora-root-url: img
---

## 162. 买卖股票的最佳时机 II

![](162.png)

```c++
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int l = 0,r = nums.size() - 1;
        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mid] > nums[mid + 1]) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```
## 164. 最大间距

![](164.png)

```c++
class Solution {
public:
    int maximumGap(vector<int>& nums) {
        if (nums.size()<2) return 0;
        int N = nums.size();
        int MaxNum = INT_MIN;
        int MinNum = INT_MAX;
        for (int i = 0;i<nums.size();++i){
            MaxNum = max(nums[i],MaxNum);
            MinNum = min(nums[i],MinNum);
        }
        if (MaxNum == MinNum) return 0;
        double len = (MaxNum-MinNum)*1.0/(N-1);

        int BucketSize = floor((MaxNum-MinNum)/len)+1;


        vector<int> minbucket(BucketSize,INT_MAX);
        vector<int> maxbucket(BucketSize,INT_MIN);



        for (int i = 0;i<nums.size();++i){
            int index = floor((nums[i]-MinNum)/len);
            minbucket[index] = min(minbucket[index],nums[i]);
            maxbucket[index] = max(maxbucket[index],nums[i]);
        }

        int delta = 0;
        int premax = maxbucket[0];
        for (int i = 1;i<BucketSize;++i){
            int curdelta = 0;
            if (minbucket[i]!=INT_MAX){
                delta = max(minbucket[i] - premax,delta);
                premax  = maxbucket[i];
            }

        }
        return delta;


    }
};
```
## 165. 比较版本号

![](165.png)

```c++
class Solution {
public:
    int compareVersion(string s1, string s2) {
        int i = 0,j = 0;
        while(i < s1.size() || j < s2.size())
        {
            int x = i,y = j;
            while(x < s1.size() && s1[x] != '.') x ++;
            while(y < s2.size() && s2[y] != '.') y ++;
            int a = i == x ? 0 : atoi(s1.substr(i, x - i).c_str());
            int b = j == y ? 0 : atoi(s2.substr(j, y - j).c_str());
            if(a > b) return 1;
            if(a < b) return -1;
            i = x + 1;
            j = y + 1;
        }
        return 0;
    }
};
```
## 166. 分数到小数

![](166.png)

```c++
class Solution {
public:
    string fractionToDecimal(int _n, int _d) {
        long long n = _n, d = _d;
        bool minus = false;
        if (n < 0) minus = !minus, n = -n;
        if (d < 0) minus = !minus, d = -d;
        string res = to_string(n / d);
        n %= d;
        if (!n)
        {
            if (minus && res != "0") return '-' + res;
            return res;
        }

        res += '.';
        unordered_map<long long, int> hash;
        while (n)
        {
            if (hash[n])
            {
                res = res.substr(0, hash[n]) + '(' + res.substr(hash[n]) + ')';
                break;
            }
            else hash[n] = res.size();
            n *= 10;
            res += to_string(n / d);
            n %= d;
        }
        if (minus) res = '-' + res;
        return res;
    }
};
```
## 167. 两数之和 II - 输入有序数组

![](167.png)

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        for(int i = 0,j = numbers.size() - 1;i < numbers.size();i ++)
        {
            while(numbers[i] + numbers[j] > target) j --;
            if(numbers[i] + numbers[j] == target) return {i + 1,j + 1};
        }
        return {-1,-1};
    }
};
```
## 168. Excel表列名称


![](168.png)

```c++
class Solution {
public:
    string convertToTitle(int n) {
        string res;
        while (n) {
            res = char ((n - 1) % 26 + 'A') + res;
            n = (n - 1) / 26; //注意corner case: n=26
        }
        return res;
    }
};
```
## 169. 多数元素

![](169.png)

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int, int> hash;
        for (int i = 0; i < nums.size(); i++) {
            hash[nums[i]] += 1;
            if (hash[nums[i]] > nums.size() / 2)
                return nums[i];
        }
    }
};
/*
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size(), t;
        while (n > 1) {
            t = 0;
            for (int i = 0; i < n; i += 2) {
                if (i == n - 1 || nums[i] == nums[i + 1])
                    nums[t++] = nums[i];
            }
            n = t;
        }
        return nums[0];
    }
};

class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int cnt = 0, candidate;
        for (int i = 0; i < nums.size(); i++) {
            if (cnt == 0)
                candidate = nums[i];
            if (candidate == nums[i])
                cnt++;
            else
                cnt--;
        }
        return candidate;
    }
};
*/
```
## 171. Excel表列序号

![](171.png)

```c++
class Solution {
public:
    int titleToNumber(string s) {
        int num = 0;
        for (int i = 0; i < s.length(); ++i) {
            num = 26 * num + (s[i] - 'A' + 1); //注意'A'不是0而是1
        }
        return num;
    }
};
```
## 172. 阶乘后的零

![](172.png)

```c++
class Solution {
public:
    int trailingZeroes(int n) {
        return n < 5 ? 0 : n/5 + trailingZeroes(n/5); //递归
    }
};
```
## 173. 二叉搜索树迭代器

![](173.png)

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
class BSTIterator {
public:
    stack<TreeNode*> stk;
    BSTIterator(TreeNode* root) {
        while(root)
        {
            stk.push(root);
            root = root->left;
        }
    }
    
    /** @return the next smallest number */
    int next() {
        auto p = stk.top();
        stk.pop();
        int res = p->val;
        p = p->right;
        while(p)
        {
            stk.push(p);
            p = p->left;
        }

        return res;
    }
    
    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !stk.empty();
    }
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```
## 174. 地下城游戏

![](174.png)

```c++
class Solution {
public:
    bool check(long long initial, vector<vector<int>>& dungeon) {
        int n = dungeon.size(), m = dungeon[0].size();
        vector<vector<long long>> f(n, vector<long long>(m, 0));
        f[0][0] = initial + dungeon[0][0];
        for (int i = 0; i < n; i++)
            for (int j = 0; j < m; j++) {
                if (i > 0 && f[i - 1][j] > 0)
                    f[i][j] = max(f[i][j], f[i - 1][j] + dungeon[i][j]);
                if (j > 0 && f[i][j - 1] > 0)
                    f[i][j] = max(f[i][j], f[i][j - 1] + dungeon[i][j]);
            }
        return f[n - 1][m - 1] > 0;
    }
    int calculateMinimumHP(vector<vector<int>>& dungeon) {
        long long l = 1, r = 10000000000000ll;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid, dungeon)) r = mid;
            else l = mid + 1;
        }
        return l;
    }
};
/*
class Solution {
public:
    int calculateMinimumHP(vector<vector<int>>& dungeon) {
        int n = dungeon.size(), m = dungeon[0].size();
        vector<vector<long long>> f(n, vector<long long>(m, 1000000000000ll));
        f[n - 1][m - 1] = max(-dungeon[n - 1][m - 1], 0) + 1;
        for (int i = n - 1; i >= 0; i--)
            for (int j = m - 1; j >= 0; j--) {
                if (i < n - 1)
                    f[i][j] = min(f[i][j], f[i + 1][j] - dungeon[i][j]);
                if (j < m - 1)
                    f[i][j] = min(f[i][j], f[i][j + 1] - dungeon[i][j]);
                f[i][j] = f[i][j] <= 0 ? 1 : f[i][j];
            }
        return f[0][0];
    }
};
*/
```
## 179. 最大数

![](179.png)

```c++
class Solution {
public:
    static bool cmp(int x, int y) {
        string sx = to_string(x), sy = to_string(y);
        return sx + sy > sy + sx;
    }
    string largestNumber(vector<int>& nums) {
        sort(nums.begin(), nums.end(), cmp);
        string ans = "";
        for (int i = 0; i < nums.size(); i++)
            ans += to_string(nums[i]);

        for (int i = 0; i < ans.length(); i++)
            if (i == ans.length() - 1 || ans[i] != '0')
                return ans.substr(i, ans.length() - i);
        return ans;
    }
};
```