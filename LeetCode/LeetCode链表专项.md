---
typora-root-url: img/
---

# *链表专题*
## LeetCode 19. 删除链表的倒数第N个节点

![19](19.png)

```c++
/**双指针
	1.建立虚拟头结点
	2.first向后走n步
    3.first、second同时向后走，当first走到末尾时停止。
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
        ListNode *dummy = new ListNode(-1);//创建虚拟头结点
        dummy->next = head;//虚拟头结点指向head节点
        ListNode *first = dummy, *second = dummy;//双指针
        for (int i = 0; i < n; i ++ ) first = first->next;
        while (first -> next)
        {
            first = first->next;
            second = second->next;
        }
        second->next = second->next->next;
        return dummy->next;//虚拟头结点的下一个节点，不能是head！！！
    }
};
```
## LeetCode 237. 删除链表中的节点

![](237.png)

```c++
/*直接指向下一个节点
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
    void deleteNode(ListNode* node) {
        node->val = node->next->val;
        node->next = node->next->next;
        //还可以写成*(node) = *(node->next);
        //C++的特性，*(node)取node结构体的指针和地址！！！
    }
};
```
## LeetCode 83.删除排序链表中的重复元素

![](83.png)

```c++
/*相同的都排在一起，只取第一个
1.下一个点和当前点相同，则删掉下一点
2.下一个点和当前点不相同，则移动指针
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
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head) return NULL;
        ListNode *first, *second;
        first = second = head;
        while (first)
        {
            if (first->val != second->val)
            {
                second->next = first;
                second = first;
            }
            first = first->next;
        }
        second->next = NULL;
        return head;
    }
};

简洁版：
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
		auto cur = head;
        while(cur){
            if(cur->next && cur->next->val == cur->val)
                cur->next = cur->next->next;
            else cur = cur->next;
        }
        return head;
    }
};
```
## LeetCode 206. 反转链表
![](206.png)

```c++
/*指针变方向，递归
双指针，
c = b->next
b ->next = a;
a = b,b = c;
b为空时a为头结点
链表形式：a--b--c....
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
    ListNode* reverseList(ListNode* head) {
        if (!head) return NULL;
        if (!head->next) return head;
        ListNode *p = reverseList(head->next);
        head->next->next = head;
        head->next = NULL;
        return p;
    }
};
***********************************************************************
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
		if(!head) return NULL;
        
        auto a = head,b = head->next;
        while(b)
        {
            auto c = b->next;
            b->next = a;
            a = b,b = c;
		}
        head->next = NULL;
        return a;
    }
};
```

## LeetCode 92. 反转链表 II

![](92.png)

![](92_1.png)
```c++
/*先求三个点的位置
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
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        if (m == n) return head;//不用操作
        ListNode *dummy = new ListNode(-1);
        dummy->next = head;
        ListNode *b = dummy;
        for (int i = 0; i < m - 1; i ++ ) b = b->next;
        ListNode *a = b;
        b = b->next;
        ListNode *c = b->next;
        for (int i = 0; i < n - m; i ++ )
        {
            ListNode *t = c->next;
            c->next = b;
            b = c, c = t;
        }
        ListNode *mp = a->next;
        ListNode *np = b;
        a->next = np, mp->next = c;
        return dummy->next;
    }
};
```
## LeetCode 61. 旋转链表

![](61.png)

```c++
/*把最后k个移动到开头
1.k %= n 保证k在0 ~ n之间！！！
2.双指针，first指针从头往后走K步
3.first、second同时往后走，当first走到结尾时停止
4.移动指针
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
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head) return head;

        int n = 0;
        ListNode *p = head;
        while (p)
        {
            n ++ ;
            p = p->next;
        }

        k %= n;
        if (!k) return head;


        ListNode *first = head;
        while (k -- && first) first = first->next;
        ListNode *second = head;
        while (first->next)
        {
            first = first->next;
            second = second->next;
        }
        //移动指针
        first->next = head;
        head = second->next;
        second->next = NULL;
        return head;
    }
};

***********************简洁版**************************
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
    ListNode* rotateRight(ListNode* head, int k) {
        if(!head) return NULL;

        int n = 0;
        for(auto p = head;p;p = p->next) n ++;

        k %= n;
        auto first = head,second = head;
        while(k --) first = first->next;
        while(first->next)
        {
            first = first->next;
            second = second->next;
        }
        first->next = head;
        head = second->next;
        second->next = NULL;

        return head;
    }
};
```

## LeetCode 24.两两交换链表中的节点

![](24.png)

```c++
/*相邻两点之间交换
1.建立虚拟头结点
2.交换指针，移动指针
p->next = b;
a->next = b->next;
b->next = a;
p = a;
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
        auto dummy = new ListNode(-1);
        dummy->next = head;

        for(auto p = dummy;p && p->next && p->next->next;p = p->next)
        {
            auto a = p->next,b = a->next;
            p->next = b;
            a->next = b->next;
            b->next = a;
            p = a;
        }
        return dummy->next;
    }
};
```

## LeetCode 160. 相交链表 !!!

![](160.png)

![](160_1.png)

```c++
/*p,q都走完a+b+c之后一定相遇！！！妙啊~
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
        ListNode *p = headA, *q = headB;
        while (p != q)
        {
            if (p) p = p->next;
            else p = headB;
            if (q) q = q->next;
            else q = headA;
        }
        return p;
    }
};
```

## LeetCode 142. 环形链表 II

![](142.png)

![](142_1.png)

```c++
/*
1.快慢指针，快的每次走一步，慢的每次走两步。
2.在环中每走y步两指针距离就少一
3.相遇后把慢的放到开头
4.快慢每次都走一步，当再次相遇时就是环的入口
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
        if (!head || !head->next) return 0;
        ListNode *first = head, *second = head;

        while (first && second)
        {
            first = first->next;
            second = second->next;
            if (second) second = second->next;
            else return 0;

            if (first == second)
            {
                first = head;
                while (first != second)
                {
                    first = first->next;
                    second = second->next;
                }
                return first;
            }
        }

        return 0;
    }
};

*******************************************************************
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

## LeetCode 148. 排序链表

![](148.png)

![](148_1.png)

```c++
/*不能用递归，用归并排序
空间复杂度O(1)
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
## LeetCode 21. 合并两个有序链表

![](21.png)

```c++
/*建立虚拟节点，合并节点。
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
## LeetCode 141. 环形链表

![](141.png)

![](141_1.png)

```c++
/*快慢指针，只要有环就一定能追上！！！
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
## LeetCode 147. 对链表进行插入排序

![](https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)

![](147.png)

```c++
/*建立虚拟头结点，指向原链表头部。
然后扫描原链表，对于每个节点v，从前往后扫描结果链表，找到第一个比v大的节点u，将v
插入到u之前。
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
## LeetCode 146. LRU缓存机制 !!!

![](146.png)

![](146_1.png)

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
## LeetCode 138.复制带随机指针的链表

![](138.png)

![](138_1.png)

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;

    Node() {}

    Node(int _val, Node* _next, Node* _random) {
        val = _val;
        next = _next;
        random = _random;
    }
};
*/
class Solution {
public:
    Node *copyRandomList(Node *head) {
        if (!head) return 0;
        unordered_map<Node*, Node*> hash;
        Node *root = new Node(head->val, NULL, NULL);
        hash[head] = root;
        while (head->next)
        {
            if (hash.count(head->next) == 0)
                hash[head->next] = new Node(head->next->val, NULL, NULL);
            hash[head]->next = hash[head->next];

            if (head->random && hash.count(head->random) == 0)
                hash[head->random] = new Node(head->random->val, NULL, NULL);
            hash[head]->random = hash[head->random];

            head = head->next;
        }

        if (head->random && hash.count(head->random) == 0)
            hash[head->random] = new Node(head->random->val, NULL, NULL);
        hash[head]->random = hash[head->random];

        return root;
    }
};
```
