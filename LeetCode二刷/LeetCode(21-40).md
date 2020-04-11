---
typora-root-url: img
---

## 21. 合并两个有序链表

![](21.png)

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        auto dummy = new ListNode(-1);
        auto p = dummy;
        while(l1 && l2)
        {
            if (l1->val < l2->val)
            {
                p->next = l1;
                p = l1;
                l1 = l1->next;
            }
            else
            {
                p->next = l2;
                p = l2;
                l2 = l2->next;
            }
        }
        if (!l1) l1 = l2;
        while(l1)
        {
            p->next = l1;
            p = l1;
            l1 = l1->next;
        }
        return dummy->next;
    }
};
```
## 22. 括号生成

![](22.png)

```c++
class Solution {
public:
    vector<string> res;
    void solve(int l ,int r,int n,string cur)
        {
            if(l == n && r == n)
            {
                res.push_back(cur);
                return;
            }
            if(l < n) solve(l + 1,r,n,cur + "(");
            if(r < l) solve(l, r + 1, n, cur + ")");
        }
    vector<string> generateParenthesis(int n) {
        if(n == 0) return res;
        solve(0,0,n,"");
        return res;
    }
};
```
## 23. 合并K个排序链表

![](23.png)

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
    ListNode* merge2Lists(ListNode* l1, ListNode* l2) {
        ListNode *head = new ListNode(0);
        ListNode *cur = head;
        while (l1 != NULL && l2 != NULL) {
            if (l1 -> val < l2 -> val) {
                cur -> next = l1;
                l1 = l1 -> next;
            }
            else {
                cur -> next = l2;
                l2 = l2 -> next;
            }
            cur = cur -> next;
        }
        cur -> next = (l1 != NULL ? l1 : l2);
        return head -> next;
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.size() == 0)
            return NULL;
        if (lists.size() == 1)
            return lists[0];

        int mid = lists.size() / 2;
        vector<ListNode*> left = vector<ListNode*>(lists.begin(), lists.begin() + mid);
        vector<ListNode*> right = vector<ListNode*>(lists.begin() + mid, lists.end());
        ListNode *l1 = mergeKLists(left);
        ListNode *l2 = mergeKLists(right);
        return merge2Lists(l1, l2);
    }
};
```
## 24. 两两交换链表中的节点

![](24.png)

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
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummy = new ListNode(0);
        dummy -> next = head;
        ListNode* cur = dummy;

        while (cur != NULL) {
            ListNode* first = cur -> next;
            if (first == NULL)
                break;

            ListNode* second = first -> next;
            if (second == NULL)
                break;

            // 按照一定的次序，交换相邻的两个结点。
            cur -> next = second;
            first -> next = second -> next;
            second -> next = first;

            cur = first;
        }
        return dummy -> next;
    }
};
```
## 25. K 个一组翻转链表

![](25.png)

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
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode* dummy = new ListNode(0);
        dummy -> next = head;
        ListNode* cur = dummy;

        while (cur != NULL) {
            ListNode *first = cur -> next;
            ListNode *end = cur;

            for (int i = 0; i < k && end != NULL; i++)
                end = end -> next;

            if (end == NULL)
                break;
            //change;
            ListNode *p1 = first;
            ListNode *p2 = first -> next;
            while (p1 != end) {
                ListNode *new_p2 = p2 -> next;
                p2 -> next = p1;
                p1 = p2;
                p2 = new_p2;
            }

            first -> next = p2;
            cur -> next = end;
            cur = first;
        }
        return dummy -> next;
    }
};
```
## 26. 删除排序数组中的重复项

![](26.png)

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.empty()) return 0;
        int k = 1;
        for(int i = 1;i < nums.size();i ++)
        {
            if(nums[i] != nums[i - 1])
                nums[k ++] = nums[i];
        }
        return k;
    }
};
```
## 27. 移除元素

![](27.png)

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int k = 0;
        for(int i =0 ;i < nums.size();i++)
            if(nums[i] != val)
                nums[k ++] =  nums[i];
        
        return k;
                
    }
};
```
## 28. 实现 strStr()

![](28.png)

```c++
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = haystack.size(),m = needle.size();
        for(int i= 0 ;i < n - m + 1;i ++)
        {
            bool flag = true;
            for(int j = 0;j < m;j ++)
                if(haystack[i + j] != needle[j])
                {
                    flag = false;
                    break;
                }
            if(flag) return i;
        }
        return -1;

    }
};
```
## 29. 两数相除

![](29.png)

```c++
class Solution {
public:
    int divide(int dividend, int divisor) {
        const int HALF_INT_MIN = -1073741824;
        int x = dividend, y = divisor;

        bool sign = (x > 0) ^ (y > 0);

        if (x > 0) x = -x;
        if (y > 0) y = -y;

        vector<pair<int, int>> ys;

        for (int t1 = y, t2 = -1; t1 >= x; t1 += t1, t2 += t2) {
            ys.emplace_back(t1, t2);
            if (t1 < HALF_INT_MIN)
                break;
        }

        int ans = 0;
        for (int i = ys.size() - 1; i >= 0; i--)
            if (x <= ys[i].first) {
                x -= ys[i].first;
                ans += ys[i].second;
            }

        if (!sign) {
            if (ans == INT_MIN)
                return INT_MAX;
            ans = -ans;
        }

        return ans;
    }
};
```
## 30. 串联所有单词的子串

![](30.png)

```c++
class Solution {
public:
    int check(string s, int begin, int n, int len, int tot,
            unordered_map<string, int>& wc, vector<int>& ans) {
        // 寻找以begin开始的子串的所有匹配，并返回下一次开始匹配的位置。
        unordered_map<string, int> vis;
        int count = 0;
        for (int i = begin; i < n - len + 1; i += len) {
            string candidate = s.substr(i, len);
            if (wc.find(candidate) == wc.end())
                // 遇到不合法的候选单词，直接返回下一次的开始位置为i+len。
                return i + len;

            while (vis[candidate] == wc[candidate]) {
                // 遇到多余的候选单词，不断从begin开始删除候选单词，直到当前候选单词合法。
                vis[s.substr(begin, len)]--;
                count--;
                begin += len;
            }
            vis[candidate]++; // 插入候选单词。
            count++;
            if (count == tot) // 找到一个位置，记录答案。
                ans.push_back(begin);
        }
        return n;
    }
    vector<int> findSubstring(string s, vector<string>& words) {
        vector<int> ans;
        int n = s.length();
        int tot = words.size();
        if (tot == 0) return ans;

        unordered_map<string, int> wc;

        for(int i = 0; i < tot; i++)
            wc[words[i]]++;
        int len = words[0].length();

        for (int offset = 0; offset < len; offset++) // 枚举划分
            for (int begin = offset; begin < n;
                begin = check(s, begin, n, len, tot, wc, ans));

        return ans;
    }
};
```
## 31. 下一个排列

![](31.png)

```c++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int n = nums.size();
        int j = -1;
        for(int i = n - 2;i >= 0;i --)
            if(nums[i] < nums[i + 1])
            {
                j = i;
                break;
            }
        if(j == -1) reverse(nums.begin(),nums.end());
        else
        {
            for(int i = n - 1;i > j;i --)
                if(nums[i] > nums[j])
                {
                    swap(nums[i],nums[j]);
                    break;
                }
            reverse(nums.begin() + j + 1,nums.end());
        }
    }
};
```
## 32. 最长有效括号

![](32.png)

```c++
class Solution {
public:

    int work(string s)
    {
        int res = 0;
        for(int i = 0,start = 0,cnt = 0;i <s.size();i ++)
            if(s[i] == '(') cnt ++;
            else
            {
                cnt --;
                if(cnt < 0) start  = i + 1,cnt = 0;
                else if(!cnt) res = max(res,i - start + 1);
            }
        return res;
    }
    int longestValidParentheses(string s) {
        int res = work(s);
        reverse(s.begin(),s.end());
        for(auto &c : s) c ^= 1;
        return max(res,work(s));
    }
};
```
## 33. 搜索旋转排序数组

![](33.png)

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if(nums.empty()) return -1;
        
        int l = 0,r = nums.size() - 1;
        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mid] <= nums.back()) r = mid;
            else l = mid + 1;
        }

        if(target <= nums.back()) r = nums.size() - 1;
        else l = 0,r --;

        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mid] >= target) r = mid;
            else l = mid + 1;
        }

        if(nums[l] == target) return l;
        return -1;
    }
};
```
## 34. 在排序数组中查找元素的第一个和最后一个位置

![](34.png)

```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        if(nums.empty()) return {-1,-1};

        int l = 0,r = nums.size() - 1;
        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mid] >= target) r = mid;
            else l = mid + 1;
        }

        if(nums[r] != target) return {-1,-1};
        int strat = r;
        l = 0,r = nums.size() - 1;
        while(l  < r)
        {
            int mid = l + r + 1 >> 1;
            if(nums[mid] <= target) l = mid;
            else r = mid - 1;
        }
        int end = l;
        return {strat,end};
    }
};
```
## 35. 搜索插入位置

![](35.png)

```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int l = 0,r = nums.size();
        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mid] >= target) r = mid;
            else l = mid + 1;
        }
        return l;
    }
};
```
## 36. 有效的数独

![](36.png)

```c++
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        vector<int> row(9), col(9), squ(9); // 使用三个整型数组判重。
        for (int i = 0; i < 9; i++)
            for (int j = 0; j < 9; j++) {
                if (board[i][j] == '.')
                    continue;
                if (board[i][j] < '1' || board[i][j] > '9') return false;
                int num = board[i][j] - '0';

                // 以row[i] & (1 << num)为例，这是判断第i行中，num数字是否出现过。
                // 即row[i]值的二进制表示中，第num位是否是1。
                // 以下col和squ同理。

                if ((row[i] & (1 << num)) ||
                    (col[j] & (1 << num)) ||
                    (squ[(i / 3) * 3 + (j / 3)] & (1 << num)))
                    return false;

                row[i] |= (1 << num);
                col[j] |= (1 << num);
                squ[(i / 3) * 3 + (j / 3)] |= (1 << num);
            }
        return true;
    }
};
```
## 37. 解数独

![](37.png)

```c++
class Solution {
public:
    bool row[9][9] = {0},col[9][9] = {0},cell[3][3][9] = {0};//初始化
    void solveSudoku(vector<vector<char>>& board) {
        for(int i = 0;i < 9;i ++)
            for(int j = 0;j < 9;j ++)
            {
                char c = board[i][j];
                if(c != '.')
                {
                    int t = c - '1';
                    row[i][t] = col[j][t] = cell[i / 3][j / 3][t] = true;
                }
            }

        dfs(board,0,0);//左上角开始走
    }

    bool dfs(vector<vector<char>> &board,int x,int y)
    {
        if(y == 9) x ++ ,y = 0;//出界就去下一行
        if(x == 9) return true;//整个数都做完了
        if(board[x][y] != '.') return dfs(board,x,y + 1);//已经填了数，调到下一个

        for(int i = 0;i < 9;i ++)
        {
            if(!row[x][i] && !col[y][i] && !cell[x / 3][y / 3][i])
            {
                board[x][y] = '1' + i;//更新状态
                row[x][i] = col[y][i] = cell[x / 3][y / 3][i] = true;
                if(dfs(board,x,y + 1)) return true;
                row[x][i] = col[y][i] = cell[x / 3][y / 3][i] = false;
                board[x][y] = '.';//恢复状态
            }
        }
        return false;
    }
};
```
## 38. 外观数列

![](38.png)

```c++
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
                while(k < res.size() && res[k] == res[j]) k ++;
                ns += to_string(k - j) + res[j];
                j = k;
            }
            res = ns;
        }
        return res;
    }
};
```
## 39. 组合总和

![](39.png)

```c++
class Solution {
public:
    void solve(int i, vector<int>& candidates, int sum,
             vector<int> &ch, int target, vector<vector<int>>& ans) {
        // 注意这里的 ch 是引用。
        if (sum == target) { // 找到目标值，添加答案。
            ans.push_back(ch);
            return;
        }
        if (i == candidates.size() || sum > target) // 超出范围回溯。
            return;

        solve(i + 1, candidates, sum, ch, target, ans);

        ch.push_back(candidates[i]);
        solve(i, candidates, sum + candidates[i], ch, target, ans);
        ch.pop_back();
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> ans;
        sort(candidates.begin(), candidates.end());
        vector<int> ch; // ch 记录已选择的数字。
        solve(0, candidates, 0, ch, target, ans);
        return ans;
    }
};
```
## 40. 组合总和 II

![](40.png)

```c++
class Solution {
public:
    void solve(int i, vector<int>& candidates, int sum,
             vector<int>& ch, int target, vector<vector<int>>& ans) {
             // 注意这里的 ch 是引用。
        if (sum == target) {
            ans.push_back(ch);
            return;
        }
        if (i == candidates.size() || sum > target)
            return;

        for (int j = i + 1; j < candidates.size(); j++)
            if (candidates[j] != candidates[i]) {
                // 不用该层的数字时，需要找到下一个不同的数字。
                solve(j, candidates, sum, ch, target, ans);
                break;
            }

        // 选择该层的数字。
        ch.push_back(candidates[i]);
        solve(i + 1, candidates, sum + candidates[i], ch, target, ans);
        ch.pop_back();
    }


    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<vector<int>> ans;
        sort(candidates.begin(), candidates.end());
        vector<int> ch; // ch 记录选择的数字。
        solve(0, candidates, 0, ch, target, ans);
        return ans;
    }
};
```