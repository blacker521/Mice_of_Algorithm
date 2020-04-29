---
typora-root-url: img
---

## 301. 删除无效的括号

![](301.png)

## 303. 区域和检索 - 数组不可变

![](303.png)

```c++
class NumArray {
public:
    vector<int> f;
    NumArray(vector<int>& nums) {
        int n = nums.size();
        f = vector<int>(n + 1,0); //!!!!定义未初始化的vector!!!!
        for(int i = 1;i <= n;i ++)
            f[i] = f[i - 1] + nums[i - 1];
    }
    
    int sumRange(int i, int j) {
        return f[j + 1] - f[i];
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(i,j);
 */
```
## 304. 二维区域和检索 - 矩阵不可变

![](304.png)

```c++
class NumMatrix {
public:
    vector<vector<int>> f;

    NumMatrix(vector<vector<int>> matrix) {
        if (matrix.empty()) return;
        int n = matrix.size(), m = matrix[0].size();
        f = vector<vector<int>>(n + 1, vector<int>(m + 1, 0));
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
                f[i][j] = f[i - 1][j] + matrix[i - 1][j - 1];

        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ )
                f[i][j] += f[i][j - 1];
    }

    int sumRegion(int row1, int col1, int row2, int col2) {
        return f[row2 + 1][col2 + 1] - f[row2 + 1][col1] - f[row1][col2 + 1] + f[row1][col1];
    }
};

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * int param_1 = obj.sumRegion(row1,col1,row2,col2);
 */
```
## 306. 累加数

![](306.png)

## 307. 区域和检索 - 数组可修改

![](307.png)

```c++
class NumArray {
public:
    vector<int> a, f, sum;
    int n;
    NumArray(vector<int> nums) {
        n = nums.size();
        a = nums;
        f = vector<int>(n + 1, 0);
        sum = vector<int>(n + 1, 0);
        for (int i = 1; i <= n; i++)
            sum[i] = sum[i - 1] + nums[i - 1];
    }

    void update(int i, int val) {
        int x = i;
        for (x++; x <= n; x += x & -x)
            f[x] += val - a[i];
        a[i] = val;
    }

    int prefixSum(int x) {
        int res = 0;
        for (; x; x -= x & -x)
            res += f[x];
        return res;
    }

    int sumRange(int i, int j) {
        return prefixSum(j + 1) - prefixSum(i) + sum[j + 1] - sum[i];
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * obj.update(i,val);
 * int param_2 = obj.sumRange(i,j);
 */
```
## 309. 最佳买卖股票时机含冷冻期

![](309.png)

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if (n == 0)
            return 0;

        vector<int> f(n); // 当天不持有
        vector<int> g(n); // 当天持有

        f[0] = 0;
        g[0] = -prices[0];
        for (int i = 1; i < n; i++) {
            f[i] = max(f[i - 1], g[i - 1] + prices[i]);
            if (i >= 2)
                g[i] = max(g[i - 1], f[i - 2] - prices[i]);
            else
                g[i] = max(g[i - 1], -prices[i]);
        }
        return f[n - 1];
    }
};
```
## 310. 最小高度树

![](310.png)

## 313. 超级丑数

![](313.png)

```c++
class Solution {
public:
    int nthSuperUglyNumber(int n, vector<int>& primes) {
        int idx = 1;
        vector<int> res;
        vector<int> index;
        index.resize(primes.size());
        res.push_back(1);

        while(idx<n){
            int minval = INT_MAX;
            int minindex = -1;
            for(int i = 0;i<primes.size();++i){
                if (primes[i]*res[index[i]] < minval){
                    minval = res[index[i]] * primes[i];
                    minindex = i;
                }
            }
            index[minindex]++;
            if (minval == res.back()) continue;
            res.push_back(minval);
            idx = idx + 1;
        }
        return res.back();
    }
};
```
## 315. 计算右侧小于当前元素的个数

![](315.png)

## 316. 去除重复字母

![](316.png)

```c++
class Solution {
public:
    string removeDuplicateLetters(string s) {
        vector<bool> visit(26,false);
        vector<int> cnt;
        cnt.resize(26);

        for (auto c: s){
            cnt[c-'a']++;
        }
        string result = "";
        for(auto c:s){
            cnt[c-'a']--;
            if (visit[c-'a']) continue;
            while(!result.empty()&&result.back()>c&&cnt[result.back()-'a']>0){
                visit[result.back()-'a'] = false;
                result.pop_back();

            }
            result+=c;
            visit[c-'a'] =true;
        }

        return result;

    }
};
```
## 318. 最大单词长度乘积

![](318.png)

```c++
class Solution {
public:
    int maxProduct(vector<string>& words) {
        vector<pair<int, int>> states;
        for (auto word : words)
        {
            unordered_set<char> S;
            for (auto c : word) S.insert(c);
            int state = 0;
            for (int i = 0; i < 26; i ++ )
                if (S.count(i + 'a'))
                    state += 1 << i;
            states.push_back(make_pair(state, word.size()));
        }
        int res = 0;
        for (int i = 0; i < states.size(); i ++ )
            for (int j = 0; j < i; j ++ )
            {
                if (states[i].first & states[j].first) continue;
                res = max(res, states[i].second * states[j].second);
            }
        return res;
    }
};
```
## 319. 灯泡开关

![](319.png)

```c++
class Solution {
public:
    int bulbSwitch(int n) {
        return sqrt(n);
    }
};
```