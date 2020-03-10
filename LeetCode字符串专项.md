---
typora-root-url: img/
---

# 字符串

## LeetCode 38. 外观数列

![](38.png)

```c++
/*模拟题
把上一行连续段读出来

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
***************************************************
    
class Solution {
public:
    string countAndSay(int n) {
        string res = "1";
        for(int i = 0;i < n - 1;i ++)
        {
            string ns;
            for(int j = 0;j < res.size();)
            {
                int k = j;
                while(k < res.size() && res[k] == res[j]) k ++;//找到连续的最大长度
                ns += to_string(k - j) + res[j];
                j = k;//要更新j的大小，跳过相同的字符，找下一个字符！！！
            }
            res = ns;//一直进行循环n次，上一个的res是下一个的ns
        }
        return res
    }
};
```

## LeetCode 49. 字母异位词分组

![](49.png)

![](49_1.png)

```c++
/*
字符串分组，字母相同，顺序可以不同
eat、tea、ate是一组
tan，nat是一组
bat是一组

用排序加hash

*/
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> hash;//第一个string是原来的字符串，第二个string是排完序的关键字。
        for (auto &str : strs)
        {
            string key = str;//备份
            sort(key.begin(), key.end());
            hash[key].push_back(str);
        }

        vector<vector<string>> res;
        for (auto item : hash) res.push_back(item.second);//映射的值。

        return res;
    }
};
```

## LeetCode 151.翻转字符串里的单词 

![](151.png)

![](151_1.png)

```c++
/*
按照单词进行反转
翻转两次
1.按单词翻转。
2.翻转整个字符串。

连续不等于空格的字符串是单词
reverse是左闭右开的！！！
*/
class Solution {
public:
    string reverseWords(string s) {
        int k = 0;
        for (int i = 0; i < s.size();)
        {
            int j = i;
            while (j < s.size() && s[j] == ' ') j ++ ;//连续不等于空格的字符串是单词
            if (j == s.size()) break;
            i = j;
            while (j < s.size() && s[j] != ' ') j ++ ;
            reverse(s.begin() + i, s.begin() + j);//第一次翻转
            if (k) s[k ++ ] = ' ';
            while (i < j) s[k ++ ] = s[i ++ ];
        }
        s.erase(s.begin() + k, s.end());//把k后面多余的部分删掉（多余的空格）
        reverse(s.begin(), s.end());//再翻转一次

        return s;
    }
};
```
## LeetCode 165. 比较版本号

![](165.png)

```c++
/*
先比较第一位，第二位.....
按位比较，位数不够补0
atoi(string.c_str())把字符串变成数字
*/
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
*************************************************************************
class Solution {
public:
    int compareVersion(string s1, string s2) {
        int i = 0,j = 0;
        while(i < s1.size() || j < s2.size())
        {
            int x = i,y = j;
            while(x < s1.size() && s1[x] != '.') x ++;
            while(y < s2.size() && s2[y] != '.') y ++;
            int a = i == x ? 0 : atoi(s1.substr(i, x - i).c_str());//x == y >>>> a = 0 
            int b = j == y ? 0 : atoi(s2.substr(j, y - j).c_str());
            if(a > b) return 1;
            if(a < b) return -1;
            i = x + 1;
            j = y + 1;
        }
        return 0;
    }
};
```

## LeetCode 929.独特的电子邮件地址

![](929.png)

![](929_1.png)

```c++
/*
把email分成用户名和域名，用substr求子串，找到@的位置，然后求子串
把用户名过滤生成新的用户名
再将新用户名和域名合起来组成新的email
用unordered_set存储结果，会自动去重
*/
class Solution {
public:
    int numUniqueEmails(vector<string>& emails) {
        unordered_set<string> S;
        for (auto email : emails)
        {
            string local, domain;
            int k = 0;
            while (email[k] != '@') k ++ ;
            local = email.substr(0, k);
            domain = email.substr(k + 1);
            string newLocal;
            for (auto c : local)
            {
                if (c == '+') break;
                if (c != '.') newLocal += c;
            }
            S.insert(newLocal + '@' + domain);
        }
        return S.size();
    }
};
*******************************************************************
class Solution {
public:
    int numUniqueEmails(vector<string>& emails) {
        unordered_set<string> hash;
        for(auto email : emails)
        {
            int at = email.find('@');
            string name;
            for(auto c : email.substr(0,at))
                if(c == '+') break;
                else if(c != '.') name += c;
            
            string domain = email.substr(at + 1);
            hash.insert(name + '@' + domain);
        }
        return hash.size();
    }
};
```

## LeetCode 5. 最长回文子串

![](5.png)

![](5_1.png)

```c++
/*
回文子串可以用马拉车Manacher算法降低到O(n)

*/
class Solution {
public:
    string longestPalindrome(string s) {
        int res = 0;
        string str;
        for (int i = 0; i < s.size(); i ++ )
        {
            for (int j = 0; i - j >= 0 && i + j < s.size(); j ++ )
                if (s[i - j] == s[i + j])
                {
                    if (j * 2 + 1 > res)
                    {
                        res = j * 2 + 1;
                        str = s.substr(i - j, j * 2 + 1);
                    }
                }
                else break;

            for (int j = i, k = i + 1; j >= 0 && k < s.size(); j -- , k ++ )
                if (s[j] == s[k])
                {
                    if (k - j + 1 > res) 
                    {
                        res = k - j + 1;
                        str = s.substr(j, k - j + 1);
                    }
                }
                else break;
        }
        return str;
    }
};
```

