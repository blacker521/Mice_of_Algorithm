---
typora-root-url: img/
---

# 字符串

## LeetCode 38. 外观数列

![](38.png)

```c++
/*
*/
class Solution {
public:
    string countAndSay(int n) {
        string res = "1";
        while (-- n)
        {
            string newRes = "";
            for (int i = 0; i < res.size();)
            {
                int j = i;
                while (j < res.size() && res[i] == res[j]) j ++ ;
                newRes += to_string(j - i) + res[i];
                i = j;
            }
            res = newRes;
        }
        return res;
    }
};
```

## LeetCode 49. 字母异位词分组

![](49.png)

![](49_1.png)

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> dict;
        for (auto &str : strs)
        {
            string key = str;
            sort(key.begin(), key.end());
            dict[key].push_back(move(str));
        }

        vector<vector<string>> res;
        for (auto i = dict.begin(); i != dict.end(); i ++ )
        {
            res.push_back(move(i -> second));
        }

        return res;
    }
};
```

## LeetCode 151.翻转字符串里的单词 

![](151.png)

![](151_1.png)

```c++
class Solution {
public:
    string reverseWords(string s) {
        int k = 0;
        for (int i = 0; i < s.size();)
        {
            int j = i;
            while (j < s.size() && s[j] == ' ') j ++ ;
            if (j == s.size()) break;
            i = j;
            while (j < s.size() && s[j] != ' ') j ++ ;
            reverse(s.begin() + i, s.begin() + j);
            if (k) s[k ++ ] = ' ';
            while (i < j) s[k ++ ] = s[i ++ ];
        }
        s.erase(s.begin() + k, s.end());
        reverse(s.begin(), s.end());

        return s;
    }
};
```
## LeetCode 165. 比较版本号

![](165.png)

```c++
class Solution {
public:
    int compareVersion(string version1, string version2) {
        int i = 0, j = 0;
        while (i < version1.size() || j < version2.size())
        {
            int ki = i, kj = j;
            while (ki < version1.size() && version1[ki] != '.') ki ++ ;
            while (kj < version2.size() && version2[kj] != '.') kj ++ ;
            string si, sj;
            if (i != ki) si = version1.substr(i, ki - i);
            if (j != kj) sj = version2.substr(j, kj - j);
            if (atoi(si.c_str()) < atoi(sj.c_str())) return -1;
            if (atoi(si.c_str()) > atoi(sj.c_str())) return 1;
            if (ki != i) i = ki + 1;
            if (kj != j) j = kj + 1;
        }
        return 0;
    }
};
```