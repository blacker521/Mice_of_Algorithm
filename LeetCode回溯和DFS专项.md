---
typora-root-url: img/
---

## LeetCode 17. 电话号码的字母组合

![](17.png)

![](17_1.png)

```c++
class Solution {
public:
    vector<char> digit[10];
    vector<string> res;

    void init() {
        char cur = 'a';
        for (int i = 2; i < 10; i++) {
            for (int j = 0; j < 3; j++)
                digit[i].push_back(cur++);
            if (i == 7 || i == 9)
                digit[i].push_back(cur++);
        }
    }

    void solve(string digits, int d, string cur) {
        if (d == digits.length()) {
            res.push_back(cur);
            return;
        }

        int cur_num = digits[d] - '0';

        for (int i = 0; i < digit[cur_num].size(); i++)
            solve(digits, d + 1, cur + digit[cur_num][i]);
    }

    vector<string> letterCombinations(string digits) {
        if (digits == "")
            return res;
        init();
        solve(digits, 0, "");
        return res;
    }
};
************************************************************************
/*
state{}
for 每个数字
	for c = 当前数字的所有备选字母
		for s = state{}中的所有字符串
			s += c
			将s加入到新的集合中去
*/	
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
##  LeetCode 79.单词搜索

![](79.png)

![](79_1.png)

```c++
/*
1.枚举起点
2.从起点开始，依次搜索下一个点的位置
3.在枚举过程中，要保证和目标单词匹配
*/
class Solution {
public:
    bool exist(vector<vector<char>>& board, string str) {
        for (int i = 0; i < board.size(); i ++ )
            for (int j = 0; j < board[i].size(); j ++ )
                if (dfs(board, str, 0, i, j))
                    return true;
        return false;
    }

    bool dfs(vector<vector<char>> &board, string &str, int u, int x, int y) {
        if (board[x][y] != str[u]) return false;
        if (u == str.size() - 1) return true;
        int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
        char t = board[x][y];
        board[x][y] = '*';
        for (int i = 0; i < 4; i ++ ) {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < board.size() && b >= 0 && b < board[a].size()) {
                if (dfs(board, str, u + 1, a, b)) return true;
            }
        }
        board[x][y] = t;
        return false;
    }
};
********************************************************************
class Solution {
public:
    int n,m;
    int dx[4] = {-1,0,1,0},dy[4] = {0,1,0,-1};
    bool exist(vector<vector<char>>& board, string word) {
        if(board.empty() || board[0].empty()) return false;//矩阵是空的
        n = board.size(),m = board[0].size();

        for(int i = 0;i < n;i ++)   //枚举起点
            for(int j = 0;j < m;j ++)
                if(dfs(board,i,j,word,0))
                    return true;
        return false;
    }
	//x，y是当前走到哪个格子了，word是要找的目标单词，u是目标单词的第几位
    bool dfs(vector<vector<char>> &board,int x,int y,string &word,int u)
    {
        if(board[x][y] != word[u]) return false;
        if(u == word.size() - 1) return true;//所有位都匹配完且成功了

        board[x][y] = '.';
        for(int i = 0;i < 4;i ++) 
        {
            int a = x + dx[i],b = y + dy[i];
            if(a >= 0 && a < n && b >= 0 && b < m)
                if(dfs(board,a,b,word,u + 1))
                    return true;
        }
        board[x][y] = word[u];//回溯，恢复现场
        return false;   
    }
};
```
## LeetCode 46. 全排列

![](46.png)

![](46_1.png)

```c++
/*
两种不同方法:
枚举每个位置上放哪个数


枚举每个数放到哪个位置上
*/
class Solution {
public:
    vector<vector<int>> ans;//所有方案
    vector<bool> st;
    vector<int> path;//当前方案

    vector<vector<int>> permute(vector<int>& nums) {

        for (int i = 0; i < nums.size(); i ++ ) st.push_back(false);//初始化
        dfs(nums, 0);
        return ans;
    }

    void dfs(vector<int> &nums, int u)//u是第几个数
    {
        if (u == nums.size())
        {
            ans.push_back(path);
            return ;
        }

        for (int i = 0; i < nums.size(); i ++ )
            if (!st[i])
            {
                st[i] = true;
                path.push_back(nums[i]);
                dfs(nums, u + 1);
                st[i] = false;
                path.pop_back();
            }
    }
};
```
## LeetCode 47. 全排列 II

![](47.png)

![](47_1.png)

```c++
class Solution {
public:
    vector<bool> st;
    vector<int> path;
    vector<vector<int>> ans;

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        st = vector<bool>(nums.size(), false);
        path = vector<int>(nums.size());
        dfs(nums, 0, 0);
        return ans;
    }
    void dfs(vector<int>& nums, int u, int start)//u表示枚举到了哪个数字，start表示从哪个位置从哪儿开始搜
    {
        if (u == nums.size())
        {
            ans.push_back(path);
            return;
        }

        for (int i = start; i < nums.size(); i ++ )
            if (!st[i])
            {
                st[i] = true;
                path[i] = nums[u];
                if (u + 1 < nums.size() && nums[u + 1] != nums[u])
                    dfs(nums, u + 1, 0);
                else
                    dfs(nums, u + 1, i + 1);
                st[i] = false;
            }
    }
};
```

## LeetCode 78. 子集

![](78.png)

![](78_1.png)

```c++
/*
二进制表示
0~n的每一位上的0、1表示这个数是否被选了！！！

i >> j & 1 ----i的二进制表示的第j位是否为1 ！！！
1 << n -----表示2的n次方！！！
*/
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> res;
        int n = nums.size();
        for (int i = 0; i < (1 << n); i ++ )
        {
            vector<int> temp;
            for (int j = 0; j < n; j ++ )
                if (i >> j & 1) //i的二进制表示的第j位是否为1
                    temp.push_back(nums[j]);
            res.push_back(temp);
        }
        return res;
    }
};
```

## LeetCode 90. 子集 II

![](90.png)

![](90_1.png)

```c++
/*
每个数可多选
*/
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        dfs(0, nums);
        return ans;
    }

    void dfs(int u, vector<int>&nums)
    {
        if (u == nums.size())
        {
            ans.push_back(path);
            return;
        }
        int k = u;//计算一段区间一共有多少个相同的数字
        while (k < nums.size() && nums[k] == nums[u]) k ++ ;
        dfs(k, nums);
        for (int i = u; i < k; i ++ )
        {
            path.push_back(nums[i]);
            dfs(k, nums);
        }
        path.erase(path.end() - (k - u), path.end());//恢复现场
    }
};
*****************************************************************************
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        dfs(nums,0);
        return ans;
    }

    void dfs(vector<int> &nums,int u)
    {
        if(u == nums.size())
        {
            ans.push_back(path);
            return;
        }

        int k = 0;
        while(u + k < nums.size() && nums[u + k] == nums[u]) k ++;

        for(int i = 0; i <= k;i ++)
        {
            dfs(nums,u + k);
            path.push_back(nums[u]);
        }

        for(int i = 0;i <= k;i ++)path.pop_back();

    }
};
```