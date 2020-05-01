---
typora-root-url: img
---

## 141. 环形链表

![](141.png)

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
    bool hasCycle(ListNode *head) {
        if(!head || !head->next) return NULL;
        auto first = head,second = head->next;
        while(first && second)
        {
            if(first == second) return true;
            first = first->next;
            second = second->next;
            if(second) second = second->next;
        }
        return false;
    }
};
```
## 142. 环形链表 II

![](142.png)

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
    ListNode *detectCycle(ListNode *head) {
        auto fast = head,slow = head;
        while(fast)
        {
            fast = fast->next;
            slow = slow->next;
            if(fast) fast = fast->next;
            else break;

            if(fast == slow)
            {
                fast = head;
                while(slow != fast)
                {
                    fast = fast->next;
                    slow = slow->next;
                }
                return slow;
            }
        }
        return NULL;
    }
};
```
## 143. 重排链表

![](143.png)

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
    void reorderList(ListNode* head) {
        int n = 0;
        for (ListNode *p = head; p; p = p->next) n ++ ;
        if (n <= 2) return;
        ListNode *later = head;
        for (int i = 0; i + 1 < (n + 1) / 2; i ++ )
            later = later->next;
        ListNode *a = later, *b = later->next;

        while (b)
        {
            ListNode *c = b->next;
            b->next = a;
            a = b;
            b = c;
        }

        later->next = 0;
        while (head && head != a)
        {
            b = a->next;
            a->next = head->next;
            head->next = a;
            head = head->next->next;
            a = b;
        }
    }
};
```
## 144. 二叉树的前序遍历

![](144.png)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        while (root)
        {
            if (!root->left)
            {
                res.push_back(root->val);
                root = root->right;
            }
            else
            {
                TreeNode *pre = root->left;
                while (pre->right && pre->right != root) pre = pre->right;
                if (pre->right) pre->right = 0, root = root->right;
                else
                {
                    pre->right = root;
                    res.push_back(root->val);
                    root = root->left;
                }
            }
        }
        return res;
    }
};
```
## 145. 二叉树的后序遍历

![](145.png)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
      vector<int> result, left, right;
      if(!root) return result;

      left = postorderTraversal(root->left);
      for(auto &x:left) result.push_back(x);

      right = postorderTraversal(root->right);
      for(auto &x:right) result.push_back(x);

      result.push_back(root->val);

      return result;


    }
};
```
## 146. LRU缓存机制

![](146.png)

```c++
class LRUCache {
public:
    struct Node
    {
        int val, key;
        Node *left, *right;
        Node() : key(0), val(0), left(NULL), right(NULL) {}
    };
    Node *hu, *tu; // hu: head_used, tu: tail_used; head在左侧，tail在右侧
    Node *hr, *tr; // hr: head_remains, tr: tail_remains; head在左侧，tail在右侧
    int n;
    unordered_map<int, Node*> hash;

    void delete_node(Node *p)
    {
        p->left->right = p->right, p->right->left = p->left;
    }

    void insert_node(Node *h, Node *p)
    {
        p->right = h->right, h->right = p;
        p->left = h, p->right->left = p;
    }

    LRUCache(int capacity) {
        n = capacity;
        hu = new Node(), tu = new Node();
        hr = new Node(), tr = new Node();
        hu->right = tu, tu->left = hu;
        hr->right = tr, tr->left = hr;

        for (int i = 0; i < n; i ++ )
        {
            Node *p = new Node();
            insert_node(hr, p);
        }
    }

    int get(int key) {
        if (hash[key])
        {
            Node *p = hash[key];
            delete_node(p);
            insert_node(hu, p);
            return p->val;
        }
        return -1;
    }

    void put(int key, int value) {
        if (hash[key])
        {
            Node *p = hash[key];
            delete_node(p);
            insert_node(hu, p);
            p->val = value;
            return;
        }

        if (!n)
        {
            n ++ ;
            Node *p = tu->left;
            hash[p->key] = 0;
            delete_node(p);
            insert_node(hr, p);
        }

        n -- ;
        Node *p = hr->right;
        p->key = key, p->val = value, hash[key] = p;
        delete_node(p);
        insert_node(hu, p);
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

## 147. 对链表进行插入排序

![](147.png)

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
    ListNode* insertionSortList(ListNode* head) {
        auto dummy = new ListNode(-1);
        while(head)
        {
            auto p = head->next,q = dummy;
            while(q->next && q->next->val <= head->val) q = q->next;

            head->next = q->next;
            q->next = head;

            head = p;
        }
        return dummy->next;
    }
};
```
## 148. 排序链表

![](148.png)

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
    ListNode* sortList(ListNode* head) {
        int n = 0;
        for (ListNode *p = head; p; p = p->next) n ++ ;

        ListNode *dummy = new ListNode(-1);
        dummy->next = head;
        for (int i = 1; i < n; i *= 2)
        {
            ListNode *begin = dummy;
            for (int j = 0; j + i < n; j += i * 2)
            {
                ListNode *first = begin->next, *second = first;
                for (int k = 0; k < i; k ++ )
                    second = second->next;
                int f = 0, s = 0;
                while (f < i && s < i && second)
                    if (first->val < second->val)
                    {
                        begin = begin->next = first;
                        first = first->next;
                        f ++ ;
                    }
                    else
                    {
                        begin = begin->next = second;
                        second = second->next;
                        s ++ ;
                    }

                while (f < i)
                {
                    begin = begin->next = first;
                    first = first->next;
                    f ++ ;
                }
                while (s < i && second)
                {
                    begin = begin->next = second;
                    second = second->next;
                    s ++ ;
                }

                begin->next = second;
            }
        }

        return dummy->next;
    }
};
```
## 149. 直线上最多的点数

![](149.png)

```c++
/**
 * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */
class Solution {
public:
    int maxPoints(vector<Point>& points) {
        if (points.empty()) return 0;
        int res = 1;
        for (int i = 0; i < points.size(); i ++ )
        {
            unordered_map<long double, int> map;
            int duplicates = 0, verticals = 1;

            for (int j = i + 1; j < points.size(); j ++ )
                if (points[i].x == points[j].x)
                {
                    verticals ++ ;
                    if (points[i].y == points[j].y) duplicates ++ ;
                }

            for (int j = i + 1; j < points.size(); j ++ )
                if (points[i].x != points[j].x)
                {
                    long double slope = (long double)(points[i].y - points[j].y) / (points[i].x - points[j].x);
                    if (map[slope] == 0) map[slope] = 2;
                    else map[slope] ++ ;
                    res = max(res, map[slope] + duplicates);
                }

            res = max(res, verticals);
        }
        return res;
    }
};
```
## 150. 逆波兰表达式求值

![](150.png)

```c++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> sta;
        for (auto &t : tokens)
            if (t == "+" || t == "-" || t == "*" || t == "/")
            {
                int a = sta.top();
                sta.pop();
                int b = sta.top();
                sta.pop();
                if (t == "+") sta.push(a + b);
                else if (t == "-") sta.push(b - a);
                else if (t == "*") sta.push(a * b);
                else sta.push(b / a);
            }
            else sta.push(atoi(t.c_str()));
        return sta.top();
    }
};
```
## 151. 翻转字符串里的单词

![](151.png)

```c++
class Solution {
public:
    string reverseWords(string s) {
        int k = 0;
        for(int i = 0;i <s.size();i ++)
        {
            int j = i;
            while(j < s.size() && s[j] == ' ') j ++;
            if(j == s.size()) break;
            i = j;
            while(j < s.size() && s[j] != ' ') j ++;
            reverse(s.begin() + i,s.begin() + j);
            if(k) s[k ++] = ' ';
            while(i < j) s[k ++] = s[i ++];
        } 
        s.erase(s.begin() + k,s.end());
        reverse(s.begin(),s.end());

        return s;
    }
};
```
## 152. 乘积最大子数组

![](152.png)

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int pos = 0, neg = 0;
        int res = INT_MIN;
        for (auto x : nums)
        {
            if (x > 0)
            {
                if (pos > 0) pos *= x;
                else pos = x;
                neg *= x;
            }
            else if (x < 0)
            {
                int np = pos, ng = neg;
                np = neg * x;
                if (pos > 0) ng = pos * x;
                else ng = x;
                pos = np, neg = ng;
            }
            else
            {
                pos = neg = 0;
            }
            res = max(res, x);
            if (pos != 0) res = max(res, pos);
        }
        return res;
    }
};
```
## 153. 寻找旋转排序数组中的最小值

![](153.png)

```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0,r = nums.size() - 1;
        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mid] <= nums.back()) r = mid;
            else l = mid + 1;
        }
        return nums[r];
    }
};
```
## 154. 寻找旋转排序数组中的最小值 II

![](154.png)

```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        while (l < r && nums[r] == nums[l]) r -- ;
        // 其实这块写r - l + 1 < 2就OK，不过思考的时候可以偷个懒，直接把小于5的全部特盘掉了。
        if (r - l + 1 < 5)
        {
            int res = INT_MAX;
            for (int x : nums) res = min(res, x);
            return res;
        }
        if (nums[0] <= nums[r]) return nums[0];
        else
        {
            int end = nums[r];
            while (l < r)
            {
                int mid = (l + r + 1) / 2;
                if (nums[mid] >= nums[0]) l = mid;
                else r = mid - 1;
            }
            return nums[r + 1];
        }
    }
};
```
## 155. 最小栈

![](155.png)

```c++
class MinStack {
public:
    stack<int> stk,stk_min;
    /** initialize your data structure here. */
    MinStack() {

    }
    
    void push(int x) {
        stk.push(x);
        if(stk.empty() || stk_min.top() >= x)
            stk_min.push(x);
    }
    
    void pop() {
        if(stk_min.top() == stk.top()) stk_min.pop();
        stk.pop();
    }
    
    int top() {
        return stk.top();
    }
    
    int getMin() {
        return stk_min.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```
## 160. 相交链表

![](160.png)

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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        auto p = headA,q = headB;
        while(p != q)
        {
            if(p) p = p->next;
            else p = headB;
            if(q) q = q->next;
            else q = headA;
        }
        return p;
    }
};
```