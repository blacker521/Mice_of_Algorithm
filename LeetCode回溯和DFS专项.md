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

## LeetCode 216. 组合总和 III

![](216.png)

![](216_1.png)

```c++
/*
依次枚举每个数从哪个位置上选
dfs(枚举到了第几个数字，当前选择的所有数的和，开始枚举的位置)
倒着枚举 
*/

class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;

    vector<vector<int>> combinationSum3(int k, int n) {
        dfs(k, n, 1);
        return ans;
    }

    void dfs(int k, int n, int start)
    {
        if (!k)
        {
            if (!n) ans.push_back(path);
            return;
        }

        for (int i = start; i <= 10 - k; i ++ )
            if (n >= i)
            {
                path.push_back(i);
                dfs(k - 1, n - i, i + 1);
                path.pop_back();
            }
    }
};
*********************************************************************
class Solution {
public:
    vector<vector<int>> ans;
    vector<int> path;
    vector<vector<int>> combinationSum3(int k, int n) {
        dfs(k,1,n);
        return ans;
    }

    void dfs(int k,int start,int n)
    {
        if(!k)//枚举完所有数
        {
            if(!n) ans.push_back(path);//总和也是倒着来
            return;
        }

        for(int i = start;i <= 9;i ++)
        {
            path.push_back(i);
            dfs(k - 1,i + 1,n - i);
            path.pop_back();
        }
    }
};
```

## LeetCode 52. N皇后 II

![](52.png)

```c++
//暴力
class Solution {
public:
    int ans;
    vector<bool> row, col, diag, anti_diag;

    int totalNQueens(int n) {
        row = col = vector<bool>(n, false);
        diag = anti_diag = vector<bool>(2 * n, false);
        ans = 0;
        dfs(0, 0, 0, n);
        return ans;
    }

    void dfs(int x, int y, int s, int n)
    {
        if (y == n) x ++ , y = 0;
        if (x == n)
        {
            if (s == n) ++ ans;
            return ;
        }

        dfs(x, y + 1, s, n);
        if (!row[x] && !col[y] && !diag[x + y] && !anti_diag[n - 1 - x + y])
        {
            row[x] = col[y] = diag[x + y] = anti_diag[n - 1 - x + y] = true;
            dfs(x, y + 1, s + 1, n);
            row[x] = col[y] = diag[x + y] = anti_diag[n - 1 - x + y] = false;
        }
    }
};
**************************************************************************************
/*
依次枚举每一行皇后的位置，且不与上面冲突
精确覆盖问题，Dancing Link算法
*/
class Solution {
public:

    int ans = 0,n;
    vector<bool> col,d,ud;
    int totalNQueens(int _n) {
        n = _n;//把n做成全局变量
        col = vector<bool>(n);
        d = ud = vector<bool>(n * 2);
        dfs(0); 

        return ans;
    }
    
    void dfs(int u)
    {
        if(u == n )//找到了一个方案
        {
            ans ++;
            return;
        }

        for(int i = 0;i < n;i ++)
            if(!col[i] && !d[u + i] && !ud[u - i + n])
            {
                col[i] = d[u + i] = ud[u - i + n] = true;
                dfs(u + 1);
                col[i] = d[u + i] = ud[u - i + n] = false;
            }
    }
};
```

## LeetCode 37. 解数独

![](37.png)

![](37_1.png)

```c++
class Solution {
public:
    bool dfs(int x, int y, vector<vector<char>>& board,
            vector<int>& row, vector<int>& col, vector<int>& squ) {
        if (y == 9) {
            x++;
            y = 0;
        }
        if (x == 9)
            return true;
        if (board[x][y] == '.') {
            for (int i = 1; i <= 9; i++)
                if (! (
                    (row[x] & (1 << i)) ||
                    (col[y] & (1 << i)) ||
                    (squ[(x / 3) * 3 + (y / 3)] & (1 << i))
                    )
                   ) {
                    row[x] |= (1 << i);
                    col[y] |= (1 << i);
                    squ[(x / 3) * 3 + (y / 3)] |= (1 << i);
                    board[x][y] = i + '0';
                    if(dfs(x, y + 1, board, row, col, squ))
                        return true;
                    board[x][y] = '.';
                    row[x] -= (1 << i);
                    col[y] -= (1 << i);
                    squ[(x / 3) * 3 + (y / 3)] -= (1 << i);
                }
        }
        else {
            if (dfs(x, y + 1, board, row, col, squ))
                return true;
        }
        return false;
    }

    void solveSudoku(vector<vector<char>>& board) {

        vector<int> row(9), col(9), squ(9);
        for (int i = 0; i < 9; i++)
            for (int j = 0; j < 9; j++) {
                if (board[i][j] == '.')
                    continue;
                int num = board[i][j] - '0';
                row[i] |= (1 << num);
                col[j] |= (1 << num);
                squ[(i / 3) * 3 + (j / 3)] |= (1 << num);
            }

        dfs(0, 0, board, row, col, squ);
    }
};
************************************************
/*从前往后枚举每个空格该填哪个数
状态：row[9][9],col[9][9],cell[3][3][9]
精确覆盖问题，Dancing Links算法
*/    
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

    bool dfs(vector<vector<char>> &board,int x,int y)//board是引用类型
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

## LeetCode 473. 火柴拼正方形

![](473.png)

![](473_1.png)

```c++
bool cmp(int x,int y){
    return x>y;
}
class Solution {
public:
    bool makesquare(vector<int>& nums) {
        if (nums.size() == 0) return false;
        int sum = 0;
        for (int i = 0;i<nums.size();++i){
            sum += nums[i];
        }
        int len = sum/4;
        if (len*4 != sum)
            return false;


        sort(nums.begin(),nums.end(),cmp);
        vector<int> SubSum(4);
        bool res = dfs(nums,SubSum,len,0);
        return res;

    }
    bool dfs(vector<int>&nums,vector<int>SubSum,int len,int index){
        if (SubSum[0] == len && SubSum[1] == len && SubSum[2] == len){
            return true;
        }
        for (int i = 0;i<4;++i){
            if (SubSum[i]+nums[index]> len) continue;
            SubSum[i] += nums[index];
            if (dfs(nums,SubSum,len,index+1)) return true;
            SubSum[i] -= nums[index];
        }
        return false;


    }
};
**************************************************************************
/*
依次构造正方形的每条边
剪枝：
1.从大到小枚举所有边
2.每条内部的木棒长度规定成从大到小
3.如果当前木棒拼接失败，则跳过接下来所有长度相同的木棒
4.如果当前木棒拼接失败，且是当前边的第一个，则直接减掉当前分支
5.如果当前木棒拼接失败，且是当前边的最后一个，则直接减掉当前分支
*/
class Solution {
public:
    vector<bool> st;

    bool makesquare(vector<int>& nums) {
        int sum = 0;
        for(auto u : nums) sum += u;

        if(!sum || sum % 4) return false;

        sort(nums.begin(),nums.end());//第一、二个剪枝
        reverse(nums.begin(),nums.end());

        st = vector<bool>(nums.size());
        return dfs(nums,0,0,sum / 4);
    }

    bool dfs(vector<int> &nums,int u, int cur,int length)
    {
        if(cur == length) u ++,cur = 0;
        if(u == 4) return true;

        for(int i = 0;i < nums.size();i ++)
        {
            if(!st[i] && cur + nums[i] <= length)
            {
                st[i] = true;
                if(dfs(nums,u,cur + nums[i],length)) return true;
                st[i] = false;
                if(!cur) return false;//第四个剪枝
                if(cur + nums[i] == length) return false;//第五个剪枝
                while(i + 1 < nums.size() && nums[i + 1] == nums[i]) i ++;//第三个剪枝
            }

        }
    return false;
    }
};
```