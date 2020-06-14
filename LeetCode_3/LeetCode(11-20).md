---
typora-root-url: img
---

## 11. 盛最多水的容器

![](/11.png)

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int res = 0;
        for (int i = 0, j = height.size() - 1; i < j; )
        {
            res = max(res, 
                min(height[i], height[j]) * (j - i));
            if (height[i] > height[j]) j -- ;
            else i ++ ;
        }
        return res;
    }
};
```



## 12. 整数转罗马数字

![](/12.png)

```c++
class Solution {
public:
    string intToRoman(int num) {
        int values[] = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        string reps[] = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

        string res;
        for (int i = 0; i < 13; i ++ )
            while(num >= values[i])
            {
                num -= values[i];
                res += reps[i];
            }
        return res;
    }
};
```
## 13. 罗马数字转整数

![](/13.png)

```c++
class Solution {
public:
    int romanToInt(string s) {
        int n = s.length(), ans = 0;
        unordered_map<char, int> words;
        words['I'] = 1; words['V'] = 5;
        words['X'] = 10; words['L'] = 50; 
        words['C'] = 100; words['D'] = 500;
        words['M'] = 1000;
        for (int i = 0; i < n; i++) {
            if (i != n - 1 && words[s[i + 1]] > words[s[i]]) {
                ans += words[s[i + 1]] - words[s[i]];
                i++;
            }
            else
                ans += words[s[i]];
        }
        return ans;
    }
};
```
## 14. 最长公共前缀

![](/14.png)

```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& s) {
        string res;
        for(int i = 0 ;i < s.size();i ++)
        {
            for(int j = 0;j < s[0].size();j ++)
            {
                if(s[i][j] != s[i + 1][j] != s[i + 2][j])
                {
                    res = "";
                    break;
                }
                else res =  s[0].substr(0,j);
            }
        }
        return res;
    }
};
/*
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string res;
        if(strs.empty()) return res;

        for(int i = 0;;i ++)
        {
            if(i >= strs[0].size()) return res;
            char c = strs[0][i];
            for(auto & str : strs)
                if(str.size() <= i || str[i] != c) return res;
            res += c;
        }
        return res;
    }
}; */
```
## 15. 三数之和

![](/15.png)

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        for (int st = 0; st < nums.size(); st++) {
            while (st != 0  && st < nums.size() && nums[st] == nums[st - 1])
                st++;
            int l = st + 1, r = nums.size() - 1;
            while (l < r) {
                if (nums[st] + nums[l] + nums[r] == 0) {
                    res.push_back({nums[st], nums[l], nums[r]});
                    do l++;while (l < r && nums[l - 1] == nums[l]);
                    do r--;while (l < r && nums[r] == nums[r + 1]);
                }
                else if (nums[st] + nums[l] + nums[r] < 0) {
                    do l++;while (l < r && nums[l - 1] == nums[l]);
                }
                else {
                    do r--;while (l < r && nums[r] == nums[r + 1]);
                }
            }
        }
        return res;
    }
};
```
## 16. 最接近的三数之和

![](/16.png)

```c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        int ans = nums[0] + nums[1] + nums[2];
        for (int i = 0; i < nums.size(); i++)
            for (int j = i + 1; j < nums.size(); j++)
                for (int k = j + 1; k < nums.size(); k++)
                    if (abs(nums[i] + nums[j] + nums[k] - target) < abs(ans - target))
                        ans = nums[i] + nums[j] + nums[k];

        return ans;
    }
};
```
## 17. 电话号码的字母组合

![](/17.png)

```c++
class Solution {
public:
    string chars[8] = {"abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};

    vector<string> letterCombinations(string digits) {
        if(digits.empty()) return vector<string>();

        vector<string> state(1,"");
        for(auto u : digits)
        {
            vector<string> now;
            for(auto c : chars[u - '2'])
                for(auto s : state)
                    now.push_back(s + c);
            state = now;
        }
        return state;
    }
};
/*
class Solution {
public:
    vector<string> ans;
    string strs[10] = {"","","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};

    vector<string> letterCombinations(string digits) {
        if(digits.empty()) return ans;
        dfs(digits,0,"");
        return ans;
    }

    void dfs(string &digits,int u,string path)
    {
        if(u == digits.size()) ans.push_back(path);
        else
        {
            for(auto c : strs[digits[u] - '0'])
                dfs(digits,u + 1,path + c);
        }
    }
};
*/
```
## 18. 四数之和

![](/18.png)

```c++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        int n = nums.size();
        for (int i = 0; i < n; i++) {
            while (i > 0 && i < n && nums[i] == nums[i - 1])
                i++;
            for (int j = i + 1; j < n; j++) {
                while (j != i + 1 && j < n && nums[j] == nums[j - 1])
                    j++;
                int l = j + 1, r = n - 1;
                while (l < r) {
                    if (nums[i] + nums[j] + nums[l] + nums[r] == target) {
                        res.push_back({nums[i], nums[j], nums[l], nums[r]});
                        do { l++; } while (l < r && nums[l - 1] == nums[l]);
                        do { r--; } while (l < r && nums[r] == nums[r + 1]);
                    }
                    else if (nums[i] + nums[j] + nums[l] + nums[r] < target) {
                        do { l++; } while (l < r && nums[l - 1] == nums[l]);
                    }
                    else {
                        do { r--; } while (l < r && nums[r] == nums[r + 1]);
                    }
                }
            }
        }
        return res;
    }
};
```
## 19. 删除链表的倒数第N个节点

![](/19.png)

```c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *dummy = new ListNode(-1);
        dummy->next = head;
        ListNode *first = dummy, *second = dummy;
        for(int i = 0;i < n;i ++) first = first->next;
        while(first->next)
        {
            first = first->next;
            second = second->next;
        }

        second->next = second->next->next;
        return dummy->next;
    }
};
```
## 20. 有效的括号

![](/20.png)

```c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> stk;
        for (int i = 0; i < s.length(); i++) {
            if (s[i] == '(' || s[i] == '[' || s[i] == '{')
                stk.push(s[i]);
            else if (s[i] == ')') {
                if (stk.empty() || stk.top() != '(')
                    return false;
                stk.pop();
            }
            else if (s[i] == ']') {
                if (stk.empty() || stk.top() != '[')
                    return false;
                stk.pop();
            }
            else {
                if (stk.empty() || stk.top() != '{')
                    return false;
                stk.pop();
            }
        }
        return stk.empty();
    }
};
```