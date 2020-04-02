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
## 1036 Boys vs Girls (25分)

![](1036.png)

![](1036-1.png)

```c++
#include <iostream>

using namespace std;

int main()
{
    int n;
    string girl_name,girl_id;
    int girl_score,boy_score;
    string boy_name,boy_id;
    cin >> n;
    for(int i = 0;i < n;i ++)
    {
        string name,sex,id;
        int score;
        cin >> name >> sex >> id >> score;
        if(sex == "F")
        {
            if(girl_name.empty() || girl_score < score)
            {
                girl_name = name;
                girl_id = id;
                girl_score = score;
            }
        }
        else
        {
            if(boy_name.empty() || boy_score > score)
            {
                boy_name = name;
                boy_id = id;
                boy_score = score;
            }
        }
    }
    if(girl_name.empty()) puts("Absent");
    else cout << girl_name << ' ' << girl_id << endl;
    
    if(boy_name.empty()) puts("Absent");
    else cout << boy_name << ' ' << boy_id << endl;
    
    if(girl_name.size() && boy_name.size()) cout << abs(girl_score - boy_score);
    else cout << "NA" << endl;
    
}
```
## 1050 String Subtraction (20分)

![](1050.png)

```c++
#include <iostream>
#include <unordered_set>

using namespace std;

int main()
{
    string s1,s2;
    getline(cin,s1);
    getline(cin,s2);
    unordered_set<char> hash;
    
    for(auto c:s2) hash.insert(c);
    
    string res;
    for(auto c:s1)
        if(!hash.count(c))
            res += c;
    cout << res << endl;
}
```
## 1071 Speech Patterns (25分)

![](1071.png)

```c++
#include <iostream>
#include <unordered_map>
using namespace std;

bool check(char c)
{
    if(c >= '0' && c <= '9') return true;
    if(c >= 'a' && c <= 'z') return true;
    if(c >= 'A' && c <= 'Z') return true;
    return false;
}
int main()
{ 
    string s;
    getline(cin,s);
    
    unordered_map<string,int> hash;
    
    for(int i = 0;i < s.size();i ++)
        if(check(s[i]))
        {
            string word;
            int j = i;
            while(j < s.size() && check(s[j])) word += tolower(s[j ++]);
            
            hash[word] ++;
            i = j; 
        }
    string word;
    int cnt = -1;
    for(auto item : hash)
        if(item.second > cnt || item.second == cnt && item.first < word)
        {
            word = item.first;
            cnt = item.second;
        }
    cout << word <<' ' << cnt << endl;
}
```