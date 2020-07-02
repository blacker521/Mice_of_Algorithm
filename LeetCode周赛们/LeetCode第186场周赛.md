---
typora-root-url: img/
---

## LeetCode 1413. 逐步求和得到正数的最小值

![](1413.png)

```c++
class Solution {
public:
    int minStartValue(vector<int>& nums) {
        int s = 0, m = 1;
        for (int x : nums) {
            s -= x;
            m = max(m, s + 1);
        }

        return m;
    }
};
```
## 1414. 和为 K 的最少斐波那契数字数目

![](1414.png)

```c++
class Solution {
public:
    int findMinFibonacciNumbers(int k) {
        int x = 1, y = 1;
        while (x + y <= k) {
            int t = x + y;
            x = y; y = t;
        }

        int ans = 0;
        while (k > 0) {
            if (k >= y) {
                k -= y;
                ans++;
            }
            int t = y - x;
            y = x; x = t;
        }

        return ans;
    }
};
```

## 1415. 长度为 n 的开心字符串中字典序第 k 小的字符串

![](1415.png)

```c++
class Solution {
public:
    bool solve(int x, string &cur, int &k) {
        if (x == 0) {
            k--;
            if (k == 0) return true;
            return false;
        }

        for (char c = 'a' ; c <= 'c'; c++)
            if (cur.size() == 0 || cur.back() != c) {
                cur += c;
                if (solve(x - 1, cur, k))
                    return true;
                cur.pop_back();
            }

        return false;
    }

    string getHappyString(int n, int k) {
        string cur;
        if (solve(n, cur, k))
            return cur;
        return "";
    }
};
```

## LeetCode 1416. 恢复数组

![](1416.png)

```c++
class Solution {
public:
    #define LL long long
    int numberOfArrays(string s, int k) {
        const int mod = 1000000007;
        int n = s.size();
        vector<int> f(n + 1, 0);
        f[0] = 1;

        for (int i = 1; i <= n; i++) {
            LL cur = 0, p = 1;
            for (int j = i - 1; j >= 0 && p <= k; j--, p *= 10) {
                cur += (s[j] - '0') * p;
                if (cur <= k && s[j] != '0')
                    f[i] = (f[i] + f[j]) % mod;
            }
        }

        return f[n];
    }
};
```

