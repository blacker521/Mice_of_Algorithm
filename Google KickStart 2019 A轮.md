## [AcWing 549. 训练](https://www.acwing.com/problem/content/551/)

作为一名学校足球教练，你的任务是挑选一支由P个学生组成的团队代表你的学校。

共有N名学生供你挑选，第 i 名学生的技术等级为Si，这是一个正整数，表示他们的技术水平。

在你看来一个合理的团队中的P个球员的技术应该是相当的，这样才能使每个人都融入到队内。

在最开始，你可能无法直接选出一个配置合理的队伍，因此你将为一些学生提供一对一的辅导。

将一名学生的技术等级提高1需要你花费1个小时的时间来进行辅导。

比赛季很快就开始了（事实上，第一场比赛已经开始了！），所以你想知道训练出一个合理的团队，你需要提供的最少训练小时数是多少。

#### 输入格式

第一行包含整数T，表示共有T组测试数据。

每组测试数据的第一行包含两个整数N和P。

第二行包含N个整数Si，其中第 i 个整数为第 i 个学生的技术等级。

#### 输出格式

每组数据输出一个结果，每个结果占一行。

结果表示为“Case #x: y”，其中x为组别编号（从1开始），y为所需的最少训练小时数。

#### 数据范围

*1≤T≤100,*
*1≤Si≤10000,*
*2≤P≤N,*
*2≤N≤105*

#### 输入样例：

```
3
4 3
3 1 9 100
6 2
5 5 1 2 3 4
5 5
7 7 1 7 7
```

#### 输出样例：

```
Case #1: 14
Case #2: 0
Case #3: 6
```

#### 样例解释

在样例＃1中，你可以花费总共6个小时训练第一个学生，8个小时训练第二个学生，使得前三个学生的技能水平同为9。这是你可以花费的最短时间，因此答案是14。

在样例＃2中，你可以选择直接选择前两个学生而无需进行任何指导，因此答案为0。

在样例＃3中，P = N，因此每个学生都将加入您的团队。 你必须花6个小时训练第三个学生，这样所有人的技术等级都为7。 这是你可以花费的最短时间，所以答案是6。

#### 算法答案

```c++
#include <iostream>
#include <algorithm>
#include <limits.h>

using namespace std;

const int N = 100010;

int n, p;
int skill[N], sum[N];

int main()
{
    int T;
    cin >> T;
    for (int C = 1; C <= T; C ++ )
    {
        cin >> n >> p;
        for (int i = 1; i <= n; i ++ ) cin >> skill[i];
        sort(skill + 1, skill + 1 + n);
        for (int i = 1; i <= n; i ++ ) sum[i] = sum[i - 1] + skill[i];

        int res = INT_MAX;
        for (int i = p; i <= n; i ++ )
            res = min(res, p * skill[i] - (sum[i] - sum[i - p]));

        printf("Case #%d: %d\n", C, res);
    }
    return 0;
}
```
## [AcWing 550. 包裹](https://www.acwing.com/problem/content/552/)

恭喜你被聘为著名包裹投递公司的首席决策者（CDM）。

客户喜欢更快的物流，因此你决定在全球范围内降低包裹的运输时间，从而吸引更多客户。

你已经向当局介绍了这个想法，他们已经为你分配了足够的预算来建立一个新的运输处。

我们将世界划分为一个R×C的方格矩阵。

每个方格内或者存在一个运输处，或者没有。

你可以选择一个还未建立运输处的方格，在上面建立新的运输处。

如果某方格包含运输处，则包裹到该方格的运输时间为0。

否则，该方格的运输时间被定义为该方格与包含运输处的任何其他方格之间的最小曼哈顿距离。

总运输时间是所有方格的运输时间的最大值。

在你有权利建立一个新的运输处（最多一个）的情况下，总运输时间最短是多少？

注意：两个方格（r1，c1）和（r2，c2）之间的曼哈顿距离定义为| r1 - r2 | + | c1 - c2 |，其中| * |运算符表示绝对值。

#### 输入格式

第一行包含整数T，表示共有T组测试数据。

每组测试数据第一行包含两个整数R和C，分别表示矩阵的行数和列数。

接下来R行，每行包含一个长度为C的只包含0或1的字符串，其中0表示没有运输处，1表示有运输处。

#### 输出格式

每组数据输出一个结果，每个结果占一行。

结果表示为“Case #x: y”，其中x为组别编号（从1开始），y为最小总运输时间。

#### 数据范围

1≤T≤1001≤T≤100,
1≤R,C≤2501≤R,C≤250,
数据保证至少存在一个运输处

#### 输入样例：

```
3
3 3
101
000
101
1 2
11
5 5
10001
00000
00000
00000
10001
```

#### 输出样例：

```
Case #1: 1
Case #2: 0
Case #3: 2
```

#### 样例解释

在样例＃1中，通过在没有运输处的五个方格中的任何一个中建立新的运输处，你就可以获得最小总运输时间1。

在样例＃2中，所有方格都已经有一个运输处，因此最小总运输时间为0。请注意，你可以选择不建立新的运输处。

在样例＃3中，要获得最小的总运输时间2，你可以在以下任何方格中建立新的运输处：(2,3),(3,2),(3,3),(3,4),(4,3)。

#### 算法答案

```c++
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>
#include <limits.h>

using namespace std;

const int N = 255;

int n, m;
string g[N];
int dist[N][N];

void bfs(int k)
{
    queue<pair<int, int>> q;
    memset(dist, -1, sizeof dist);
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            if (g[i][j] == '1')
            {
                dist[i][j] = 0;
                q.push({i, j});
            }

    int dx[4] = {-1, 0, 1, 0, }, dy[4] = {0, 1, 0, -1};
    while (q.size())
    {
        auto t = q.front();
        q.pop();
        int x = t.first, y = t.second;
        int distance = dist[x][y];
        if (distance == k) continue;
        for (int i = 0; i < 4; i ++ )
        {
            int a = x + dx[i], b = y + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && dist[a][b] == -1)
            {
                dist[a][b] = distance + 1;
                q.push({a, b});
            }
        }
    }
}

bool check(int k)
{
    bfs(k);

    int min_sum = INT_MAX, max_sum = INT_MIN;
    int min_sub = INT_MAX, max_sub = INT_MIN;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            if (dist[i][j] == -1)
            {
                min_sum = min(min_sum, i + j);
                max_sum = max(max_sum, i + j);
                min_sub = min(min_sub, i - j);
                max_sub = max(max_sub, i - j);
            }

    if (min_sum == INT_MAX) return true;

    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            if (g[i][j] == '0')
            {
                int sum = max(abs(i + j - min_sum), abs(i + j - max_sum));
                int sub = max(abs(i - j - min_sub), abs(i - j - max_sub));
                if (max(sum, sub) <= k) return true;
            }

    return false;
}

int main()
{
    int T;
    cin >> T;
    for (int C = 1; C <= T; C ++ )
    {
        cin >> n >> m;
        for (int i = 0; i < n; i ++ ) cin >> g[i];

        int l = 0, r = n + m;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (check(mid)) r = mid;
            else l = mid + 1;
        }

        printf("Case #%d: %d\n", C, r);
    }

    return 0;
}
```
## [AcWing 551. 抢票](https://www.acwing.com/problem/content/553/)

你正在出售电影院前排座位的门票。

前排共有N个座位，从左到右编号为1到N。

短短一个周末的时间，就堆积了关于座位的Q个预订请求！

第 i 个预订要求订下从编号LiLi到RiRi的所有座位。

你现在将每个预订输入系统，一次输入一个。

由于某些预订可能会重叠，因此系统可能无法完全满足每个预订。

当你在系统中输入一个预订时，系统会将该预订所请求的座位里所有还未进行分配的座位分配给该预订。

现在要求每个订单都至少分配k个座位，请问k最大是多少。

#### 输入格式

第一行包含整数T，表示共有T组测试数据。

每组测试数据第一行包含两个整数N和Q。

接下来Q行，每行包含两个整数LiLi和RiRi。

#### 输出格式

每组数据输出一个结果，每个结果占一行。

结果表示为“Case #x: y”，其中x为组别编号（从1开始），y为k的最大值。

#### 数据范围

1≤T≤1001≤T≤100,
1≤N≤1061≤N≤106,
1≤Li≤Ri≤N1≤Li≤Ri≤N,
1≤Q≤300001≤Q≤30000

#### 输入样例：

```
3
5 3
1 2
3 4
2 5
30 3
10 11
10 10
11 11
10 4
1 8
4 5
3 6
2 7
```

#### 输出样例：

```
Case #1: 1
Case #2: 0
Case #3: 2
```

#### 样例解释

在样例＃1中，有N = 5个席位和Q = 3个预订。一个可能的顺序是：

在第二次预订中，系统分配其2个座位（3和4）。
在第一次预订中，系统分配其2个座位（1和2）。
在第三次预订中，系统分配其1个座位（仅限座位5，因为座位1,2,3和4已经预订）。

每个预订至少分配1个座位，且不存在至少2个座位的分配方式，答案是1。

在样本案例＃2中，有N = 30个席位和Q = 3个预订。无论你如何分配座位，至少有一个预订将没有座位可分。所以答案是0.请注意，可以有座位不属于任何预订！

在样本案例＃3中，有N = 10个席位且Q = 4个预订。一个可能的顺序是：

在第二次预订中，系统分配其2个座位（4和5）。
在第三次预订中，系统分配其2个座位（3和6，因为4和5已经预订）。请注意，预订的座位不一定彼此相邻。
在第四次预订中，系统分配其2个座位（2和7）。
在第一次预订时，系统分配其2个座位（1和8）。

每个预订至少分配2个座位，并且不存在至少3个座位的分配方式，因此答案是2。

#### 算法答案

```c++
#include <iostream>
#include <algorithm>
#include <fstream>
#include <vector>
#include <deque>
#include <assert.h>
#include <queue>
#include <stack>
#include <set>
#include <map>
#include <stdio.h>
#include <string.h>
#include <utility>
#include <math.h>
#include <bitset>
#include <iomanip>
#include <complex>

using namespace std;

#define rep(i, a, b) for (int i = (a), i##_end_ = (b); i < i##_end_; ++i)
#define debug(...) fprintf(stderr, __VA_ARGS__)
#define mp make_pair
#define x first
#define y second
#define pb push_back
#define SZ(x) (int((x).size()))
#define ALL(x) (x).begin(), (x).end()

template<typename T> inline bool chkmin(T &a, const T &b) { return a > b ? a = b, 1 : 0; }
template<typename T> inline bool chkmax(T &a, const T &b) { return a < b ? a = b, 1 : 0; }
template<typename T> inline bool smin(T &a, const T &b)   { return a > b ? a = b : a;    }
template<typename T> inline bool smax(T &a, const T &b)   { return a < b ? a = b : a;    }

typedef long long LL;

const int N = (int) 1e6 + 6, mod = (int) 0;
int n, q, xl[N], xr[N], mvl[N];
pair<int, int> sr[N];
int check(int k) {
    for (int j = 0; j < q; ++j)
        xl[j] = sr[j].first, xr[j] = -sr[j].second, mvl[j] = xl[j];
    for (int j = 0; j < q; ++j) {
        int l = xl[j], r = xr[j];   
        int st = mvl[j];
        int allowed_after = r;
        int cnt = 0;
        for (int i = j + 1; i < q; ++i) {
            if (xr[i] <= r) {
                if (xl[i] <= st) {
                    st = max(st, xr[i]);    
                } else {
                    cnt += xl[i] - st;
                    st = max(st, xr[i]);
                    if (cnt >= k) {
                        allowed_after = xl[i] - (cnt - k);
                        break;
                    }
                }
            }
        }
        if (cnt < k) {
            cnt += r - st;
            if (cnt < k) return 0;
            allowed_after = r - (cnt - k);
        }
        for (int i = j + 1; i < q; ++i) {
            if (xl[i] >= allowed_after) break;
            if (xr[i] > r) {
                mvl[i] = max(mvl[i], r);
            }
        }
    }
    return 1;
}
int main() {
    int tc;
    cin >> tc;
    for (int tt = 1; tt <= tc; ++tt) {
        cout << "Case #" << tt << ": ";
        cin >> n >> q;
        for (int j = 0; j < q; ++j) {
            cin >> xl[j] >> xr[j], --xl[j];
            sr[j] = mp(xl[j], -xr[j]);  
        }
        sort(sr, sr + q);
        int bl = 0, br = n + 1;
        while (bl < br - 1) {
            int bm = bl + br >> 1;
            if (check(bm)) {
                bl = bm;
            } else {
                br = bm;
            }
        }
        cout << bl << '\n';

    }
}
```

