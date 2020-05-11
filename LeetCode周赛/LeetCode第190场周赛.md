---
typora-root-url: img/
---

## LeetCode 1436. 旅行终点站

![](1436.png)

```c++
class Solution {
public:
    string destCity(vector<vector<string>>& paths) {
        unordered_set<string> s;

        for (const auto &v : paths)
            s.insert(v[0]);

        for (const auto &v : paths)
            if (s.find(v[1]) == s.end())
                return v[1];

        return "";
    }
};
```
## 1437. 是否所有 1 都至少相隔 k 个元素

![](1437.png)

```c++
class Solution {
public:
    bool kLengthApart(vector<int>& nums, int k) {
        int n = nums.size();
        int last = -1;
        for (int i = 0; i < n; i++)
            if (nums[i] == 1) {
                if (last != -1 && i - last <= k)
                    return false;
                last = i;
            }

        return true;
    }
};
```

## 1438. 绝对差不超过限制的最长连续子数组

![](1438.png)

```c++
class Solution {
public:
    int longestSubarray(vector<int>& nums, int limit) {
        int n = nums.size();
        deque<int> m1, m2;

        int ans = 0;
        for (int i = 0, j = 0; i < n; i++) {
            while (!m1.empty() && nums[i] >= nums[m1.back()])
                m1.pop_back();

            m1.push_back(i);

            while (!m2.empty() && nums[i] <= nums[m2.back()])
                m2.pop_back();

            m2.push_back(i);

            while (!m1.empty() && !m2.empty() 
                   && nums[m1.front()] - nums[m2.front()] > limit) {
                j++;
                while (!m1.empty() && m1.front() < j)
                    m1.pop_front();

                while (!m2.empty() && m2.front() < j)
                    m2.pop_front();
            }

            ans = max(ans, i - j + 1);
        }

        return ans;
    }
};
```

## 1439. 有序矩阵中的第 k 个最小数组和

![](1439.png)

```c++
class Solution {
public:
    int kthSmallest(vector<vector<int>>& mat, int k) {
        int m = mat.size();
        int n = mat[0].size();

        #define PIV pair<int, vector<int>>

        priority_queue<PIV, vector<PIV>, greater<PIV>> q;
        set<vector<int>> vis;

        vector<int> idx(m, 0);
        int sum = 0;

        for (int i = 0; i < m; i++)
            sum += mat[i][0];

        q.push(make_pair(sum, idx));

        while (--k) {
            sum = q.top().first;
            idx = q.top().second;

            q.pop();

            for (int i = 0; i < m; i++) {
                int j = idx[i];
                if (j + 1 < n) {
                    idx[i]++;
                    if (vis.find(idx) == vis.end()) {
                        vis.insert(idx);
                        q.push(make_pair(sum - mat[i][j] + mat[i][j + 1], idx));
                    }
                    idx[i]--;
                }
            }
        }

        return q.top().first;
    }
};
```