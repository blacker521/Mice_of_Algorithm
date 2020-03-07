# *Week1-链表专题*
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
```

## LeetCode 92. 反转链表 II

![](92.png)

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
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        if (m == n) return head;
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

简洁版
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

