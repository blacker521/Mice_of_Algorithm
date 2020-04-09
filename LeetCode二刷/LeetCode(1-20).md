---
typora-root-url: img
---

## 1. 两数之和

![](1.png)

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> hash;
        for(int i = 0;i < nums.size();i ++)
        {
            if(hash.count(target - nums[i])) return {hash[target -nums[i]],i};
            hash[nums[i]] = i;
        }
        return {-1,-1};
    }
};
```
## 2. 两数相加

![](2.png)

```c++
class Solution {
public:
    ListNode *addTwoNumbers(ListNode *l1, ListNode *l2) 
    {
        ListNode *res = new ListNode(-1);   //添加虚拟头结点，简化边界情况的判断
        ListNode *cur = res;
        int carry = 0;  //表示进位
        while (l1 || l2) 
        {
            int n1 = l1 ? l1->val : 0;
            int n2 = l2 ? l2->val : 0;
            int sum = n1 + n2 + carry;
            carry = sum / 10;
            cur->next = new ListNode(sum % 10);
            cur = cur->next;
            if (l1) l1 = l1->next;
            if (l2) l2 = l2->next;
        }
        if (carry) cur->next = new ListNode(1); //如果最高位有进位，则需在最前面补1.
        return res->next;   //返回真正的头结点
    }
};
```
## 3. 无重复字符的最长子串

![](3.png)

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char,int> hash;
        int res = 0;
        for(int i = 0,j = 0;j < s.size();j ++)
        {
            hash[s[j]] ++;
            while(hash[s[j]] > 1) hash[s[i ++]] --;
            res = max(res,j - i + 1);
        }
        return res;
    }
};
```
## 4. 寻找两个有序数组的中位数

![](4.png)

```c++
class Solution {
public:
    string intToRoman(int num) {
        int values[] = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        string reps[] = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

        string res;
        int 
        for(int i = 0; i < 13; i ++ )
            while(num >= values[i])
            {
                num -= values[i];
                res += reps[i];
            }
        return res;
    }
};
```
## 5. 最长回文子串

![](5.png)

```c++
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
## 6. Z 字形变换

![](6.png)

```c++
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
## 7. 整数反转

![](7.png)

```c++
class Solution {
public:
    int reverse(int x) {
        string s = to_string(x); 
        int n = s.size();
        long long t;
        for(int i = 0; i < n / 2; i++) swap(s[i],s[n - i -1]);
        t = stoi(s);
        return x < 0 ? -t : t;
    }
};
```
## 8. 字符串转换整数 (atoi)

![](8.png)

```c++
class Solution {
public:
    int myAtoi(string str) {
        int ans = 0,i = 0,flag = 1;
        while(str[i] == ' ') i++;
        if(str[i] == '-'){
            flag = -1;
        }
        if(str[i] == '+' || str[i] == '-') i++;
        while(i < str.size() && isdigit(str[i])){
            int r = str[i] - '0'; 
            if(ans > INT_MAX / 10 || (ans == INT_MAX / 10 && r > 7)){
                return flag > 0 ? INT_MAX : INT_MIN;
            }
            ans = ans * 10 + r;
            i++;
        }
        return flag > 0 ? ans : -ans;
        
    }
};
```
## 9. 回文数

![](9.png)

```c++
class Solution {
 public:
     bool isPalindrome(int x) {
         string str = to_string(x);
         int mid = (str.length() % 2 == 1) ? (str.length()/2) :(str.length()/2-1);
         if(str.length() % 2 == 1)
         {
             for(int i = 1;i + mid < str.length(); i ++){
                 if(str[mid + i] != str[mid - i])return false;
             }
             return true;
         }
         else{
             for(int i = 1;i + mid < str.length();i ++){
                 if(str[mid + i] != str[mid - i + 1])return false;
             }
             return true;
         }
     }
 };
```
## 10. 正则表达式匹配

![](10.png)

## 11. 盛最多水的容器

![](11.png)

## 12. 整数转罗马数字

![](12.png)

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

![](13.png)

```c++
class Solution {
public:
    string intToRoman(int num)
    {
        char* c[4][10] = {
            {"","I","II","III","IV","V","VI","VII","VIII","IX"},
            {"","X","XX","XXX","XL","L","LX","LXX","LXXX","XC"},
            {"","C","CC","CCC","CD","D","DC","DCC","DCCC","CM"},
            {"","M","MM","MMM"}
        };
        string roman;
        roman.append(c[3][num / 1000]);
        roman.append(c[2][num / 100 % 10]);
        roman.append(c[1][num / 10 % 10]);
        roman.append(c[0][num % 10]);
         
        return roman;
    }
};
```
## 14. 最长公共前缀

![](14.png)

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
```
## 15. 三数之和

![](15.png)

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

![](16.png)

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

![](17.png)

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
```
## 18. 四数之和

![](18.png)

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

![](19.png)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
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

![](20.png)

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