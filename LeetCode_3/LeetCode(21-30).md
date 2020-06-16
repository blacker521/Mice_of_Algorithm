---
typora-root-url: img
---

## 21. 合并两个有序链表

![](/21.png)

```c++
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

![](/22.png)

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

![](/23.png)

```c++
class Solution {
public:
    struct Cmp{
        bool operator() (ListNode* a, ListNode *b)//重载小于号
        {
            return a->val > b->val;
        }
    };

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        priority_queue<ListNode* , vector<ListNode*>,Cmp> heap;
        auto dummy = new ListNode(-1),tail = dummy;
        for(auto l : lists) if(l) heap.push(l);

        while(heap.size())
        {
            auto t = heap.top();
            heap.pop();

            tail = tail -> next = t;
            if(t->next) heap.push(t->next);
        }
        return dummy->next;
    }
};
```
## 24. 两两交换链表中的节点

![](/24.png)

```c++
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
/*
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        auto dummy = new ListNode(-1);
        dummy->next = head;
        for(auto p = dummy;p -> next && p->next->next;)
        {
            auto a = p->next,b = a->next;
            p->next = b;
            a->next = b->next;
            b->next = a;
            p = a;
        }
        return dummy->next;
    }
};*/
```
## 25. K 个一组翻转链表

![](/25.png)

```c++
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

![](/26.png)

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.empty()) return 0;
        int k = 1;
        for(int i = 1;i < nums.size();i ++)
            if(nums[i] != nums[i - 1])
                nums[k ++] = nums[i];
        return k;
    }
};
```
## 27. 移除元素

![](/27.png)

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

![](/28.png)

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

![](/29.png)

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
/*
class Solution {
public:
    int divide(int x, int y) {
        typedef long long LL;
        vector<LL> exp;
        bool is_minus = false;
        if (x < 0 && y > 0 || x > 0 && y < 0) is_minus = true;

        LL a = abs((LL)x), b = abs((LL)y);
        for (LL i = b; i <= a; i = i + i) exp.push_back(i);

        LL res = 0;
        for (int i = exp.size() - 1; i >= 0; i -- )
            if (a >= exp[i]) {
                a -= exp[i];
                res += 1ll << i;
            }

        if (is_minus) res = -res;

        if (res > INT_MAX || res < INT_MIN) res = INT_MAX;

        return res;
    }
};
*/
```
## 30. 串联所有单词的子串

![](/30.png)

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
/*
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        vector<int> res;
        if (words.empty()) return res;
        int n = s.size(), m = words.size(), w = words[0].size();
        unordered_map<string, int> tot;
        for (auto& word : words) tot[word] ++ ;

        for (int i = 0; i < w; i ++ ) {
            unordered_map<string, int> wd;
            int cnt = 0;
            for (int j = i; j + w <= n; j += w) {
                if (j >= i + m * w) {
                    auto word = s.substr(j - m * w, w);
                    wd[word] -- ;
                    if (wd[word] < tot[word]) cnt -- ;
                }
                auto word = s.substr(j, w);
                wd[word] ++ ;
                if (wd[word] <= tot[word]) cnt ++ ;
                if (cnt == m) res.push_back(j - (m - 1) * w);
            }
        }

        return res;
    }
};
*/
```
