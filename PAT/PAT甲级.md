---
typora-root-url: img_Advanced Level/
---

## 1001 A+B Format (20分)

![](1001.png)
```c++
/*
把int变成string用to_string(s)
-'0'变成数字；
+'0'变成字符；
string在前面加字符，用ans = s[i] + ans;用ans += s[i]是在后面加！！！
*/
#include <iostream>
#include <string>

using namespace std;

int main()
{
    int a,b;
    cin >> a >> b;
    int res = a + b;
    string s = to_string(res);
    string ans;
    for(int i = s.size() - 1,j = 0;i >= 0;i --)
    {
        ans = s[i] + ans;
        ++ j;
        if(j % 3 == 0 && i && s[i - 1] != '-') ans = ',' + ans;
    }
    cout << ans;
}
```
## 1002 A+B for Polynomials (25分)

![](1002.png)

```c++


```

## 1005 Spell It Right (20分)

![](1005.png)

```c++
#include <iostream>

using namespace std;

string s[] = {"zero","one","two","three","four","five","six","seven","eight","nine"};
int main()
{
    string n;
    int ans;
    cin >> n;
    for(auto c:n) ans += (c - '0')% 10;
    
    string res = to_string(ans);
    
    cout << s[res[0] - '0'];
    for(int i = 1;i < res.size();i ++) cout << ' ' << s[res[i] - '0'];
}
```

## 1006 Sign In and Sign Out (25分)

![](1006.png)

```c++
/*
时间位数相同可以直接比较字典序！！！
*/
#include <iostream>

using namespace std;

int main()
{
    string open_id,open_time;
    string close_id,close_time;
    int n;
    cin >> n;
    for(int i = 0;i < n;i ++)
    {
        string id,in_time,out_time;
        cin >> id >> in_time >> out_time;
        
        if(!i || in_time < open_time) open_time = in_time,open_id = id;
        if(!i || out_time > close_time) close_time = out_time,close_id = id;
    }
    cout << open_id << ' ' << close_id;

}
```

## 1035 Password (20分)

![](1035-1.png)

![](1035-2.png)

```c++
#include <iostream>
#include <string>

using namespace std;

const int N = 1010;

string id[N],code[N];

void change(string &str)
{
    for(auto &c:str)
    {
        if(c == '1') c = '@';
        if(c == '0') c = '%';
        if(c == 'l') c = 'L';
        if(c == 'O') c = 'o';
    }
}
int main()
{
    int n;
    cin >> n;
    int m = 0;
    for(int i = 0;i < n;i ++)
    {
        string cur_id,cur_code;
        cin >> cur_id >> cur_code;
        string changed_code = cur_code;
        change(changed_code);
        
        if(changed_code != cur_code)
        {
            id[m] = cur_id;
            code[m] = changed_code;
            m ++;
        }
    }
    if(!m)
    {
        if(n == 1) puts("There is 1 account and no account is modified");
        else printf("There are %d accounts and no account is modified",n);
    }
    else
    {
        cout << m <<endl;
        for(int i = 0;i < m;i ++) cout << id[i] << ' ' << code[i] << endl;
    }
}
```