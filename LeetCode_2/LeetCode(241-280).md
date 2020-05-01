---
typora-root-url: img
---

## 241. 为运算表达式设计优先级

![](241.png)

```c++
class Solution {
public:
    vector<int> solve(int l, int r, const vector<int>& input) {
        if (l > r)
            return vector<int>{};
        if (l == r)
            return vector<int>{input[l]};
        vector<int> sum;
        for (int i = l + 1; i < r; i++) {
            vector<int> left = solve(l, i - 1, input);
            vector<int> right = solve(i + 1, r, input);
            for (auto x : left)
                for (auto y : right) {
                    if (input[i] == 0)
                        sum.push_back(x + y);
                    else if (input[i] == 1)
                        sum.push_back(x - y);
                    else
                        sum.push_back(x * y);
                }
        }
        return sum;
    }
    vector<int> diffWaysToCompute(string input) {
        vector<int> cleaned_input;
        int num = 0, n = 0;
        for (auto c : input) {
            if (c >= '0' && c <= '9')
                num = num * 10 + c - '0';
            else {
                cleaned_input.push_back(num);
                num = 0;
                if (c == '+')
                    cleaned_input.push_back(0);
                else if (c == '-')
                    cleaned_input.push_back(1);
                else
                    cleaned_input.push_back(2);
                n += 2;
            }
        }

        cleaned_input.push_back(num);
        n++;

        return solve(0, n - 1, cleaned_input);
    }
};
```
## 242. 有效的字母异位词

![](242.png)

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        vector<int> hash(26, 0);
        for (auto c : s)
            hash[c - 'a']++;
        for (auto c : t)
            hash[c - 'a']--;
        for (int i = 0; i < 26; i++)
            if (hash[i] != 0)
                return false;
        return true;
    }
};
```
## 257. 二叉树的所有路径

![](257.png)

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
    void dfs(TreeNode *cur, string path, vector<string>& ans) {
        if (cur -> left == NULL && cur -> right == NULL) {
            ans.push_back(path);
            return;
        }
        if (cur -> left != NULL)
            dfs(cur -> left, path + "->" + to_string(cur -> left -> val), ans);

        if (cur -> right != NULL)
            dfs(cur -> right, path + "->" + to_string(cur -> right -> val), ans);
    }
    vector<string> binaryTreePaths(TreeNode* root) {
        if (root == NULL)
            return vector<string>{};
        string path = to_string(root -> val);
        vector<string> ans;
        dfs(root, path, ans);
        return ans;
    }
};
```
## 258. 各位相加

![](258.png)

```c++
class Solution {
public:
    int addDigits(int num) {
        while (num >= 10) {
            int tot = 0;
            for (; num > 0; num /= 10)
                tot += num % 10;
            num = tot;
        }
        return num;
    }
};
```
## 260. 只出现一次的数字 III

![](260.png)

```c++
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        int sum = 0;
        for (int i = 0; i < nums.size(); i++)
            sum ^= nums[i];
        int pos = 0;
        for (int i = 0; i < 32; i++)
            if ((sum >> i) & 1) {
                pos = i;
                break;
            }
        int s1 = 0, s2 = 0;
        for (int i = 0; i < nums.size(); i++)
            if ((nums[i] >> pos) & 1)
                s1 ^= nums[i];
            else
                s2 ^= nums[i];

        return vector<int>{s1, s2};
    }
};
```
## 263. 丑数

![](263.png)

```c++
class Solution {
public:
    bool isUgly(int num) {
        if (num <= 0)
            return false;
        while (num % 5 == 0)
            num = num / 5;
        while (num % 3 == 0)
            num = num / 3;
        while (num % 2 == 0)
            num = num / 2;
        return num == 1;
    }
};
```
## 264. 丑数 II

![](264.png)

```c++
class Solution {
public:
    long long nthUglyNumber(int n) {
        if(n <= 0)
            return 0;
        priority_queue<long long>pq;
        pq.push(-1);
        //由于priority queue会把最大的值放在顶端，为了实现最小堆，我把数字取负数放进去，最后取结果时再变为正数即可。
        long long lastnum = 0;//标记前一个数字
        for(int i = 1;i<=n;i++){
            long long top = pq.top();//取最小堆堆顶
            pq.pop();
            if(top == lastnum){
            //注意到不能重复，如果第i个丑数和第i-1个丑数重复了，那么跳过该丑数
                i-=1;
                continue;            
            }
            lastnum =top;
            pq.push(top*2);//将堆顶乘上2、3、5并放入堆中
            pq.push(top*3);
            pq.push(top*5);
        }
        return lastnum*-1;//注意将结果取相反数
    }
};
/*
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> uglyNumbers;
        uglyNumbers.push_back(1);
        int index2 = 0;//2 3 5三个指针
        int index3 = 0;
        int index5 = 0;
        for(int i = 1;i<n;i++){
            int curMaxNum2 = uglyNumbers[index2]*2;//找出2 3 5指针指向的当前最大丑数
            int curMaxNum3 = uglyNumbers[index3]*3;
            int curMaxNum5 = uglyNumbers[index5]*5;
            int uglynum = min(min(curMaxNum2,curMaxNum3),curMaxNum5);//从当前最大丑数中选一个最小的
            if(uglynum==curMaxNum2)//更新指针
                index2++;
            if(uglynum==curMaxNum3)
                index3++;
            if(uglynum==curMaxNum5)
                index5++;
            uglyNumbers.push_back(uglynum);
        }
        return uglyNumbers[n-1];
    }
};
*/
```
## 268. 缺失数字

![](268.png)

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        long long sum = 0;
        int n = nums.size();
        for (auto x : nums)
            sum += x;
        return (int)((long long)(n) * (n + 1) / 2 - sum);
    }
};
```
## 273. 整数转换英文表示

![](273.png)

## 274. H 指数

![](274.png)

```c++
class Solution {
public:
    int hIndex(vector<int>& citations) {
        sort(citations.begin(), citations.end());
        int res = 0;
        for (int i = 0; i < citations.size(); i ++ )
            if (citations.size() - i <= citations[i])
            {
                res = citations.size() - i;
                break;
            }
        return res;
    }
};
```
## 275. H指数 II

![](275.png)

```c++
class Solution {
public:
    int hIndex(vector<int>& citations) {
        if(citations.empty()) return 0;
        int l = 0,r = citations.size();
        while(l < r)
        {
            int mid = l + r + 1>> 1;
            if(citations[citations.size() - mid] >= mid) l = mid;
            else r = mid - 1;
        }
        return r;
    }
};
```
## 278. 第一个错误的版本

![](278.png)

```c++
// Forward declaration of isBadVersion API.
bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        long long l = 0,r = n;
        while(l < r)
        {
            int mid = l + r + 0ll >> 1;
            if(isBadVersion(mid)) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```
## 279. 完全平方数

![](279.png)

```c++
class Solution {
public:
    int numSquares(int n) {
        vector<int> f(n + 1, n);
        f[0] = 0;
        for (int i = 1; i <= n; i++)
            for (int j = 1; j * j <= i; j++)
                f[i] = min(f[i], f[i - j * j] + 1);

        return f[n];
    }
};
/*
class Solution {
public:
    int numSquares(int n) {
        vector<int> f(n + 1, n);
        queue<int> q;
        f[0] = 0;
        q.push(0);
        while (!q.empty()) {
            int s = q.front();
            if (s == n)
                break;
            q.pop();
            for (int i = 1; s + i * i <= n; i++)
                if (f[s + i * i] > f[s] + 1) {
                    f[s + i * i] = f[s] + 1;
                    q.push(s + i * i);
                }
        }
        return f[n];
    }
};
*/
```