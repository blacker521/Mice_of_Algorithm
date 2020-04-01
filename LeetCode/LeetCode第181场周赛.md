---
typora-root-url: img/
---

## LeetCode 1389. 按既定顺序创建目标数组

![](1389.png)

手动自己实现

```c++
/*
如果有数就把原来有数的位置空出来
1.把indx[i] 到最后都后移一位
2.插入
*/
class Solution {
public:
    vector<int> createTargetArray(vector<int>& nums, vector<int>& index) {
        vector<int> target;

        for (int i = 0; i < nums.size(); i ++ ) {
            target.push_back(0);//插入一个位置防止数组越界
            for (int j = target.size() - 1; j > index[i]; j -- )
                target[j] = target[j - 1];//后移
            target[index[i]] = nums[i];//插入
        }
    
        return target;
    }
};
```
用insert函数
```c++
class Solution {
public:
    vector<int> createTargetArray(vector<int>& nums, vector<int>& index) {
        vector<int> target;

        for (int i = 0; i < nums.size(); i ++ )
            target.insert(target.begin() + index[i], nums[i]);//insert(插入的位置，数据)
    
        return target;
    }
};
```

## LeetCode 1390. 四因数  

![](1390.png)

```c++
class Solution {
public:
    int sumFourDivisors(vector<int>& nums) {
        int res = 0;
        for (auto x : nums) {
            int sum = 0, cnt = 0;
            for (int i = 1; i * i <= x; i ++ ) {
                if (x % i == 0) {
                    sum += i, cnt ++ ;
                    if (x / i != i) sum += x / i, cnt ++ ;//另外一个约数不一样，就加上！！！
                }
            }

            if (cnt == 4) res += sum;
        }

        return res;
    }
};
```

## LeetCode 1391. 检查网格中是否存在有效路径 

![](1391.png)

![](/1391_1.png)

```c++
/*
上0右1下2右3
1：有道
1: 0 1 0 1
2: 1 0 1 0
3: 0 0 1 1
4: 0 1 1 0
5: 1 0 0 1
6: 1 1 0 0 
判断能不能走过去
1.(x,y)街道的i方向是否为1
2.(x,y)下一个街道i方向的反方向是否为1
0 2是一对反方向---(第二位不一样)
1 3是一对反方向---(第二位不一样)
用 i ^ 2 看第二位
*/
class Solution {
public:
    bool hasValidPath(vector<vector<int>>& grid) {
        return dfs(0, 0, grid);
    }

    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    int state[6][4] = {
        {0, 1, 0, 1},
        {1, 0, 1, 0},
        {0, 0, 1, 1},
        {0, 1, 1, 0},
        {1, 0, 0, 1},
        {1, 1, 0, 0},
    };

    bool dfs(int x, int y, vector<vector<int>>& grid) {
        int n = grid.size(), m = grid[0].size();
        if (x == n - 1 && y == m - 1) return true;

        int k = grid[x][y];
        grid[x][y] = 0;

        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a < 0 || a >= n || b < 0 || b >= m || !grid[a][b]) continue;
            if (!state[k - 1][i] || !state[grid[a][b] - 1][i ^ 2]) continue;

            if (dfs(a, b, grid)) return true;
        }

        return false;
    }
};
```

## LeetCode 1392. 最长快乐前缀

![](1392.png)

```c++
/*
非平凡最长和前缀相同的后缀
KMP
*/
class Solution {
public:
    string longestPrefix(string s) {
        int n = s.size();
        s = ' ' + s;//占位，使下标变成从1开始
        vector<int> next(n + 1);

        for (int i = 2, j = 0; i <= n; i ++ ) {
            while (j && s[i] != s[j + 1]) j = next[j];
            if (s[i] == s[j + 1]) j ++ ;
            next[i] = j;
        }

        return s.substr(1, next[n]);
    }
};
```

