---
typora-root-url: img/
---
#### LeetCode 1380. 矩阵中的幸运数

![1380](1380.png)

```c++
class Solution {
public:
    vector<int> luckyNumbers (vector<vector<int>>& matrix) {
        int n = matrix.size(),m = matrix[0].size();
        vector<int> res;

        for(int i = 0;i < n;i ++)
        {
            for(int j = 0;j < m;j ++)
            {
                bool is_lucky = true;
                for(int k = 0; k < m;k ++)
                {
                    if(matrix[i][j] > matrix[i][k])
                    {
                        is_lucky = false;
                        break;
                    }

                }
                if(is_lucky)
                {
                    for(int k = 0; k < n;k ++)
                        if(matrix[i][j] < matrix[k][j])
                        {
                            is_lucky = false;
                            break;
                        }
                }
                if(is_lucky) res.push_back(matrix[i][j]);
            }
        }
        return res;
    }
};
```
## LeetCode 1381. 设计一个支持增量操作的栈

![1381](1381.png)

```c++
class CustomStack {
public:
    
    vector<int> stk;
    int top;

    CustomStack(int maxSize) {
        stk = vector<int>(maxSize);
        top = 0;
    }
    
    void push(int x) {
       if(top == stk.size()) return;
       stk[top ++ ] = x; 
    }
    
    int pop() {
        if(!top) return -1;
        return stk[-- top];
    }
    
    void increment(int k, int val) {
        for(int i = 0;i < k && i < top;i ++)
            stk[i] += val;
    }
};

/**
 * Your CustomStack object will be instantiated and called as such:
 * CustomStack* obj = new CustomStack(maxSize);
 * obj->push(x);
 * int param_2 = obj->pop();
 * obj->increment(k,val);
 */
```
## LeetCode 1382. 将二叉搜索树变平衡

![1382](1382.png)

```c++
 /*
 中序遍历--->有序数组--->平衡二叉树
 中间节点是根节点，左边递归创建左子树，右边递归创建右子树
节点个数m是奇数时，左右都为(m - 1)/2个，差为0
节点个数m是偶数时，左(m - 1)/2 - 1 右都为(m - 1)/2
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
    vector<TreeNode*> node;
    TreeNode* balanceBST(TreeNode* root) {
        dfs(root); 

        return build(0,node.size() - 1);
    }

    TreeNode * build(int l,int r)
    {
        if(l > r) return NULL;
        int mid = l + r >> 1;
        node[mid]->left = build(l,mid - 1);
        node[mid]->right = build(mid + 1,r);
        return node[mid];
    }
    void dfs(TreeNode* root)
    {
        if(!root) return;
        dfs(root->left);
        node.push_back(root);
        dfs(root->right);
    }
};
```

