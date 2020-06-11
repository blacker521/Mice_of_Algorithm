---
typora-root-url: img
---

## 1. 两数之和

![](/1.png)

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> hash; 
        for(int i = 0;i < nums.size();i ++)
        {
            if(hash.count(target - nums[i])) return {hash[target -nums[i]],i};
            hash[nums[i]] = i;
        }
        return {-1,-1};
    }
};
```
## 2. 两数相加

![](/2.png)

```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        auto dummy = new ListNode(-1),cur = dummy;
        int t = 0;
        while(l1 || l2 || t)
        {
            if(l1) t += l1->val,l1 = l1->next;
            if(l2) t += l2->val,l2 = l2->next;
            cur = cur->next = new ListNode(t % 10);
            t /= 10;
        }
        return dummy->next;
    }
};
```
## 3. 无重复字符的最长子串

![](/3.png)

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char,int> hash;
        int res = 0;
        for(int i = 0,j = 0;j < s.size();j ++)
        {
            hash[s[j]] ++;
            while(hash[s[j]] > 1) hash[s[i ++]] --;
            res = max(res,j - i + 1);
        }
        return res;
    }
};
```
## 4. 寻找两个有序数组的中位数

![](/4.png)

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int N1 = nums1.size();
        int N2 = nums2.size();
        if (N1 < N2) return findMedianSortedArrays(nums2, nums1);

        int lo = 0, hi = N2 * 2;
        while (lo <= hi) {
            int mid2 = (lo + hi) / 2;
            int mid1 = N1 + N2 - mid2;

            double L1 = (mid1 == 0) ? INT_MIN : nums1[(mid1-1)/2];
            double L2 = (mid2 == 0) ? INT_MIN : nums2[(mid2-1)/2];
            double R1 = (mid1 == N1 * 2) ? INT_MAX : nums1[(mid1)/2];
            double R2 = (mid2 == N2 * 2) ? INT_MAX : nums2[(mid2)/2];

            if (L1 > R2) lo = mid2 + 1;
            else if (L2 > R1) hi = mid2 - 1;
            else return (max(L1,L2) + min(R1, R2)) / 2;
        }
        return -1;
    }
};
/*递归
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int tot = nums1.size() + nums2.size();
        if(tot % 2 == 0)
        {
            int left = find(nums1,0,nums2,0,tot / 2);
            int right = find(nums1,0,nums2,0,tot / 2 + 1);
            return (left + right) / 2.0;
        }
        else 
        {
            return find(nums1,0,nums2,0,tot / 2 + 1);
        }
    }
    int find(vector<int> &nums1,int i,vector<int> &nums2,int j,int k)
    {
        if(nums1.size() - i  > nums2.size() - j) return find(nums2,j,nums1,i,k);
        if(k == 1)
        {
            if(nums1.size() == i) return nums2[j];
            else return min(nums1[i],nums2[j]);
        }
        if(nums1.size() == i) return nums2[j + k - 1];
        int si = min((int)nums1.size(),i + k / 2),sj = j + k - k / 2;
        if(nums1[si - 1] > nums2[sj - 1])
            return find(nums1,i,nums2,sj,k - (sj - j));
        else
            return find(nums1,si,nums2,j,k - (si - i));
    }
};*/
```
## 5. 最长回文子串

![](/5.png)

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        string res;
        for(int i = 0;i < s.size();i ++)
        {
            for(int j = i,k = i;j >= 0 && k < s.size() && s[k] == s[j];j --,k ++)
                if(res.size() < k - j + 1)
                    res = s.substr(j,k - j + 1);
            
            for(int j = i,k = i + 1;j >= 0 && k < s.size() && s[k] == s[j];j --,k ++)
                if(res.size() < k - j + 1)
                    res = s.substr(j,k - j + 1);
        }
        return res;
    }
};
```
## 6. Z 字形变换

![](6.png)

```c++
class Solution {
public:
    string convert(string s, int n) {
        if(n == 1) return s;
        string res;
        for(int i = 0;i < n;i ++)
        {
            if(!i || i == n -1)
            {
                for(int j = i;j < s.size();j += 2 * (n - 1)) res += s[j];
            }
            else
            {
                for(int j = i,k = 2 * (n - 1) - i;j < s.size() || k < s.size();j += 2 *(n - 1),k += 2 * (n - 1))
                {
                    if(j < s.size()) res += s[j];
                    if(k < s.size()) res += s[k];
                }
            }
        }
        return res;
    }
};
```
## 7. 整数反转

![](7.png)

```c++
class Solution {
public:
    int reverse(int x) {
        long long res = 0;
        while(x)
        {
            res = res * 10 + x % 10;
            x /= 10;
        }
        if (res < INT_MIN || res > INT_MAX) return 0;
        return res;
    }
};
```
## 8. 字符串转换整数 (atoi)

![](8.png)

```c++
class Solution {
public:
    int myAtoi(string str) {
        long long res = 0;
        int k = 0;
        while(k < str.size() && (str[k] == ' ' || str[k] == '\t')) k ++ ;
        int minus = 1;
        if (k >= str.size()) return 0;
        if (str[k] == '-') minus = -1, k ++;
        if (str[k] == '+')
            if (minus == -1) return 0;
            else k ++ ;
        while(str[k] >= '0' && str[k] <= '9')
        {
            res = res * 10 + str[k] - '0';
            k ++ ;
            if (res > INT_MAX) break;
        }
        res *= minus;
        if (res > INT_MAX) return INT_MAX;
        if (res < INT_MIN) return INT_MIN;
        return res;
    }
};
```
## 9. 回文数

![](9.png)

```c++
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0 || x && x % 10 == 0) return false;
        int s = 0;
        while (s <= x)
        {
            s = s * 10 + x % 10;
            if (s == x || s == x / 10) return true; // 分别处理整数长度是奇数或者偶数的情况
            x /= 10;
        }
        return false;
    }
};
```
## 10. 正则表达式匹配

![](10.png)
