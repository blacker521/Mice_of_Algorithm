---
typora-root-url: img/
---

## LeetCode 1408. 数组中的字符串匹配

![](1408.png)

```c++
class Solution {
public:
    bool check(const vector<string> &words, int i) {
        int n = words.size();
        for (int j = 0; j < n; j++)
            if (j != i)
                if (words[j].find(words[i]) != string::npos)
                    return true;

        return false;
    }

    vector<string> stringMatching(vector<string>& words) {
        int n = words.size();
        vector<string> ans;
        for (int i = 0; i < n; i++)
            if (check(words, i))
                ans.push_back(words[i]);

        return ans;
    }
};
```
## LeetCode 1409. 查询带键的排列

![](1409.png)

```c++
class Solution {
public:
    vector<int> processQueries(vector<int>& queries, int m) {
        vector<int> p(m);
        for (int i = 0; i < m; i++)
            p[i] = i + 1;

        vector<int> ans;
        for (int q : queries) {
            for (int i = 0; i < m; i++)
                if (q == p[i]) {
                    ans.push_back(i);
                    int x = p[i];
                    for (int j = i - 1; j >= 0; j--)
                        p[j + 1] = p[j];
                    p[0] = x;
                    break;
                }
        }

        return ans;
    }
};
```

## 1410. HTML 实体解析器

![](1410.png)

```c++
class Solution {
public:
    bool check(const string &s, int l, const string &t) {
        if (l < t.length())
            return false;

        for (int i = l - 1, j = t.length() - 1; j >= 0; i--, j--)
            if (s[i] != t[j])
                return false;

        return true;
    }

    string entityParser(string text) {
        vector<pair<string, char>> p;
        p.emplace_back("&quot;", '"');
        p.emplace_back("&apos;", '\'');
        p.emplace_back("&amp;", '&');
        p.emplace_back("&gt;", '>');
        p.emplace_back("&lt;", '<');
        p.emplace_back("&frasl;", '/');

        string s(text.size(), ' ');
        int l = 0;

        for (char c : text) {
            s[l++] = c;
            if (c == ';') {
                for (const auto &t : p)
                    if (check(s, l, t.first)) {
                        l -= t.first.length();
                        s[l++] = t.second;
                        break;
                    }
            }
        }

        return s.substr(0, l);
    }
};
```

## 1411. 给 N x 3 网格图涂色的方案数

![](1411.png)

