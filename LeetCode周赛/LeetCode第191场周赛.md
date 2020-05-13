---
typora-root-url: img/
---

## 1441. 用栈操作构建数组

![](1441.png)

```c++
class Solution {
public:
    vector<string> buildArray(vector<int>& target, int n) {
        vector<string> ans;

        int i = 0, j = 1;
        while (i < target.size()) {
            ans.push_back("Push");
            if (target[i] == j) i++;
            else ans.push_back("Pop");
            j++;
        }

        return ans;
    }
};
```
## 1442. 形成两个异或相等数组的三元组数目

![](1442.png)

```c++
class Solution {
public:
    int countTriplets(vector<int>& arr) {
        int n = arr.size();
        vector<int> s(n + 1);
        s[0] = 0;

        for (int i = 1; i <= n; i++)
            s[i] = s[i - 1] ^ arr[i - 1];

        int ans = 0;
        for (int i = 1; i <= n; i++)
            for (int j = i + 1; j <= n; j++)
                for (int k = j; k <= n; k++)
                    if (s[j - 1] ^ s[i - 1] == s[k] ^ s[j - 1])
                        ans++;

        return ans;
    }
};
```

## 1443. 收集树上所有苹果的最少时间

![](1443.png)

```c++
class Solution {
public:
    vector<vector<int>> tree;

    bool solve(int u, int fa, vector<bool> &hasApple, int &ans) {
        for (int v : tree[u])
            if (v != fa) {
                if (solve(v, u, hasApple, ans)) {
                    hasApple[u] = true;
                    ans += 2;
                }
            }

        return hasApple[u];
    }

    int minTime(int n, vector<vector<int>>& edges, vector<bool>& hasApple) {
        tree.resize(n);

        for (const auto &e : edges) {
            tree[e[0]].push_back(e[1]);
            tree[e[1]].push_back(e[0]);
        }

        int ans = 0;
        solve(0, -1, hasApple, ans);

        return ans;
    }
};
```

## 1444. 切披萨的方案数

![](1444.png)