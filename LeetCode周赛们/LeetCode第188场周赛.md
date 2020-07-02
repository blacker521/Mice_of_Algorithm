---
typora-root-url: img/
---

## LeetCode 1422. 分割字符串的最大得分

![](1422.png)

```c++
class Solution {
public:
    int maxScore(string s) {
        int n = s.length();
        vector<int> z(n, 0), o(n, 0);
        if (s.front() == '0')
            z[0] = 1;

        for (int i = 1; i < n; i++)
            z[i] = z[i - 1] + int(s[i] == '0');

        if (s.back() == '1')
            o[n - 1] = 1;

        for (int i = n - 2; i >= 0; i--)
            o[i] = o[i + 1] + int(s[i] == '1');

        int ans = 0;
        for (int i = 1; i < n; i++)
            ans = max(ans, z[i - 1] + o[i]);

        return ans;
    }
};
```
## 1423. 可获得的最大点数

![](1423.png)

```c++
class Solution {
public:
    int maxScore(vector<int>& cardPoints, int k) {
        int n = cardPoints.size();

        vector<int> s(n + 1, 0);

        for (int i = 1; i <= n; i++)
            s[i] = s[i - 1] + cardPoints[i - 1];

        int ans = 0;
        for (int i = 0; i <= k; i++)
            ans = max(ans, s[i] + s[n] - s[n - (k - i)]);

        return ans;
    }
};
```

## 1424. 对角线遍历 II

![](1424.png)

```c++
class Solution {
public:
    vector<int> findDiagonalOrder(vector<vector<int>>& nums) {
        int n = nums.size();

        vector<int> pre(n + 1), nxt(n + 1);

        int s = 0;
        for (int i = 0; i <= n; i++) {
            pre[i] = i - 1;
            nxt[i] = i + 1;
        }

        int d = 0;
        vector<int> ans;

        while (s < n) {
            vector<int> cur;
            for (int i = s; i < min(d + 1, n); i = nxt[i]) {
                if (d - i < nums[i].size()) {
                    cur.push_back(nums[i][d - i]);
                    if (d - i == nums[i].size() - 1) {
                        if (pre[i] == -1) s = nxt[i];
                        else nxt[pre[i]] = nxt[i];

                        pre[nxt[i]] = pre[i];
                    }
                }
            }

            ans.insert(ans.end(), cur.rbegin(), cur.rend());
            d++;
        }

        return ans;
    }
};
```

## 1425. 带限制的子序列和

![](1425.png)

```c++
class Solution {
public:
    int constrainedSubsetSum(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> f(n);

        deque<int> q;
        int ans = nums[0];

        f[0] = nums[0];
        q.push_back(0);

        for (int i = 1; i < n; i++) {
            while (!q.empty() && i - q.front() > k)
                q.pop_front();

            f[i] = max(nums[i], f[q.front()] + nums[i]);
            ans = max(ans, f[i]);

            while (!q.empty() && f[i] >= f[q.back()])
                q.pop_back();

            q.push_back(i);
        }
        return ans;
    }
};
```