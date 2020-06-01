---
typora-root-url: img
---

## 181. 超过经理收入的员工

![](181.png)

```mysql
SELECT
    *
FROM
    Employee AS a,
    Employee AS b
WHERE
    a.ManagerId = b.Id
        AND a.Salary > b.Salary
;
```
## 182. 查找重复的电子邮箱

![](182.png)

```mysql
select Email from
(
  select Email, count(Email) as num
  from Person
  group by Email
) as statistic
where num > 1
;
```
## 183. 从不订购的客户

![](183.png)

```mysql
select customers.name as 'Customers'
from customers
where customers.id not in
(
    select customerid from orders
);
```
## 184. 部门工资最高的员工

![](184.png)

```mysql
SELECT
    Department.name AS 'Department',
    Employee.name AS 'Employee',
    Salary
FROM
    Employee
        JOIN
    Department ON Employee.DepartmentId = Department.Id
WHERE
    (Employee.DepartmentId , Salary) IN
    (   SELECT
            DepartmentId, MAX(Salary)
        FROM
            Employee
        GROUP BY DepartmentId
	)
;
```
## 185. 部门工资前三高的所有员工

![](185.png)

## 187. 重复的DNA序列

![](187.png)

```c++
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
## 188. 买卖股票的最佳时机 IV

![](188.png)

```c++
class Solution {
public:
    int maxProfitII(vector<int>& prices) {
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
    int maxProfit(int k, vector<int>& prices) {
        int n = prices.size();
        if (k >= n / 2)
            return maxProfitII(prices);

        vector<vector<int>> f(2, vector<int>(k + 1, -1000000000));
        vector<vector<int>> g(2, vector<int>(k + 1, -1000000000));
        f[0][0] = 0;
        for (int i = 1; i <= n; i++)
            for (int j = 0; j <= k; j++) {
                f[i & 1][j] = f[i - 1 & 1][j];
                g[i & 1][j] = max(g[i - 1 & 1][j], f[i - 1 & 1][j] - prices[i - 1]);
                if (j > 0)
                    f[i & 1][j] = max(f[i & 1][j], g[i - 1 & 1][j - 1] + prices[i - 1]);
            }
        int ans = 0;
        for (int i = 0; i <= k; i++)
            ans = max(ans, f[n & 1][i]);
        return ans;
    }
};
```
## 189. 旋转数组

![](189.png)

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k %= nums.size();
        reverse(nums.begin(), nums.end());
        reverse(nums.begin(), nums.begin() + k);
        reverse(nums.begin() + k, nums.end());
    }
};
```
## 190. 颠倒二进制位

![](190.png)

```c++
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        uint32_t res = 0;
        for (int i = 0; i < 32; i ++ )
            res = (res << 1) + (n >> i & 1);
        return res;
    }
};
```
## 191. 位1的个数

![](191.png)

```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int res = 0;
        while (n)
        {
            res += n & 1;
            n >>= 1;
        }
        return res;
    }
};
```
## 198. 打家劫舍

![](198.png)

```c++
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
## 199. 二叉树的右视图

![](199.png)

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
    vector<int> rightSideView(TreeNode* root) {
        vector<int>res;
        if(!root)
            return res;
        vector<TreeNode*>Nodes;//记录按层遍历的节点的数组
        int l = 0, r = 0;
        Nodes.push_back(root);
        while(l<=r){//l r维护每一层的开始和结尾节点的位置
            res.push_back(Nodes[l]->val);//返回每层的第一个节点
            int cnt = 0;
            for(int i = l;i<=r;i++){
                if(Nodes[i]->right){//优先访问右边节点
                    Nodes.push_back(Nodes[i]->right);
                    cnt++;
                }
                if(Nodes[i]->left){
                    Nodes.push_back(Nodes[i]->left);
                    cnt++;
                }
            }
            l = r+1;
            r = l+cnt-1;
        }
        return res;
    }
};
```
## 200. 岛屿数量

![](200.png)

```c++
class Solution {
public:
    int dx[4] = {0, 1, 0, -1};
    int dy[4] = {-1, 0, 1, 0};
    void dfs(int x, int y, vector<vector<char>>& grid) {
        grid[x][y] = '0';
        for (int i = 0; i < 4; i++) {
            int tx = x + dx[i], ty = y + dy[i];
            if (tx >= 0 && tx < grid.size() && ty >= 0 && ty < grid[0].size() && grid[tx][ty] == '1')
                dfs(tx, ty, grid);
        }
    }
    int numIslands(vector<vector<char>>& grid) {
        int n = grid.size();
        if (n == 0)
            return 0;
        int m = grid[0].size();
        int ans = 0;
        for (int i = 0; i < n; i++)
            for (int j = 0; j < m; j++)
                if (grid[i][j] == '1') {
                    ans++;
                    dfs(i, j, grid);
                }
        return ans;
    }
};
```
