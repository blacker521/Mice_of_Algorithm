---
typora-root-url: img
---

## 1480. 一维数组的动态和			//前缀和

![](/1480.png)

```c++
class Solution {
public:
    vector<int> runningSum(vector<int>& nums) {
        vector<int> sum(nums.size());
        sum[0] = nums[0];
        for(int i = 1;i < nums.size();i ++)
            sum[i] = sum[i - 1] + nums[i];
        return sum;
    }
};
```
## 1481. 不同整数的最少数目

![](/1481.png)

```c++
class Solution {
public:
    int findLeastNumOfUniqueInts(vector<int>& arr, int k) {
        unordered_map<int,int> hash;
        for(auto x :arr) hash[x] ++;

        vector<int> cnt;
        for(auto x : hash) cnt.push_back(x.second);
        sort(cnt.begin(),cnt.end());

        int res = cnt.size();
        for(auto c : cnt)
            if(k >= c)
            {
                k -= c;
                res --;
            }
            else
            {
                break;
            }

        return res;
    }
};
```

## 1482. 制作 m 束花所需的最少天数

![](/1482.png)

```c++
class Solution {
public:

    int get(int l, int r, int k) {
        return (r - l + 1) / k;
    }

    int minDays(vector<int>& bs, int m, int k) {
        vector<pair<int, int>> q;
        int n = bs.size();
        for (int i = 0; i < bs.size(); i ++ ) q.push_back({bs[i], i + 1});

        vector<int> l(n + 2), r(n + 2);
        sort(q.begin(), q.end());
        int sum = 0;
        for (auto x : q) {
            int i = x.second;
            if (l[i - 1] && r[i + 1]) {
                sum = sum - get(l[i - 1], i - 1, k) - get(i + 1, r[i + 1], k) + get(l[i - 1], r[i + 1], k);
                r[l[i - 1]] = r[i + 1];
                l[r[i + 1]] = l[i - 1];
            } else if (l[i - 1]) {
                sum = sum - get(l[i - 1], i - 1, k) + get(l[i - 1], i, k);
                r[l[i - 1]] = i;
                l[i] = l[i - 1];
            } else if (r[i + 1]) {
                sum = sum - get(i + 1, r[i + 1], k) + get(i, r[i + 1], k);
                r[i] = r[i + 1];
                l[r[i + 1]] = i;
            } else {
                sum = sum + get(i, i, k);
                r[i] = l[i] = i;
            }

            if (sum >= m) return x.first;
        }

        return -1;
    }
};
```

## 1483. 树节点的第 K 个祖先

![](/1483.png)