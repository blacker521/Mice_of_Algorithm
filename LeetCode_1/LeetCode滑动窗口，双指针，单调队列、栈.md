---
typora-root-url: img/
---

## LeetCode 167. 两数之和 II - 输入有序数组

![](167.png)

![](167_1.png)

```c++
/*
双指针算法：先想暴力，如果有单调性，就可以优化
暴力：两重循环，暴力枚举
双指针：i后移1位，j从后往前移动
*/
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        for (int i = 0, j = numbers.size() - 1; i < j; i ++ )
        {
            while (numbers[j] + numbers[i] > target) j -- ;
            if (numbers[j] + numbers[i] == target)
            {
                vector<int> res;
                res.push_back(i + 1), res.push_back(j + 1);
                return res;
            }
        }
        return {-1，-1};
    }
};
**********************************************************************
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        for(int i = 0,j = numbers.size() - 1;i < numbers.size();i ++)
        {
            while(numbers[i] + numbers[j] > target) j --;
            if(numbers[i] + numbers[j] == target) return {i + 1,j + 1};
        }
        return {-1,-1};
    }
};
```

## LeetCode 88. 合并两个有序数组

![](88.png)

![](88_1.png)

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int p = m - 1 , q = n - 1 , cur = m + n - 1 ;
        while(p>= 0 && q >= 0){
            nums1[cur--] = ( nums1[p] >= nums2[q] ? nums1[p--] : nums2[q--]);
        }
        while(q >= 0){
            nums1[cur--] = nums2[q--];
        }
    }
};
*****************************************************************************
//把两数组的中的较大值放入cur，指针都往前走一步
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int p = m - 1,q = n - 1,cur = m + n - 1;
        while(p >= 0 && q >= 0)
        {
            if(nums1[p] >= nums2[q]) nums1[cur --] = nums1[p --];
            else nums1[cur --] = nums2[q --];
        }
        while(p >= 0) nums1[cur --] = nums1[p --];
        while(q >= 0) nums1[cur --] = nums2[q --];
    }
};
```

## LeetCode 26. 删除排序数组中的重复项

![](26.png)

![](26_1.png)

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.size() == 0)
            return 0;

        int k = 0;
        for (int i = 1; i < nums.size(); i++)
            if (nums[i] != nums[k])
                nums[++k] = nums[i];

        return k + 1;
    }
};
********************************************************************
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.empty()) return 0;
        int k = 1;
        for(int i = 1;i < nums.size();i ++)
        {
            if(nums[i] != nums[i - 1])
                nums[k ++] = nums[i];
        }
        return k;
    }
};

```

## LeetCode 76. 最小覆盖子串 !!!

![](76.png)

![](76_1.png)

```c++
/*
滑动窗口
暴力：从前往后枚举终点，在往前走枚举子串中的字母，开一个hash，进行匹配
滑动窗口：i变大j一定不会变小
*/
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char, int> hash;//T中每个字母出现次数
        int cnt = 0;
        for (auto c : t)
        {
            if (!hash[c]) cnt ++ ;
            hash[c] ++ ;
        }

        string res = "";
        for (int i = 0, j = 0, c = 0; i < s.size(); i ++ )
        {
            if (hash[s[i]] == 1) c ++ ;
            hash[s[i]] -- ;//s[i]已经出现
            while (c == cnt && hash[s[j]] < 0) hash[s[j ++ ]] ++ ;
            if (c == cnt)
            {
                if (res.empty() || res.size() > i - j + 1) res = s.substr(j, i - j + 1);
            }
        }

        return res;
    }
};
***************************************************************************
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char,int> hash;

        for(auto c : t) hash[c] ++;//T中每个字母出现次数
        int cnt = hash.size();//不同字母的个数

        string res;
        for(int i = 0,j = 0,c = 0;i < s.size();i ++)//维护窗口
        {
            if(hash[s[i]] == 1) c ++;//c为已经存在字母的数
            hash[s[i]] --;//新加了字母
            while(hash[s[j]] < 0) hash[s[j ++ ]] ++;
            if(c == cnt)
            {
                if(res.empty() || res.size() > i - j + 1) res = s.substr(j,i - j + 1);
            }
        }
        return res;
    }
};
```

## LeetCode 32. 最长有效括号

![](32.png)

![](32_1.png)

```c++
class Solution {
public:
    int longestValidParentheses(string s) {
        int n = s.length();
        int start = 0, val = 0, ans = 0;
        for (int i = 0; i < n; i++) {
            if (s[i] == '(') val++;
            else val--;
            if (val < 0) {
                val = 0;
                start = i + 1;
            }
            else if (val == 0)
                ans = max(ans, i - start + 1);
        }

        start = n - 1; val = 0;
        for (int i = n - 1; i >= 0; i--) {
            if (s[i] == ')') val++;
            else val--;
            if (val < 0) {
                val = 0;
                start = i - 1;
            }
            else if (val == 0)
                ans = max(ans, start - i + 1);
        }
        return ans;
    }
};
***************************************************************************************
/*
左括号 = 1
右括号 = -1
括号序列合法 === 所有前缀和 >= 0,且总和等于0 ！！！！
每一个括号匹配的括号是一定

start点前枚举这一段的开头
cnt前缀和
（ = 1
） = -1
1.cnt[i] < 0 -----start = i + 1,cnt = 0只要不匹配直接跳过所有的
2.cnt[i] > 0 -----继续做
3.cnt[i] = 0 -----[start,i]是一段合法的括号序列
从左往右做一遍再从右往左做一遍
*/
class Solution {
public:

    int work(string s)
    {
        int res = 0;
        for(int i = 0,start = 0,cnt = 0;i <s.size();i ++)
            if(s[i] == '(') cnt ++;
            else
            {
                cnt --;
                if(cnt < 0) start  = i + 1,cnt = 0;
                else if(!cnt) res = max(res,i - start + 1);
            }
        return res;
    }
    int longestValidParentheses(string s) {
        int res = work(s);
        reverse(s.begin(),s.end());//翻转一下
        for(auto &c : s) c ^= 1;//左括号变右括号，右括号变左括号
        return max(res,work(s));
    }
};
```

![](32_2.png)

```c++
class Solution {
public:
    int longestValidParentheses(string s) {
        int n = s.length();
        int start = 0, val = 0, ans = 0;
        for (int i = 0; i < n; i++) {
            if (s[i] == '(') val++;
            else val--;
            if (val < 0) {
                val = 0;
                start = i + 1;
            }
            else if (val == 0)
                ans = max(ans, i - start + 1);
        }

        start = n - 1; val = 0;
        for (int i = n - 1; i >= 0; i--) {
            if (s[i] == ')') val++;
            else val--;
            if (val < 0) {
                val = 0;
                start = i - 1;
            }
            else if (val == 0)
                ans = max(ans, start - i + 1);
        }
        return ans;
    }
};
```
![](32_3.png)

```c++
class Solution {
public:
    int longestValidParentheses(string s) {
        int n = s.length();
        stack<int> st;

        int start = 0, ans = 0;
        for (int i = 0; i < n; i++) {
            if (s[i] == '(')
                st.push(i);
            else {
                if (!st.empty()) {
                    st.pop();
                    if (st.empty())
                        ans = max(ans, i - start + 1);
                    else
                        ans = max(ans, i - st.top());
                } else {
                    start = i + 1;
                }
            }
        }

        return ans;
    }
};
```

## LeetCode 155.最小栈

![](155.png)

![](155_1.png)

```c++
/*
前缀和
stack:-2  0  3
stk_min:前i个数的最小值
*/
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int> stackValue;
    stack<int> stackMin;
    MinStack() {

    }

    void push(int x) {
        stackValue.push(x);
        if (stackMin.empty() || stackMin.top() >= x)
            stackMin.push(x);
    }

    void pop() {
        if (stackMin.top() == stackValue.top()) stackMin.pop();
        stackValue.pop();
    }

    int top() {
        return stackValue.top();
    }

    int getMin() {
        return stackMin.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

## LeetCode 84.柱状图中最大的矩形

![](84.png)

![](84_1.png)

```c++
/*
单调栈：查找每个数左侧第一个比它小的数

如何枚举出所有情况
1.枚举所有柱形的上边界，作为整个矩形的上边界，
然后求出左右边界
	1.找出左边离它最近的比它小的柱形
	2.找出右边离它最近的比它小的柱形
*/
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size(), ans = 0;
        heights.push_back(-1);
        // 为了算法书写方便，在数组末尾添加高度 -1
        // 这会使得栈中所有数字在最后出栈。
        stack<int> st;
        for (int i = 0; i <= n; i++) {
            while (!st.empty() && heights[i] < heights[st.top()]) {
                int cur = st.top();
                st.pop();
                if (st.empty())
                    ans = max(ans, heights[cur] * i);
                else
                    ans = max(ans, heights[cur] * (i - st.top() - 1));
            }
            st.push(i);
        }
        return ans;
    }
};

*****************************************
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        vector<int> left(n),rihgt(n);//左右边界

        stack<int> stk;
        //左边第一个比它小的数
        for(int i = 0;i < n;i ++)
        {
            while(stk.size() && heights[stk.top()] >= heights[i]) stk.pop();
            if(stk.empty()) left[i] = -1;//左边没有一个大于等于它的标记-1
            else left[i] = stk.top();
            stk.push(i);
        }
        while(stk.size()) stk.pop();
        //右边第一个比它小的数
        for(int i = n - 1;i >= 0;i --)
        {
            while(stk.size() && heights[stk.top()] >= heights[i]) stk.pop();
            if(stk.empty()) rihgt[i] = n;//右有一个大于等于它的标记n
            else rihgt[i] = stk.top();
            stk.push(i);
        }

        int res = 0;
        //枚举每个边界，取最大值
        for(int i = 0;i < n;i ++) res = max(res,heights[i] * (rihgt[i] - left[i] - 1));
        return res;
    }
};
```

## LeetCode 42. 接雨水

![](42.png)

![](42_1.png)

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size(), ans = 0;
        if (n == 0)
            return 0;
        vector<int> left_max(n), right_max(n);

        left_max[0] = height[0];
        for (int i = 1; i < n; i++) 
            left_max[i] = max(left_max[i - 1], height[i]);

        right_max[n - 1] = height[n - 1];
        for (int i = n - 2; i >= 0; i--)
            right_max[i] = max(right_max[i + 1], height[i]);

        for (int i = 0; i < n; i++)
            ans += min(left_max[i], right_max[i]) - height[i];

        return ans;
    }
};
```
![](42_2.png)

![](42_3.png)

```c++
/*
一层一层算面积
加上最后一个需要加的面积为橙色的面积
左边第一个比他大的位置，然后累加面积
*/
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size(), ans = 0;
        stack<int> st;
        for (int i = 0; i < n; i++) {
            while (!st.empty() && height[st.top()] < height[i]) {
                int top = st.top();
                st.pop();
                if (st.empty()) break;
                ans += (i - st.top() - 1) * (min(height[st.top()], height[i]) - height[top]);
            }
            st.push(i);
        }
        return ans;
    }
};
**********************************************
class Solution {
public:
    int trap(vector<int>& height) {
        int res = 0;
        stack<int> stk;

        for(int i = 0;i < height.size();i ++)
        {
            int last = 0;//存储上一层的高度
            while(stk.size() && height[stk.top()] <= height[i])
            {
                int t = stk.top();
                stk.pop();
                res += (i - t - 1) * (height[t] - last);//面积
                last = height[t];
            }

            if(stk.size()) res += (i - stk.top() - 1) * (height[i] - last);//最上面的一小层
            stk.push(i);
        }
        return res;
    }
};
```

## LeetCode 239. 滑动窗口最大值

![](239.png)

![](239_1.png)

```c++
/*
单调队列：滑动窗口中的最大值
暴力：用一个队列模拟窗口，枚举窗口内的最大值
单调队列：只要前一个数小于等于后一个数就把前一个数删掉，最后是一个单调下降的队列，队头元素一定是最大值。
*/
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> ans;
        deque<int> q;
        for (int i = 0; i < n; i++) {

            while (!q.empty() && i - q.front() >= k)
                q.pop_front();

            while (!q.empty() && nums[i] >= nums[q.back()])
                q.pop_back();

            q.push_back(i);

            if (i >= k - 1)
                ans.push_back(nums[q.front()]);
        }
        return ans;
    }
};
*******************************************************************
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> res;
        deque<int> q;
        for(int i = 0;i < nums.size();i ++)
        {
            if(q.size() && i - k + 1 > q.front()) q.pop_front();//队头出窗口删掉
            while(q.size() && nums[q.back()] <= nums[i]) q.pop_back();//队尾元素小于等于当前元素就删掉
            q.push_back(i);//把当前元素加入队列
            if(i >= k - 1) res.push_back(nums[q.front()]);//窗口大小等于k就输出出来
        }
        return res;
    }
};
```

## LeetCode 918. 环形子数组的最大和

![](918.png)

![](918_1.png)

```c++
/*
把大小为n的环展开成大下为2n的链，把环上的数字复制一遍
i端点前长度为n的窗口的最小值
*/
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& A) {
        int n = A.size(), ans = A[0];

        vector<int> sum(2 * n + 1, 0);
        for (int i = 1; i <= 2 * n; i++) {
            if (i <= n)
                sum[i] = sum[i - 1] + A[i - 1];
            else
                sum[i] = sum[i - 1] + A[i - n - 1];
        }

        deque<int> q;
        q.push_back(0);

        for (int i = 1; i <= 2 * n; i++) {
            while (!q.empty() && i - q.front() > n)
                q.pop_front();

            ans = max(ans, sum[i] - sum[q.front()]);

            while (!q.empty() && sum[i] <= sum[q.back()])
                q.pop_back();

            q.push_back(i);
        }

        return ans;
    }
};
**********************************************************
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& A) {
        int n = A.size();
        for(int i = 0;i < n;i ++) A.push_back(A[i]);//复制一份儿
        vector<int> sum(n * 2 + 1);
        for(int i = 1;i <= n * 2;i ++) sum[i] = sum[i - 1] + A[i - 1];//前缀和从1开始

        int res = INT_MIN;
        deque<int> q;
        q.push_back(0);//前0个数的和是0
        for(int i = 1;i <= n * 2;i ++)
        {
            if(q.size() && i - n > q.front()) q.pop_front();//维护窗口大小
            if(q.size()) res = max(res,sum[i] - sum[q.front()]);//队头是最小元素
            while(q.size() && sum[q.back()] >= sum[i]) q.pop_back();//求最小值
            q.push_back(i);
        }
        return res;
    }
};
```

