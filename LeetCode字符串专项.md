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
枚举回文串中心点，从中心点向两边扩展，找到最大扩展长度，直到不相同为止
字符串长度为奇数是一个字母
字符串长度为偶数是两个个字母
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
***************************************************************************
class Solution {
public:
    string longestPalindrome(string s) {
        string res;
        for(int i = 0;i < s.size();i ++)
        {
            for(int j = i,k = i;j >= 0 && k < s.size() && s[k] == s[j];j --,k ++)
                if(res.size() < k - j + 1)
                    res = s.substr(j,k - j + 1);
            
            for(int j = i,k = i + 1;j >= 0 && k < s.size() && s[k] == s[j];j --,k ++)
                if(res.size() < k - j + 1)
                    res = s.substr(j,k - j + 1);
        }
        return res;
    }
};
```

## LeetCode 6.  Z 字形变换

![](6.png)

![](6_1.png)

```c++
/*
找规律
第一行首项是0，公差是2(n-1)的等差数列
两个等差数列交错
最后一行，首项是n-1公差是2(n-1)的等差数列
*/
class Solution {
public:
    string convert(string s, int numRows) {
        string res;
        if (numRows == 1) return s;
        for (int j = 0; j < numRows; j ++ )
        {
            if (j == 0 || j == numRows - 1)
            {
                for (int i = j; i < s.size(); i += (numRows-1) * 2)
                    res += s[i];
            }
            else
            {
                for (int k = j, i = numRows * 2 - 1 - j - 1;
                        i < s.size() || k < s.size();
                        i += (numRows - 1) * 2, k += (numRows - 1) * 2)
                {
                    if (k < s.size()) res += s[k];
                    if (i < s.size()) res += s[i];
                }
            }
        }
        return res;
    }
};
**************************************************************************
class Solution {
public:
    string convert(string s, int n) {
        if(n == 1) return s;
        string res;
        for(int i = 0;i < n;i ++)
        {
            if(!i || i == n -1)
            {
                for(int j = i;j < s.size();j += 2 * (n - 1)) res += s[j];
            }
            else
            {
                for(int j = i,k = 2 * (n - 1) - i;j < s.size() || k < s.size();j += 2 *(n - 1),k += 2 * (n - 1))
                {
                    if(j < s.size()) res += s[j];
                    if(k < s.size()) res += s[k];
                }
            }
        }
        return res;
    }
};
```
## LeetCode 3.无重复字符的最长子串

![](3.png)

![](3_1.png)

```c++
/*
最长且不包含重复字符的子串
先想暴力O(n^2)
for(int i = 0;i < s.size();i ++)
	for(int j = i;j >= 0;j --)
		if(check() == false)
			break;
优化
单调性O(n)
后面指针往后走，前面指针一定单调往后走
后面走一下，判断一下前面指针是不是要往后走
用hash存储每个字母出现的次数
i是前一个指针，j是后一个指针
*/
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> hash;
        int res = 0;
        for (int i = 0, j = 0; j < s.size(); j ++ )
        {
            hash[s[j]] ++ ;
            while (hash[s[j]] > 1) hash[s[i ++ ]] -- ;//如果有重复字母，一定是是s[j]!!!
            res = max(res, j - i + 1);
        }
        return res;
    }
};
```
## LeetCode 208.实现 Trie (前缀树)

![](208.png)

![](208_1.png)

```c++
/*
Trie树
*/
class Trie {
public:
    struct Node
    {
        bool is_end;
        Node *son[26];//儿子节点
        Node()
        {
            is_end = false;
            for(int i = 0;i < 26;i ++) son[i] = NULL;
        }
    }*root;
    /** Initialize your data structure here. */
    Trie() {
        root = new Node();
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        auto p = root;
        for(auto c : word)
        {
            int u = c - 'a';//求编号
            if(p->son[u] == NULL) p->son[u] = new Node();//儿子不存在就创建
            p = p->son[u];
        }
        p->is_end = true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        auto p = root;
        for(auto c : word)
        {
            int u = c - 'a';
            if(p->son[u] == NULL) return false;
            p = p->son[u];
        }
        return p->is_end;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        auto p = root;
        for(auto c : prefix)
        {
            int u = c - 'a';
            if(p->son[u] == NULL) return false;
            p = p->son[u];
        }
        return true;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```
## LeetCode 273. 整数转换英文表示

![](273.png)

![](273_1.png)

```c++
class Solution {
public:
    int hundred = 100, thousand = 1000, million = 1000000, billion = 1000000000;
    unordered_map<int, string> numbers;

    string numberToWords(int num) {
        char number20[][30] = {"Zero", "One", "Two", "Three", "Four", "Five", 
                               "Six", "Seven", "Eight", "Nine", "Ten", "Eleven", 
                               "Twelve", "Thirteen", "Fourteen", "Fifteen", 
                               "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
        char number2[][30] = {"Twenty", "Thirty", "Forty", "Fifty", "Sixty", 
                              "Seventy", "Eighty", "Ninety"};
        for (int i = 0; i < 20; i ++ ) numbers[i] = number20[i];
        for (int i = 20, j = 0; i < 100; i += 10, j ++ ) numbers[i] = number2[j];
        numbers[hundred] = "Hundred", numbers[thousand] = "Thousand";
        numbers[million] = "Million", numbers[billion] = "Billion";
        string res;
        for (int k = 1000000000; k >= 100; k /= 1000)
        {
            if (num >= k)
            {
                res += ' ' + get3(num / k) + ' ' + numbers[k];
                num %= k;
            }
        }
        if (num) res += ' ' + get3(num);
        if (res.empty()) res = ' ' + numbers[0];
        return res.substr(1);
    }

    string get3(int num)
    {
        string res;
        if (num >= hundred)
        {
            res += ' ' + numbers[num / hundred] + ' ' + numbers[hundred];
            num %= hundred;
        }
        if (num)
        {
            if (num < 20) res += ' ' + numbers[num];
            else if (num % 10 == 0) res += ' ' + numbers[num];
            else res += ' ' + numbers[num / 10 * 10] + ' ' + numbers[num % 10];
        }
        return res.substr(1);
    }
};
```