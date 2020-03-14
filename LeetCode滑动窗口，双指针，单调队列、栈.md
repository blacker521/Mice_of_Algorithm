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

## LeetCode 88. 

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

