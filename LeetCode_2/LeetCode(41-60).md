---
typora-root-url: img
---

## 41. 缺失的第一个正数

![](41.png)

```c++
class Solution{
public:
    int firstMissingPositive(vector<int>& nums) 
    {
        int n = nums.size();

        for(int i = 0; i < n; ++ i)
            while(nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i])
                swap(nums[i], nums[nums[i] - 1]);

        for(int i = 0; i < n; ++ i)
            if(nums[i] != i + 1)
                return i + 1;

        return n + 1;
    }
};
```
## 42. 接雨水

![](42.png)

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        int res = 0;
        stack<int> stk;

        for(int i = 0;i < height.size();i ++)
        {
            int last = 0;
            while(stk.size() && height[stk.top()] <= height[i])
            {
                int t = stk.top();
                stk.pop();
                res += (i - t - 1) * (height[t] - last);
                last = height[t];
            }

            if(stk.size()) res += (i - stk.top() - 1) * (height[i] - last);
            stk.push(i);
        }
        return res;
    }
};
```
## 43. 字符串相乘

![](43.png)

```c++
class Solution {
public:
    string multiply(string num1, string num2) {
        int n = num1.length(), m = num2.length();
        vector<int> a(n), b(m), c(n + m);
        for (int i = 0; i < n; i++)
            a[n - i - 1] = num1[i] - '0';
        for (int i = 0; i < m; i++)
            b[m - i - 1] = num2[i] - '0';

        for (int i = 0; i < n; i++)
            for (int j = 0; j < m; j++) {
                c[i + j] += a[i] * b[j];
                c[i + j + 1] += c[i + j] / 10;
                c[i + j] %= 10;
            }

        int l = n + m;
        while (l > 1 && c[l - 1] == 0) l--;
        string ans = "";
        for (int i = l - 1; i >= 0; i--)
            ans += c[i] + '0';
        return ans;
    }
};
```
## 44. 通配符匹配

![](44.png)

```c++
class Solution {
public:
    bool isMatch(string s, string p) {
        int n = s.length(), m = p.length();

        vector<vector<bool>> f(n + 1, vector(m + 1, false));

        f[0][0] = true;

        for (int i = 0; i <= n; i++)
            for (int j = 1; j <= m; j++) {
                char y = p[j - 1];
                if (i > 0) {
                    char x = s[i - 1];
                    if (x == y || y == '?')
                        f[i][j] = f[i][j] | f[i - 1][j - 1];
                }
                if (y == '*') {
                    f[i][j] = f[i][j] | f[i][j - 1];
                    if (i > 0)
                        f[i][j] = f[i][j] | f[i - 1][j];
                }
            }
        return f[n][m];
    }
};
```
## 45. 跳跃游戏 II

![](45.png)

```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(n);
        f[0] = 0;
        int last = 0;
        for (int i = 1; i < n; i++) { // 依次求f[i]的值。
            while (i > last + nums[last]) // 根据i来更新last。
                last++;
            f[i] = f[last] + 1; // 根据f[last]更新f[i]。
        }
        return f[n - 1];
    }
};
```
## 46. 全排列

![](46.png)

```c++
class Solution {
public:
    vector<vector<int>> ans;
    vector<bool> st;
    vector<int> path;

    vector<vector<int>> permute(vector<int>& nums) {

        for (int i = 0; i < nums.size(); i ++ ) st.push_back(false);
        dfs(nums, 0);
        return ans;
    }

    void dfs(vector<int> &nums, int u)
    {
        if (u == nums.size())
        {
            ans.push_back(path);
            return ;
        }

        for (int i = 0; i < nums.size(); i ++ )
            if (!st[i])
            {
                st[i] = true;
                path.push_back(nums[i]);
                dfs(nums, u + 1);
                st[i] = false;
                path.pop_back();
            }
    }
};
```
## 47. 全排列 II

![](47.png)

```c++
class Solution {
public:
    vector<bool> st;
    vector<int> path;
    vector<vector<int>> ans;

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        st = vector<bool>(nums.size(), false);
        path = vector<int>(nums.size());
        dfs(nums, 0, 0);
        return ans;
    }

    void dfs(vector<int>& nums, int u, int start)
    {
        if (u == nums.size())
        {
            ans.push_back(path);
            return;
        }

        for (int i = start; i < nums.size(); i ++ )
            if (!st[i])
            {
                st[i] = true;
                path[i] = nums[u];
                if (u + 1 < nums.size() && nums[u + 1] != nums[u])
                    dfs(nums, u + 1, 0);
                else
                    dfs(nums, u + 1, i + 1);
                st[i] = false;
            }
    }
};
```
## 48. 旋转图像

![](48.png)

```c++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        for(int i = 0;i < n;i ++)
            for(int j = i + 1;j < n;j ++)
                swap(matrix[i][j],matrix[j][i]);
        
        for(int i = 0;i < n;i ++)
            for(int j = 0,k = n - 1;j < k;j ++,k --)
                swap(matrix[i][j],matrix[i][k]);
    }
};
```
## 49. 字母异位词分组

![](49.png)

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string,vector<string>> hash;
        for(auto &str : strs)
        {
            string key = str;
            sort(key.begin(),key.end());
            hash[key].push_back(str);
        }

        vector<vector<string>> res;
        for(auto item : hash) res.push_back(item.second);

        return res;
    }
};
```
## 50. Pow(x, n)

![](50.png)

```c++
class Solution {
public:
    #define LL long long
    double myPow(double x, int n) {
        double ans = 1, p = x;
        LL t = abs((LL)(n)); // 注意 x 为 INT_MIN时，abs 会爆掉 int。
        for (; t; t >>= 1) { // 将 n 进行二进制拆分。
            if (t & 1)
                ans = ans * p; // p 是提前计算好的批量连乘。
            p = p * p; // 每次更新 p，自身平方。
        }
        return n > 0 ? ans : 1 / ans;
    }
};
```
## 51. N皇后

![](51.png)

```c++
class Solution {
public:
    vector<vector<string>> ans;
    vector<string> path;
    vector<bool> row, col, diag, anti_diag;

    vector<vector<string>> solveNQueens(int n) {
        row = col = vector<bool>(n, false);
        diag = anti_diag = vector<bool>(2 * n, false);
        path = vector<string>(n, string(n, '.'));
        dfs(0, 0, 0, n);
        return ans;
    }

    void dfs(int x, int y, int s, int n)
    {
        if (y == n) x ++ , y = 0;
        if (x == n)
        {
            if (s == n) ans.push_back(path);
            return ;
        }

        dfs(x, y + 1, s, n);
        if (!row[x] && !col[y] && !diag[x + y]
                && !anti_diag[n - 1 - x + y])
        {
            row[x] = col[y] = diag[x + y] = anti_diag[n - 1 - x + y] = true;
            path[x][y] = 'Q';
            dfs(x, y + 1, s + 1, n);
            path[x][y] = '.';
            row[x] = col[y] = diag[x + y] = anti_diag[n - 1 - x + y] = false;
        }
    }
};
```
## 52. N皇后 II

![](52.png)

```c++
class Solution {
public:

    int ans = 0,n;
    vector<bool> col,d,ud;
    int totalNQueens(int _n) {
        n = _n;
        col = vector<bool>(n);
        d = ud = vector<bool>(n * 2);
        dfs(0); 

        return ans;
    }
    
    void dfs(int u)
    {
        if(u == n )
        {
            ans ++;
            return;
        }

        for(int i = 0;i < n;i ++)
            if(!col[i] && !d[u + i] && !ud[u - i + n])
            {
                col[i] = d[u + i] = ud[u - i + n] = true;
                dfs(u + 1);
                col[i] = d[u + i] = ud[u - i + n] = false;
            }
    }
};
```
## 53. 最大子序和

![](53.png)

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res = INT_MIN,last = 0;
        for(int i = 0;i < nums.size();i ++)
        {
            int now = max(last,0) + nums[i];
            res = max(res,now);
            last = now;
        }

        return res;
    }
};
```
## 54. 螺旋矩阵

![](54.png)

```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> res;
        int dx[] = {0,-1,0,1},dy[] = {1,0,-1,0};
        int x = 0,y = 0,d = 0;
        int n = matrix.size(),m = matrix[0].size();
        bool st[n][m] = {false};
        for(int i= 1;i <= n * m;i ++)
        {
            int nx = x + dx[d],ny = y + dy[d];
            if(nx < 0 || nx >= n || ny < 0 || ny >= m || st[nx][ny])
            {
                d = (d + 1) % 4;
                nx = x + dx[d];
                ny = y + dy[d];
            }
            res.push_back(matrix[x][y]);
            st[x][y] = true;
            x = nx;
            y = ny;
        }
        return res;
    }
};
```
## 55. 跳跃游戏

![](55.png)

```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int n = nums.size();
        int last = 0;
        for (int i = 1; i < n; i++) {
            while (last < i && i > last + nums[last])
                last++;
            if (last == i)
                return false;
        }
        return true;
    }
};
```
## 56. 合并区间

![](56.png)

```c++
class Solution {
public:
    static bool cmp(const Interval& x, const Interval& y) {
        if (x.start != y.start)
            return x.start < y.start;
        return x.end < y.end;
    }
    vector<Interval> merge(vector<Interval>& intervals) {
        int n = intervals.size();
        sort(intervals.begin(), intervals.end(), cmp);
        vector<Interval> ans;
        if (n == 0)
            return ans;
        Interval cur = intervals[0];
        for (int i = 1; i < n; i++) {
            if (intervals[i].start > cur.end) {
                ans.push_back(cur);
                cur = intervals[i];
            }
            else if (intervals[i].end > cur.end)
                cur.end = intervals[i].end;
        }
        ans.push_back(cur);
        return ans;
    }
};
```
## 57. 插入区间

![](57.png)

## 58. 最后一个单词的长度

![](58.png)

```c++
class Solution {
public:
    int lengthOfLastWord(string s) {
        int sum = 0, res = 0;
        for (int i = 0; i < s.size(); i ++ )
            if (s[i] == ' ')
            {
                if (sum) res = sum;
                sum = 0;
            }
            else
                sum ++ ;
        if (sum) res = sum;
        return res;
    }
};
```
## 59. 螺旋矩阵 II

![](59.png)

```c++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> res(n, vector<int>(n, 0));

        int dx[4] = {-1, 0, 1, 0};
        int dy[4] = {0, 1, 0, -1};

        for (int x = 0, y = 0, i = 0, d = 1; i < n * n; i ++ )
        {
            res[x][y] = i + 1;
            int a = x + dx[d], b = y + dy[d];
            if (a < 0 || a == n || b < 0 || b == n || res[a][b])
            {
                d = (d + 1) % 4;
                a = x + dx[d], b = y + dy[d];
            }
            x = a, y = b;
        }

        return res;
    }
};
```
## 60. 第k个排列

![](60.png)

```c++
class Solution {
public:

    string getPermutation(int n, int k) {
        string res;
        vector<bool> st(n, false);
        for (int i = 0; i < n; i ++ ) //从高位到低位依次枚举每一位
        {
            int f = 1;
            for (int j = 1; j <= n - i - 1; j ++ ) f *= j; //计算 (n-i-1)!
            int next = 0;
            if (k > f) //确定当前位是第几个未使用过的数
            {
                int t = k / f;
                k %= f;
                if (k == 0) k = f, t -- ;
                while (t)
                {
                    if (!st[next]) t -- ;
                    next ++ ;
                }
            }
            while (st[next]) next ++ ;
            res += to_string(next + 1);
            st[next] = true;
        }

        return res;
    }
};
```