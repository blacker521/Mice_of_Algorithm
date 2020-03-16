---
typora-root-url: img/
---

## LeetCode 53. 最大子序和

![](53.png)

![](53_1.png)

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size(), ans;
        vector<int> f(n);
        f[0] = nums[0];
        ans = f[0];
        for (int i = 1; i < n; i++) {
            f[i] = max(f[i - 1] + nums[i], nums[i]);
            ans = max(ans, f[i]);
        }
        return ans;
    }
};
```
![](53_2.png)

```c++
class Solution {
public:
    int calc(int l, int r, vector<int>& nums) {
        if (l == r)
            return nums[l];
        int mid = (l + r) >> 1;
        int lmax = nums[mid], lsum = 0, rmax = nums[mid + 1], rsum = 0;
        for (int i = mid; i >= l; i--) {
            lsum += nums[i];
            lmax = max(lmax, lsum);
        }
        for (int i = mid + 1; i <= r; i++) {
            rsum += nums[i];
            rmax = max(rmax, rsum);
        }
        return max(max(calc(l, mid, nums), calc(mid + 1, r, nums)), lmax + rmax);
    }

    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        return calc(0, n - 1, nums);
    }
};
```
## LeetCode 120. 三角形最小路径和

![](120.png)

![](120_1.png)

```c++
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        for (int i = triangle.size()-2 ; i>=0 ; --i){ //从倒数第二行开始往上
            for (int j = 0 ; j < i+1; ++j){ 
                triangle[i][j] += min(triangle[i+1][j], triangle[i+1][j+1]) ;
            }
        }
        return triangle[0][0];
    }
};
```
## LeetCode 63. 不同路径 II

![](63.png)

![](63_1.png)

```c++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        if(m == 0)return 0;
        int n = obstacleGrid[0].size();
        if(n == 0)return 0;
        int dp[101][101];
        memset(dp, 0, sizeof(dp));
        dp[0][0] = 1 - obstacleGrid[0][0];
        for(int i = 0; i < m; i++){
            for(int j = 0; j< n; j++){
                if(obstacleGrid[i][j] == 1)
                    dp[i][j] = 0;
                else{
                    if(i > 0)
                        dp[i][j] += dp[i-1][j];
                    if(j > 0)
                        dp[i][j] += dp[i][j-1];
                }
            }
        }
        return dp[m-1][n-1];
    }
};
```
## LeetCode 91. 解码方法

![](91.png)

![](91_1.png)

```c++
class Solution {
public:
    int numDecodings(string s) {
        if (s.empty()) return 0;
        int n = s.size();
        vector<int> f(n + 1);
        f[0] = 1;
        for (int i = 1; i <= n; i ++ )
        {
            if (s[i - 1] < '0' || s[i - 1] > '9')
                return 0;
            f[i] = 0;
            if (s[i - 1] != '0') f[i] = f[i - 1];
            if (i > 1)
            {
                int t = (s[i-2]-'0')*10+s[i-1]-'0';
                if (t >= 10 && t <= 26)
                    f[i] += f[i - 2];
            }
        }
        return f[n];
    }
};
```
## LeetCode 198.

![](198.png)

![](198_1.png)

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if (n == 0)
            return 0;

        int f = nums[0], g = 0;

        for (int i = 1; i < n; i++) {
            int last_f = f, last_g = g;
            f = g + nums[i];
            g = max(last_f, last_g);
        }

        return max(f, g);
    }
};
```
## LeetCode 300.最长上升子序列

![](300.png)

![](300_1.png)

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if(nums.size()==0)
            return 0;
        vector<int> dp(nums.size(),1);
        int res = 1;
        for(int i= 1;i<nums.size();i++){
            for(int j = 0;j<i;j++){
                if(nums[i]>nums[j])
                    dp[i] = max(dp[i],dp[j]+1);
            }
            if(dp[i]>res)
                res = dp[i];
        }
        return res;
    }
};
```
![](300_2.png)
```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if(nums.size()==0)
            return 0;
        vector<int> help(nums.size()+1,0);
        help[1] = nums[0];
        int maxlen = 1;
        for(int i= 1;i<nums.size();i++){
            int left = 1;
            int right = maxlen;
            while(left<=right){//二分查找
                int mid = (left+right)/2;
                if(help[mid]<nums[i])
                    left = mid+1;
                else
                    right = mid-1;
            }
            help[left] = nums[i];//维护help数组
            if(left>maxlen)//left值是nums[i]的子序列长度
                maxlen=left;
        }
        return maxlen;
    }
};
```
## LeetCode 72.

![](72.png)

![](72_1.png)

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n = word1.size(), m = word2.size();
        if (!n || !m) return n + m;
        vector<vector<int>>f = 
            vector<vector<int>>(n + 1, 
            vector<int>(m + 1));
        f[0][0] = 0;
        for (int i = 1; i <= n; i ++ ) f[i][0] = i;
        for (int j = 1; j <= m; j ++ ) f[0][j] = j;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
            {
                f[i][j] = f[i - 1][j - 1] + 
                    (word1[i - 1] != word2[j - 1]);
                f[i][j] = min(f[i][j], f[i - 1][j] + 1);
                f[i][j] = min(f[i][j], f[i][j - 1] + 1);
            }
        return f[n][m];
    }
};
```
## LeetCode 518. 零钱兑换 II

![](518.png)

![](518_1.png)

```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        int n = coins.size();
        vector<int> f(amount + 1, 0);
        f[0] = 1;

        for (int i = 0; i < n; i++)
            for (int j = coins[i]; j <= amount; j++)
                f[j] += f[j - coins[i]];

        return f[amount];
    }
};
```
## LeetCode 664.奇怪的打印机

![](664.png)

![](664_1.png)

```c++
class Solution {
public:
    int strangePrinter(string s) {
        int n = s.length();
        if (n == 0)
            return 0;
        vector<vector<int>> f(n + 1, vector<int>(n + 1));
        for (int i = 0; i <= n; i++) {
            f[i][i] = 1;
            if (i > 0)
                f[i][i - 1] = 0;
        }

        for (int len = 2; len <= n; len++)
            for (int i = 0; i < n - len + 1; i++) {
                int j = i + len - 1;
                f[i][j] = 1 + f[i + 1][j];
                for (int k = i + 1; k <= j; k++)
                    if (s[i] == s[k])
                        f[i][j] = min(f[i][j], f[i][k - 1] + f[k + 1][j]);
            }

        return f[0][n - 1];
    }
};
```

## LeetCode 10. 正则表达式匹配

![](10.png)

![](10_1.png)

```c++
class Solution {
public:
    vector<vector<int>>f;
    int n, m;
    bool isMatch(string s, string p) {
        n = s.size();
        m = p.size();
        f = vector<vector<int>>(n + 1, vector<int>(m + 1, -1));
        return dp(0, 0, s, p);
    }

    bool dp(int x, int y, string &s, string &p)
    {
        if (f[x][y] != -1) return f[x][y];
        if (y == m)
            return f[x][y] = x == n;
        bool first_match = x < n && (s[x] == p[y] || p[y] == '.');
        bool ans;
        if (y + 1 < m && p[y + 1] == '*')
        {
            ans = dp(x, y + 2, s, p) || first_match && dp(x + 1, y, s, p);
        }
        else
            ans = first_match && dp(x + 1, y + 1, s, p);
        return f[x][y] = ans;
    }
};
```