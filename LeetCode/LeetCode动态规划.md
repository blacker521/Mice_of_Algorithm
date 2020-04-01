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
*****************************************************************************
/*
求出所有子段和的最大值
枚举所有起点、终点

状态表示：f[i]表示所有以i结尾的字段的最大值
状态计算： f[i] = max(f(i - 1),0) + nums[i];
*/
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
******************************************************************
/*
状态表示：f[i,j]   所有从起点走到第i行第j列的路径和的最小值
状态计算：
1.最后一步从左上下来:f[i - 1,j - 1]
2.最后一步从右上下来:f[i - 1,j]
*/
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<vector<long long>> f(n,vector<long long>(n));
        f[0][0] = triangle[0][0];
        for(int i = 1;i < n;i ++)
            for(int j = 0;j <= i; j ++)
            {
                f[i][j] = INT_MAX;
                if(j > 0) f[i][j] = min(f[i][j],f[i - 1][j - 1] + triangle[i][j]);//可以从左边更新
                if(j < i) f[i][j] = min(f[i][j],f[i - 1][j] + triangle[i][j]);//可以从右边更新
            }
        long long res= INT_MAX;
        for(int i = 0;i < n;i ++) res = min(res,f[n - 1][i]);//枚举最后一行的状态
        return res;
    }
};
*********************************************************
/*
二维数组>>>>>>>>>滚动数组
因为f[i]只依赖f[i - 1]这层所以可以用滚动数组
把f[][]前一个空全部 & 1 ！！！就可以变成滚动数组
*/
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<vector<long long>> f(n,vector<long long>(n));
        f[0][0] = triangle[0][0];
        for(int i = 1;i < n;i ++)
            for(int j = 0;j <= i; j ++)
            {
                f[i & 1][j] = INT_MAX;
                if(j > 0) f[i & 1][j] = min(f[i & 1][j],f[i - 1 & 1][j - 1] + triangle[i][j]);//可以从左边更新
                if(j < i) f[i & 1][j] = min(f[i & 1][j],f[i - 1 & 1][j] + triangle[i][j]);//可以从右边更新
            }
        long long res= INT_MAX;
        for(int i = 0;i < n;i ++) res = min(res,f[n - 1 & 1][i]);//枚举最后一行的状态
        return res;
    }
};
```
## LeetCode 63. 不同路径 II

![](63.png)

```c++
/*
类似于62题Unique Paths，每一个网格都可以由该网格左边或上边的网格转移过来，因此到达某一点的路径数等于到达它上一点的路径数与它左边的路径数之和，不同的是，当某个网格有障碍时，到达该网格的路径数维0。这还是一个递推问题，考虑用动态规划。动态规划数组dp[i][j] = 起点到点(i, j)的路径总数。于是我们就得到递推关系式：当网格为0时，dp[i][j] = dp[i][j-1] + dp[i-1][j]；当网格为1（说明该网格是障碍物），dp[i][j]=0。
*/
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
*************************************
/*
状态表示:f[i][j]   从起点到(i,j)的路径
状态计算:
1.最后一步往下走:f[i - 1][j]
2.最后一步往右走:f[i,j - 1]
*/
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& g) {
        int n = g.size(),m = g[0].size();
        vector<vector<long long>> f(n,vector<long long>(m));
        for(int i = 0;i < n;i ++)
            for(int j = 0;j < m;j ++)
            {
                if(g[i][j]) continue;//障碍物
                if(!i && !j) f[i][j] = 1;//左上角
                if(i > 0) f[i][j] += f[i - 1][j];//不是第一行可以从上面过来来
                if(j > 0) f[i][j] += f[i][j - 1];//不是第一列可以从左面过来来
            }
        return f[n - 1][m - 1];
    }
};
```
## LeetCode 91. 解码方法

![](91.png)

![](91_1.png)

```c++
/*
数字变字符串 解码

状态表示:f[i]   所有由前i个数字解码得到的字符串
状态计算:
1.最后一步由最后一个字母s[i]解码    f[i - 1]
2.最后一步由倒数连个字母s[i - 1],s[i]解码    f[i - 2]
*/
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
***************************************************************
class Solution {
public:
    int numDecodings(string s) {
        int n = s.size();
        vector<int> f(n + 1);
        f[0] = 1;//边界
        for(int i = 1;i <= n;i ++)
        {
            if(s[i - 1] != '0') f[i] += f[i - 1];
            if(i >= 2)
            {
                int t = (s[i - 2] - '0') * 10 + s[i - 1] - '0';
                if(t >= 10 && t <= 26) f[i] += f[i - 2];//t是位
            }
        }
        return f[n];
    }
};
```
## LeetCode 198.打家劫舍 !!!

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
****************************************************
/*
状态表示:
f[i]  在前i个数中，所有不选nums[i]的选法的最大值
g[i]  在前i个数中，所有选择nums[i]的选法的最大值
状态计算:
f[i] = max(f[i - 1],g[i -1])
g[i] = f[i - 1] + nums[i - 1]
*/
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(n + 1),g(n + 1);
        for(int i = 1;i <= n;i ++)
        {
            f[i] = max(f[i - 1],g[i - 1]);
            g[i] = f[i - 1] + nums[i - 1];
        }
        return max(f[n],g[n]);
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
*******************************************************************
/*
状态表示:f[i]   所有以i结尾的上升子序列长度的最大值
状态计算：
倒数第二个数是空、倒数第二个数是d第0个数、倒数第二个数是第1个数......
倒数第二个数的是第j个数的子序列的最大值是f[j]
f[j] + 1

nums[j] < nums[i] 才能接到倒数第二个数后面
*/
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(n);

        for(int i = 0;i < n;i ++)
        {
            f[i] = 1;//初始只有自己一个字符
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
## LeetCode 72. 编辑距离

![](72.png)

![](72_1.png)

```c++
/*
状态表示:f[i][j]    所有将第一个字符串的前i个字母变成第二个字符串的前j个字母的方案的最小值
状态计算:
1.添加操作:f[i,j - 1] + 1   加之前第一个字符串的前i个字母变成第二个字符串的前j - 1个字母已经匹配
2.删除操作:f[i - 1,j] + 1   删之前第一个字符串的前i - 1个字母变成第二个字符串的前j个字母已经匹配
3.替换操作:
	f[i - 1][j - 1]    第一个字符串的个第i个字母变和第二个字符串的第j个字母已经相等，不要替换
	f[i - 1][j - 1] + 1     第一个字符串的个第i个字母变和第二个字符串的第j个字母不相等，需要替换
	
	
四个最小值取min
*/
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n = word1.size(), m = word2.size();
        if (!n || !m) return n + m;
        vector<vector<int>>f = 
            vector<vector<int>>(n + 1,vector<int>(m + 1));
        f[0][0] = 0;
        for (int i = 1; i <= n; i ++ ) f[i][0] = i;
        for (int j = 1; j <= m; j ++ ) f[0][j] = j;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
            {
                f[i][j] = f[i - 1][j - 1] + (word1[i - 1] != word2[j - 1]);
                f[i][j] = min(f[i][j], f[i - 1][j] + 1);
                f[i][j] = min(f[i][j], f[i][j - 1] + 1);
            }
        return f[n][m];
    }
};

******************************************************
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
## LeetCode 518. 零钱兑换 II

![](518.png)

![](518_1.png)

```c++
/*
状态表示:f[i][j]    所有由前i种硬币凑出的总钱数等于j的方法的数量

状态计算:第i类物品选0,1,2,3........
0个:f[i - 1][j]
t个:f[i - 1][j - t * coins[i]]

f[i,j] = f[i -1,j] + f[i-1,j-c] + f[i-1,j-2c] +....+f[i-1,j-kc]
f[i,j-c] = f[i-1,j-c] + f[i-1,j-2c]+....+f[i-1,j-kc]

优化成一层循环!!!
f[i,j] = f[i-1,j] + f[i,j-c]
优化成一维!!!
f[j] = f[j] + f[j - c]
*/
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
/*
状态表示：f[L,R]   所有将[L,R]染成最终样子的方式的最小值
状态计算：染到L,L+1,L+2......R
只需L染色:f[L+1,R] + 1
[L,K]染色:f[L,K-1]+f[K+1,R]
*/
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
/*
状态表示:f[i,j]表示s的前i个字母和p的前i个字母是否匹配
	1.p[j] != '*',f[i,j]=f[i-1,j-1] && (s[i]和p[j]匹配)
	2.p[j] == '*',需要枚举*表示多少个字母
		f[i,j] = f[i,j-2] || f[i-1,j] && (s[i]和p[j - 1]匹配)
*/
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