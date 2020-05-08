---
typora-root-url: img/
---

## LeetCode 1417. 重新格式化字符串

![](1417.png)

```c++
class Solution {
public:
    string reformat(string s) {
        vector<char> d, c;
        for (char x : s)
            if (isdigit(x))
                d.push_back(x);
            else
                c.push_back(x);

        string ans;
        if (d.size() == c.size() - 1) {
            for (int i = 0; i < d.size(); i++) {
                ans += c[i];
                ans += d[i];
            }
            ans += c.back();
        } else if (c.size() == d.size() - 1) {
            for (int i = 0; i < c.size(); i++) {
                ans += d[i];
                ans += c[i];
            }
            ans += d.back();
        } else if (d.size() == c.size()) {
            for (int i = 0; i < c.size(); i++) {
                ans += d[i];
                ans += c[i];
            }
        }

        return ans;
    }
};
```
## LeetCode 1418. 点菜展示表

![](1418.png)

```c++
class Solution {
public:
    vector<vector<string>> displayTable(vector<vector<string>>& orders) {
        set<string> foods;
        for (const auto &v : orders)
            foods.insert(v[2]);

        int cnt = 1;
        unordered_map<string, int> fp;

        for (const auto &s : foods)
            fp[s] = cnt++;


        vector<vector<int>> res;

        cnt = 0;
        unordered_map<string, int> tp;
        for (const auto &v : orders) {
            if (tp.find(v[1]) == tp.end()) {
                tp[v[1]] = cnt;
                res.push_back(vector<int>(foods.size() + 1));
                res[cnt][0] = stoi(v[1]);
                cnt++;
            }

            res[tp[v[1]]][fp[v[2]]]++;
        }

        sort(res.begin(), res.end());

        vector<vector<string>> ans;

        ans.push_back(vector<string>(1));
        ans[0][0] = "Table";

        for (const auto &v : foods)
            ans[0].push_back(v);

        for (int i = 0; i < res.size(); i++) {
            ans.push_back(vector<string>(foods.size() + 1));
            for (int j = 0; j <= foods.size(); j++)
                ans[i + 1][j] = to_string(res[i][j]);
        }

        return ans;
    }
};
```

## 1419. 数青蛙

![](1419.png)

```c++
class Solution {
public:
    int minNumberOfFrogs(string croakOfFrogs) {
        int n = croakOfFrogs.size();
        vector<int> cnt(4, 0);
        int pending = 0;
        int ans = 0;

        for (char c : croakOfFrogs) {
            if (c == 'c') {
                cnt[0]++;
                pending++;
            } else if (c == 'r') {
                if (cnt[0] == 0)
                    return -1;
                cnt[0]--;
                cnt[1]++;
            } else if (c == 'o') {
                if (cnt[1] == 0)
                    return -1;
                cnt[1]--;
                cnt[2]++;
            } else if (c == 'a') {
                if (cnt[2] == 0)
                    return -1;
                cnt[2]--;
                cnt[3]++;
            } else if (c == 'k') {
                if (cnt[3] == 0)
                    return -1;
                cnt[3]--;
                pending--;
            }
            ans = max(ans, pending);
        }

        if (pending > 0)
            return -1;

        return ans;
    }
};
```

## LeetCode 1420. 生成数组

![](1420.png)
