---
typora-root-url: img
---

## 61. 旋转链表

![](61.png)

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
    ListNode* rotateRight(ListNode* head, int k) {
        if(!head) return NULL;

        int n = 0;
        for(auto p = head;p;p = p->next) n ++;

        k %= n;
        auto first = head,second = head;
        while(k --) first = first->next;
        while(first->next)
        {
            first = first->next;
            second = second->next;
        }
        first->next = head;
        head = second->next;
        second->next = NULL;

        return head;
    }
};
```
## 62. 不同路径

![](62.png)

```c++
class Solution {
public:
    int uniquePaths(int m, int n) {  
        int dp[101][101];
        memset(dp, 0, sizeof(dp));
        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                if(i == 1 || j == 1)
                    dp[i][j] = 1;
                else
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m][n];
    }  
};
// class Solution {
// public:
//     int uniquePaths(int m, int n) {  
//         if (m == 1 || n == 1) return 1;  
//         return uniquePaths(m, n - 1) + uniquePaths(m - 1, n);  
//     }  
// };
```
## 63. 不同路径 II

![](63.png)

```c++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& g) {
        int n = g.size(),m = g[0].size();
        vector<vector<long long>> f(n,vector<long long>(m));
        for(int i = 0;i < n;i ++)
            for(int j = 0;j < m;j ++)
            {
                if(g[i][j]) continue;
                if(!i && !j) f[i][j] = 1;
                if(i > 0) f[i][j] += f[i - 1][j];
                if(j > 0) f[i][j] += f[i][j - 1];
            }
        return f[n - 1][m - 1];
    }
};
```
## 64. 最小路径和

![](64.png)

```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int n = grid.size(),m = grid[0].size();
        if(n == 0 || m == 0) return 0;
        vector<vector<int>> dp(n,vector<int>(m,0));
        for(int i = 0;i < n; i++)
        {
            for(int j = 0; j < m;j ++)
            {
                if(i > 0) dp[i][j] = dp[i - 1][j];
                if(j > 0)
                {
                    if(i == 0) dp[i][j] = dp[i][j - 1];
                    else dp[i][j] = min(dp[i][j],dp[i][j - 1]);
                }
                dp[i][j] += grid[i][j];
            }
        }
        return dp[n - 1][m - 1];
    }
};
```
## 65. 有效数字

![](65.png)

```c++
class Solution {
public:
    bool isNumber(string s) {
        int i = 0;
        while (i < s.size() && s[i] == ' ') i ++ ;
        int j = s.size() - 1;
        while (j >= 0 && s[j] == ' ') j -- ;
        if (i > j) return false;
        s = s.substr(i, j - i + 1);

        if (s[0] == '-' || s[0] == '+') s = s.substr(1);
        if (s.empty() || s[0] == '.' && s.size() == 1) return false;

        int dot = 0, e = 0;
        for (int i = 0; i < s.size(); i ++ )
        {
            if (s[i] >= '0' && s[i] <= '9');
            else if (s[i] == '.')
            {
                dot ++ ;
                if (e || dot > 1) return false;
            }
            else if (s[i] == 'e' || s[i] == 'E')
            {
                e ++ ;
                if (i + 1 == s.size() || !i || e > 1 || i == 1 && s[0] == '.') return false;
                if (s[i + 1] == '+' || s[i + 1] == '-')
                {
                    if (i + 2 == s.size()) return false;
                    i ++ ;
                }
            }
            else return false;
        }
        return true;
    }
};
```
## 66. 加一

![](66.png)

```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int t = 1;
        for (int i = digits.size() - 1; i >= 0; i -- )
        {
            digits[i] += t;
            t = digits[i] / 10;
            digits[i] %= 10;
        }
        if (t)
        {
            digits.push_back(0);
            for (int i = digits.size() - 2; i >= 0; i -- )
                digits[i + 1] = digits[i];
            digits[0] = 1;
        }
        return digits;
    }
};
```
## 67. 二进制求和

![](67.png)

```c++
class Solution {
public:
    string addBinary(string a, string b) {
        if(a.size() < b.size()) swap(a,b);
        reverse(a.begin(),a.end());
        reverse(b.begin(),b.end());
        int t = 0;
        string res;
        int k = 0;
        while(k < b.size())
        {
            t += a[k] - '0' + b[k] - '0';
            res += to_string(t & 1);
            t /= 2;
            k ++;
        }
        while(k < a.size())
        {
            t += a[k] - '0';
            res += to_string(t & 1);
            t /= 2;
            k ++;
        }
        if(t) res += '1';
        reverse(res.begin(),res.end());
        return res;
    }
};
```
## 68. 文本左右对齐

![](68.png)

```c++
class Solution {
public:
    string space(int x)
    {
        string res;
        while (x -- ) res += ' ';
        return res;
    }

    vector<string> fullJustify(vector<string>& words, int maxWidth) {
        vector<string> res;
        for (int i = 0; i < words.size();)
        {
            int j = i + 1, s = words[i].size(), rs = words[i].size();
            while (j < words.size() && s + 1 + words[j].size() <= maxWidth)
            {
                s += 1 + words[j].size();
                rs += words[j].size();
                j ++ ;
            }
            rs = maxWidth - rs;
            string line = words[i];
            if (j == words.size())
            {
                for (i ++; i < j; i ++ )
                    line += ' ' + words[i];
                while (line.size() < maxWidth) line += ' ';
            }
            else if (j - i == 1) line += space(rs);
            else
            {
                int base = rs / (j - i - 1);
                int rem = rs % (j - i - 1);
                i ++ ;
                for (int k = 0; i < j; i ++, k ++ )
                    line += space(base + (k < rem)) + words[i];
            }
            i = j;
            res.push_back(line);
        }
        return res;
    }
};
```
## 69. x 的平方根

![](69.png)

```c++
class Solution {
public:
    int mySqrt(int x) {
        int l = 0,r = x;
        while(l < r)
        {
            int mid = l + r + 1ll >> 1;
            if(mid <= x / mid) l = mid;
            else r = mid - 1;
        }
        return r;
    }
};
```
## 70. 爬楼梯

![](70.png)

```c++
class Solution {
public:
    int climbStairs(int n) {
       int ans[100] = {0} ;
       ans[0] = 1;
       ans[1] = 1;
       for(int i = 2; i <= n; i++)
       {
           ans[i] = ans[i-1] + ans[i-2];
       }
       return ans[n]; 
    }
};
```
## 71. 简化路径

![](71.png)

```c++
class Solution {
public:
    string simplifyPath(string path) {
        if (path.back() != '/') path += '/';
        string res, s;
        for (auto &c : path)
        {
            if (res.empty()) res += c;
            else if (c == '/')
            {
                if (s == "..")
                {
                    if (res.size() > 1)
                    {
                        res.pop_back();
                        while (res.back() != '/') res.pop_back();
                    }
                }
                else if (s != "" && s != ".")
                {
                    res += s + '/';
                }
                s = "";
            }
            else
            {
                s += c;
            }
        }
        if (res.size() > 1) res.pop_back();
        return res;
    }
};
```
## 72. 编辑距离

![](72.png)

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n = word1.size(),m = word2.size();
        vector<vector<int>> f(n + 1,vector<int>(m + 1));
        for(int i = 0;i <= n;i ++) f[i][0] = i;//边界情况，把第一个字符串变成0，要进行i次删除操作
        for(int i = 0;i <= m;i ++) f[0][i] = i;//边界情况，把第二个字符串变成0，要进行i次删除操作
        
        for(int i = 1;i <= n;i ++)
        {
            for(int j = 1;j <= m;j ++)
            {
                f[i][j] = min(f[i - 1][j],f[i][j - 1]) + 1;
                f[i][j] = min(f[i][j],f[i -1][j - 1] + (word1[i - 1] != word2[j - 1]));
            }
        }
        return f[n][m];
    }
};
```
## 73. 矩阵置零

![](73.png)

```c++
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        if (matrix.empty()) return;
        int n = matrix.size(), m = matrix[0].size();
        int col0 = 1, row0 = 1;
        for (int i = 0; i < n; i ++ )
            if (!matrix[i][0]) col0 = 0;
        for (int i = 0; i < m; i ++ )
            if (!matrix[0][i]) row0 = 0;
        for (int i = 1; i < n; i ++ )
            for (int j = 1; j < m; j ++ )
                if (!matrix[i][j])
                {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
        for (int i = 1; i < n; i ++ )
            if (!matrix[i][0])
                for (int j = 1; j < m; j ++ )
                    matrix[i][j] = 0;

        for (int i = 1; i < m; i ++ )
            if (!matrix[0][i])
                for (int j = 1; j < n; j ++ )
                    matrix[j][i] = 0;

        if (!col0)
            for (int i = 0; i < n; i ++ )
                matrix[i][0] = 0;

        if (!row0)
            for (int i = 0; i < m; i ++ )
                matrix[0][i] = 0;
    }
};
```
## 74. 搜索二维矩阵

![](74.png)

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.empty() || matrix[0].empty()) return false;

        int n = matrix.size(),m = matrix[0].size();
        int l = 0,r = n * m - 1;
        while(l < r)
        {
            int mid = l + r >> 1;
            if(matrix[mid / m][mid % m] >= target) r = mid;
            else l = mid + 1;
        }
        if(matrix[l/ m][l % m] != target) return false;
        return true;
    }
};
```
## 75. 颜色分类

![](75.png)

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int red = 0, blue = nums.size() - 1;
        for(int i = 0; i <= blue;)
        {
            if(nums[i] == 0) swap(nums[i ++],nums[red ++]);
            else if(nums[i] == 2) swap(nums[i],nums[blue --]);
            else i ++;
        }
    }
};
```
## 76. 最小覆盖子串

![](76.png)

```c++
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char,int> hash;

        for(auto c : t) hash[c] ++;
        int cnt = hash.size();

        string res;
        for(int i = 0,j = 0,c = 0;i < s.size();i ++)
        {
            if(hash[s[i]] == 1) c ++;
            hash[s[i]] --;
            while(hash[s[j]] < 0) hash[s[j ++ ]] ++;
            if(c == cnt)
            {
                if(res.empty() || res.size() > i - j + 1) res = s.substr(j,i - j + 1);
            }
        }
        return res;
    }
};
```
## 77. 组合

![](77.png)

```c++
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> combine(int n, int k) {
        dfs(0, 1, n, k);
        return ans;
    }

    void dfs(int u, int start, int n, int k)
    {
        if (u == k)
        {
            ans.push_back(path);
            return ;
        }

        for (int i = start; i <= n; i ++ )
        {
            path.push_back(i);
            dfs(u + 1, i + 1, n, k);
            path.pop_back();
        }
    }
};
```
## 78. 子集

![](78.png)

```c++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> res;
        int n = nums.size();
        for(int i = 0;i < (1 << n);i ++)
        {
            vector<int> temp;
            for(int j = 0;j < n;j ++)
                if(i >> j & 1)
                    temp.push_back(nums[j]);
            res.push_back(temp);
        }
        return res;
    }
};
```
## 79. 单词搜索

![](79.png)

```c++
class Solution {
public:
    int n,m;
    int dx[4] = {-1,0,1,0},dy[4] = {0,1,0,-1};
    bool exist(vector<vector<char>>& board, string word) {
        if(board.empty() || board[0].empty()) return false;
        n = board.size(),m = board[0].size();

        for(int i = 0;i < n;i ++)   
            for(int j = 0;j < m;j ++)
                if(dfs(board,i,j,word,0))
                    return true;
        return false;
    }

    bool dfs(vector<vector<char>> &board,int x,int y,string &word,int u)
    {
        if(board[x][y] != word[u]) return false;
        if(u == word.size() - 1) return true;

        board[x][y] = '.';
        for(int i = 0;i < 4;i ++)
        {
            int a = x + dx[i],b = y + dy[i];
            if(a >= 0 && a < n && b >= 0 && b < m)
                if(dfs(board,a,b,word,u + 1))
                    return true;
        }
        board[x][y] = word[u];
        return false;   
    }
};
```
## 80. 删除排序数组中的重复项 II

![](80.png)

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size() < 3) return nums.size();
        int k = 1;
        for(int i = 2;i < nums.size();i ++)
            if(nums[i] != nums[k - 1])
                nums[ ++ k] = nums[i];
        k ++;
        return k;
    }
};
```