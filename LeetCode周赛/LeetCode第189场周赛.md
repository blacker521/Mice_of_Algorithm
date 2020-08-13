---
typora-root-url: img/
---

## LeetCode 1431. 拥有最多糖果的孩子

![](1431.png)

```c++
class Solution {
public:
    vector<bool> kidsWithCandies(vector<int>& candies, int extraCandies) {
        int n = candies.size();

        int ma = candies[0];
        for (int i = 1; i < n; i++)
            ma = max(ma, candies[i]);

        vector<bool> ans(n, false);
        for (int i = 0; i < n; i++)
            if (candies[i] + extraCandies >= ma)
                ans[i] = true;

        return ans;
    }
};
```
## 1432. 改变一个整数能得到的最大差值

![](1432.png)

```c++
class Solution {
public:
    int change(int x, int y, string s) {
        for (char &c : s)
            if (c == x + '0')
                c = y + '0';

        return stoi(s);
    }

    int maxDiff(int num) {
        string s = to_string(num);
        int a = num;

        for (int i = 0; i < s.size(); i++)
            if (s[i] < '9') {
                a = change(s[i] - '0', 9, s);
                break;
            }

        int b = num;
        if (s[0] > '1')
            b = change(s[0] - '0', 1, s);
        else {
            for (int i = 1; i < s.size(); i++)
                if (s[i] > '1') {
                    b = change(s[i] - '0', 0, s);
                    break;
                }
        }

        return a - b;
    }
};
```

## 1433. 检查一个字符串是否可以打破另一个字符串

![](1433.png)

```c++
class Solution {
public:
    void csort(string &s) {
        int n = s.size();
        vector<int> c(26, 0);
        for (int i = 0; i < n; i++)
            c[s[i] - 'a']++;

        for (int i = 25; i >= 0; i--)
            while (c[i]--)
                s[--n] = i + 'a';
    }

    bool checkIfCanBreak(string s1, string s2) {
        csort(s1);
        csort(s2);

        int n = s1.size();
        bool f1 = true, f2 = true;
        for (int i = 0; i < n; i++)
            if (s1[i] < s2[i]) f1 = false;
            else if (s1[i] > s2[i]) f2 = false;

        return f1 || f2;
    }
};
```

## 1434. 每个人戴不同帽子的方案数

![](1434.png)

```c++
class Solution {
public:
    int numberWays(vector<vector<int>>& hats) {
        const int mod = 1000000007;
        int n = hats.size();
        vector<vector<int>> f(41, vector<int>(1 << n, 0));
        vector<vector<bool>> h(n, vector<bool>(41, false));

        for (int i = 0; i < n; i++)
            for (int j : hats[i])
                h[i][j] = true;

        f[0][0] = 1;

        for (int i = 1; i <= 40; i++)
            for (int s = 0; s < (1 << n); s++) {
                f[i][s] = (f[i][s] + f[i - 1][s]) % mod;
                for (int j = 0; j < n; j++)
                    if ((s & (1 << j)) && h[j][i])
                        f[i][s] = (f[i][s] + f[i - 1][s ^ (1 << j)]) % mod;
            }

        return f[40][(1 << n) - 1];
    }
};
```