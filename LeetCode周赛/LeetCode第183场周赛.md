---
typora-root-url: img/
---

## LeetCode 1399. 统计最大组的数目

![](1399.png)

```c++
class Solution {
public:
    int countLargestGroup(int n) {
        vector<int> sum(40, 0);
        for (int i = 1; i <= n; i++) {
            int x = i, y = 0;
            while (x) {
                y += x % 10;
                x /= 10;
            }
            sum[y]++;
        }

        int ma = 0;
        for (int i = 1; i <= 36; i++)
            ma = max(ma, sum[i]);

        int ans = 0;
        for (int i = 1; i <= 36; i++)
            if (ma == sum[i])
                ans++;

        return ans;
    }
};
```
## LeetCode 1400. 构造 K 个回文字符串

![](1400.png)

```c++
class Solution {
public:
    bool canConstruct(string s, int k) {
        if(k > s.size()) return false;

        vector<int> cnt(26,0);
        for(auto c : s)
            cnt[c - 'a'] ++;
        
        int odd = 0;
        for(int i = 0;i < 26;i ++)
            if(cnt[i] & 1)
                odd ++;
        return odd <= k;
    }
};
```

## 1401. 圆和矩形是否有重叠

![](1401.png)

```c++
class Solution {
public:
    double sqr(int x) {
        return x * x;
    }

    bool check(int r, int x0, int y0, int x, int y) {
        return sqr(x0 - x) + sqr(y0 - y) <= sqr(r);
    }

    bool checkOverlap(int radius, int x_center, int y_center, int x1, int y1, int x2, int y2) {
        if (check(radius, x_center, y_center, x1, y1)
         || check(radius, x_center, y_center, x1, y2)
         || check(radius, x_center, y_center, x2, y1)
         || check(radius, x_center, y_center, x2, y2))
            return true;

        if (x1 <= x_center && x_center <= x2) {
            if (y1 <= y_center && y_center <= y2)
                return true;

            if (min(abs(y1 - y_center), abs(y2 - y_center)) <= radius)
                return true;
        }

        if (y1 <= y_center && y_center <= y2) {
            if (min(abs(x1 - x_center), abs(x2 - x_center)) <= radius)
                return true;
        }

        return false;
    }
};
```

## LeetCode 1402. 做菜顺序

![](1402.png)

```c++
class Solution {
public:
    int maxSatisfaction(vector<int>& satisfaction) {
        sort(satisfaction.begin(), satisfaction.end());
        int n = satisfaction.size();

        vector<vector<int>> f(n + 1, vector<int>(n + 1, INT_MIN));
        f[0][0] = 0;

        for (int i = 1; i <= n; i++) {
            f[i][0] = f[i - 1][0];
            for (int j = 1; j <= i; j++)
                f[i][j] = max(f[i - 1][j], f[i - 1][j - 1] + j * satisfaction[i - 1]);
        }

        int ans = INT_MIN;
        for (int j = 0; j <= n; j++)
            ans = max(ans, f[n][j]);

        return ans;
    }
};
```

