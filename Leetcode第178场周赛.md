## LeetCode 1365. How Many Numbers Are Smaller Than the Current Number

![1365](C:\Users\black\Desktop\Acwing\Mice\1365.png)

```c++
class Solution {
public:
    vector<int> smallerNumbersThanCurrent(vector<int>& nums) {
        vector<int> res(nums.size());
        for(int i = 0;i < nums.size();i ++)
        {
            for(int x : nums)
            {
                if(nums[i] > x)
                    res[i] ++;
            }
        }
        return res;
    }

};
```
## LeetCode 1366. Rank Teams by Votes 

![1366](C:\Users\black\Desktop\Acwing\Mice\1366.png)

```c++
class Solution {
public:
    string rankTeams(vector<string>& votes) {
        int n = votes[0].size();
        vector<vector<int>> ranks(26, vector<int>(n + 1));

        for (int i = 0; i < 26; i ++ ) {
            ranks[i][n] = -i;
        }

        for (auto &vote : votes)
            for (int i = 0; i < n; i ++ )
                ranks[vote[i] - 'A'][i] ++ ;

        sort(ranks.begin(), ranks.end(), greater<vector<int>>());

        string res;
        for (int i = 0; i < n; i ++ ) res += -ranks[i][n] + 'A';
        return res;
    }
};
```
## LeetCode 1367. Linked List in Binary Tree

![1367](C:\Users\black\Desktop\Acwing\Mice\1367.png)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
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
    bool isSubPath(ListNode* head, TreeNode* root) {
        if (dfs(head, root)) return true;

        if (!root) return false;
        return isSubPath(head, root->left) || isSubPath(head, root->right);  // 枚举起点
    }

    bool dfs(ListNode* cur, TreeNode* root) {  // 起点已经固定，匹配的过程
        if (!cur) return true;
        if (!root) return false;

        if (cur->val != root->val) return false;

        return dfs(cur->next, root->left) || dfs(cur->next, root->right);
    }
};
```