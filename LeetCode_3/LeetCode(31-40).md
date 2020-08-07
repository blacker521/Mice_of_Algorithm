---
typora-root-url: ./img
---

## 31. 下一个排列

![](/31.png)

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

![](/32.png)

```c++
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> stk;

        int res = 0;
        for(int i = 0,start = -1;i < s.size();i ++)
        {
            if(s[i] == '(') stk.push(i);
            else
            {
                if(stk.size())
                {
                    stk.pop();
                    if(stk.size())res = max(res,i - stk.top());
                    else res = max(res,i - start);
                }
                else
                {
                    start = i;
                }
            }
        }
        return res;
    }
};
```
## 33. 搜索旋转排序数组

![](/33.png)

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if(nums.empty()) return -1;
        int l = 0,r = nums.size() - 1;
        while(l < r)
        {
            int mid = l + r + 1 >> 1;
            if(nums[mid] >= nums[0])l = mid;
            else r = mid - 1;
        }

        if(target >= nums[0])l = 0;
        else l = r + 1,r = nums.size() - 1;
        
        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mid] >= target) r = mid;
            else l = mid + 1;
        }
        if(nums[r] == target) return r;
        return -1;
    }
};
```
## 34. 在排序数组中查找元素的第一个和最后一个位置

![](/34.png)

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

![](/35.png)

```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int l = 0,r = nums.size();
        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mßid] >= target) r = mid;
            else l = mid + 1;
        }
        return l;
    }
};
```
## 36. 有效的数独

![](/36.png)

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

![](/37.png)

```c++
class Solution {
public:
    bool row[9][9],col[9][9],cell[3][3][9];
    
    void solveSudoku(vector<vector<char>>& board) {
        memset(row,0,sizeof row);
        memset(col,0,sizeof col);
        memset(cell,0,sizeof cell);

        for(int i = 0; i < 9 ;i ++ )
            for(int j = 0;j < 9;j ++)
            {
                if(board[i][j] != '.')
                {
                    int t = board[i][j] - '1';
                    row[i][t] = col[j][t] = cell[i / 3][j / 3][t] = true;
                }
            }
        dfs(board,0,0);
    }
    bool dfs(vector<vector<char>> & board,int x,int y)
    {
        if(y == 9) x ++,y = 0;
        if(x == 9) return true;

        if(board[x][y] != '.') return dfs(board,x,y + 1);
        for(int i = 0;i < 9;i ++)
            if(!row[x][i] && !col[y][i] && !cell[x / 3][y / 3][i])
            {
                board[x][y] = '1' + i;
                row[x][i] = col[y][i] = cell[x / 3][y / 3][i] = true;
                if(dfs(board,x,y + 1)) return true;
                board[x][y] = '.';
                row[x][i] = col[y][i] = cell[x / 3][y / 3][i] = false;
            }
        return false;
    }

};
```
## 38. 外观数列

![](/38.png)

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

![](/39.png)

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

![](/40.png)

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