---
typora-root-url: img/
---

## LeetCode 1394. 找出数组中的幸运数

![](1394.png)

```c++
class Solution {
public:
    int findLucky(vector<int>& arr) {
        int res = -1;
        map<int,int> cnt;
        for(int i:arr) cnt[i] ++;
        for(auto p:cnt) if(p.first == p.second) res = p.first;

        return res;
    }
};
```
## LeetCode 1395. 统计作战单位数

![](1395.png)

```c++
class Solution {
public:
    int numTeams(vector<int>& rating) {
        int n = rating.size();
        int ans = 0;
        for(int i = 0;i < n;i ++)
        {
            for(int j = i + 1;j < n;j ++)
            {
                if(rating[i] >= rating[j]) continue;
                for(int k = j + 1;k < n;k ++)
                    if(rating[j] < rating[k]) ans ++;
            }
            for(int j = i + 1;j < n;j ++)
            {
                if(rating[i] <= rating[j]) continue;
                for(int k = j + 1;k < n;k ++)
                    if(rating[j] > rating[k]) ans ++;
            }
        }
        return ans;
    }
};
```

## LeetCode 1396. 设计地铁系统

![](1396.png)

```c++
class UndergroundSystem {
public:
    typedef pair<string,string> PSS;
    map<PSS,double> ma;
    map<PSS,int> cnt;
    unordered_map<int,string> pre;
    unordered_map<int,double> pt;
    UndergroundSystem() {

    }
    
    void checkIn(int id, string stationName, int t) {
        pre[id] = stationName;
        pt[id] = t;
    }
    
    void checkOut(int id, string stationName, int t) {
        if(pre.count(id))
        {
            PSS p(pre[id],stationName);
            ma[p] += t - pt[id];
            cnt[p] ++;
        }
    }
    
    double getAverageTime(string startStation, string endStation) {
        PSS p(startStation,endStation);
        
        return ma[p]/cnt[p];
    }
};

/**
 * Your UndergroundSystem object will be instantiated and called as such:
 * UndergroundSystem* obj = new UndergroundSystem();
 * obj->checkIn(id,stationName,t);
 * obj->checkOut(id,stationName,t);
 * double param_3 = obj->getAverageTime(startStation,endStation);
 */
```

## LeetCode 1397. 找到所有好字符串

![](1392.png)

```c++

```

