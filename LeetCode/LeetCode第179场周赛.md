---
typora-root-url: img/
---
#### LeetCode 1374. 生成每种字符都是奇数个的字符串

![1374](1374.png)

```c++
/*
n是奇数，返回n个a
n是偶数，返回1个b，n-1个a
*/
class Solution {
public:
    string generateTheString(int n) {
        string res;
        if(n % 2 == 0) res += 'b',n --;
        while(n --) res += 'a';
        return res;
    }
};
```
## LeetCode 1375. 灯泡开关 III

![1375](1375.png)

```c++
/*
记录当前打开最大的值 == 扫描个数 就是满足要求了
最大值 = 下标 + 1
*/
class Solution {
public:
    int numTimesAllBlue(vector<int>& light) {
        int res = 0;
        int max = 0;
        for(int i = 0;i < light.size();i ++)
        {
            if(light[i] > max) max = light[i];
            if(max == i + 1) res ++;
        }
        return res;
    }
};
```
## LeetCode 1376. 通知所有员工所需的时间

![1376](1376.png)

```c++
/*
从根节点到叶节点的最长带权路径
两个vector存图
*/
class Solution {
public:
    vector<vector<int>> son;
    int numOfMinutes(int n, int headID, vector<int>& manager, vector<int>& informTime) {
        son = vector<vector<int>>(n);

        for(int i = 0;i < n;i ++)
            if(i != headID)
                son[manager[i]].push_back(i);

        return dfs(headID,informTime);
    }

    int dfs(int u,vector<int>& informTime)
    {
        int res = 0;
        for(auto s : son[u]) res = max(res,dfs(s,informTime));
        return res + informTime[u];
    }
};
```

