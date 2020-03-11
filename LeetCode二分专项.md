---
typora-root-url: img/
---

## LeetCode 69. x 的平方根

![](69.png)

```c++
/*
二分流程
1.确定二分边界
2.编写二分代码框架
3.设计一个check函数(性质)
4.判断一下区间如何更新
5.如果更新方式是l=mid，r=mid-1，那么就在算mid的时候加1 
*/
class Solution {
public:
    int mySqrt(int x) {
        int l = 0,r = x;
        while(l < r)
        {
            int mid = l + r + 1ll >> 1;//1ll表示1ll是long long类型的1
            if(mid <= x / mid) l = mid;
            else r = mid - 1;
        }
        return r;
    }
};
```
## LeetCode 35. 搜索插入位置

![](35.png)

```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int n = nums.size();
        if (n == 0)
            return 0;
        int l = 0, r = n - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] < target)
                l = mid + 1;
            else
                r = mid;
        }

        if (nums[l] >= target)
            return l;

        return n;
    }
};
**********************************************************************
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int l = 0,r = nums.size();
        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mid] >= target) r = mid;
            else l = mid + 1;
        }
        return l;
    }
};
```
## LeetCode 34. 在排序数组中查找元素的第一个和最后一个位置

![](34.png)

```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        if(nums.empty()) return {-1,-1};

        int l = 0,r = nums.size() - 1;
        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mid] >= target) r = mid;
            else l = mid + 1;
        }

        if(nums[r] != target) return {-1,-1};
        int strat = r;
        l = 0,r = nums.size() - 1;
        while(l  < r)
        {
            int mid = l + r + 1 >> 1;
            if(nums[mid] <= target) l = mid;
            else r = mid - 1;
        }
        int end = l;
        return {strat,end};
    }
};
```

## LeetCode 74. 搜索二维矩阵

![](74.png)

```c++
/*
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
二维坐标(x,y)，方阵n
一维t
二维数组下标变为一维数组下标——————————t = x * n + y;
一维数组下标变为二维数组下标——————————x = t / n;y = t % n;
*/
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.empty() || matrix[0].empty()) return false;

        int n = matrix.size(),m = matrix[0].size();
        int l = 0,r = n * m - 1;
        while(l < r)
        {
            int mid = l + r >> 1;
            if(matrix[mid / m][mid % m] >= target) r = mid;//一维数组下标变为二维
            else l = mid + 1;
        }
        if(matrix[l/ m][l % m] != target) return false;
        return true;
    }
};
```

## LeetCode 153. 寻找旋转排序数组中的最小值

![](153.png)

![](153_1.png)

```c++
/*
找出最小值
check(t)
	return nums[t] <= nums.back()
*/
class Solution {
public:
    int findMin(vector<int>& nums) {
        if (nums.back() > nums[0]) return nums[0];
        int l = 0, r = nums.size() - 1;
        while (l < r)
        {
            int mid = l + r >> 1;
            if (nums[mid] >= nums[0]) l = mid + 1;
            else r = mid;
        }
        return nums[l];
    }
};
*******************************************************************************
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0,r = nums.size() - 1;
        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mid] <= nums.back()) r = mid;
            else l = mid + 1;
        }
        return nums[r];
    }
};
```
## LeetCode 33. 搜索旋转排序数组

![](33.png)

![](33_1.png)

```c++
/*
找出目标值
*/
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        if (n == 0)
            return -1;
        int l = 0, r = n - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= nums[0]) { // mid 在数组前半部分。
                if (target > nums[mid])
                    // 可以推出 target 的值一定大于 nums[0]，target 只可能在 [mid + 1, r] 中。
                    l = mid + 1;
                if (target < nums[0])
                    // 可以推出 target 的值一定小于 nums[mid]，target只可能在 [mid + 1, r] 中。
                    l = mid + 1;
                if (target <= nums[mid] && target >= nums[0])
                    // 此时 target 的值处于 nums[0] 和 nums[mid] 中，故可能在 [l, mid] 中。
                    r = mid;
            }
            else { // mid在数组后半部分
                if (target >= nums[0])
                    // 可以推出 target 的值一定大于 nums[mid]，target只可能在 [l, mid] 中。
                    r = mid;    
                if (target <= nums[mid])
                    // 可以推出 target 的值一定小于 nums[0]，target只可能在 [l, mid] 中。
                    r = mid;
                if (target > nums[mid] && target < nums[0])
                    // 此时 target 的值处于 nums[0] 和 nums[mid] 中，故可能在 [mid + 1, r] 中。
                    l = mid + 1;
            }
        }
        return nums[l] == target ? l : -1;
    }
};
*****************************************************************************
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if(nums.empty()) return -1;
        
        int l = 0,r = nums.size() - 1;
        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mid] <= nums.back()) r = mid;
            else l = mid + 1;
        }

        if(target <= nums.back()) r = nums.size() - 1;
        else l = 0,r --;

        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mid] >= target) r = mid;
            else l = mid + 1;
        }

        if(nums[l] == target) return l;
        return -1;
    }
};
```

## LeetCode 278.第一个错误的版本

![](278.png)

```c++
// Forward declaration of isBadVersion API.
bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        long long l = 0,r = n;
        while(l < r)
        {
            int mid = l + r + 0ll >> 1;
            if(isBadVersion(mid)) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```

## LeetCode 162. 寻找峰值

![](162.png)

![](162_1.png)

```c++
/*
两边都是负无穷
峰值：比两边都大
暴力O(n)
二分O(logn)
找中点，若中点比下一个点小，则峰值一定在后面，否则在前面
*/
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        while (l < r)
        {
            int mid = (l + r + 1) / 2;
            if (nums[mid] > nums[mid - 1]) l = mid;
            else r = mid - 1;
        }
        return l;
    }
};
******************************************************************
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int l = 0,r = nums.size() - 1;
        while(l < r)
        {
            int mid = l + r >> 1;
            if(nums[mid] > nums[mid + 1]) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```

## LeetCode 287. 寻找重复数

![](287.png)

![](287_1.png)

```c++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int first = 0, second = 0;
        do {
            first = nums[first];
            second = nums[nums[second]];
        } while (first != second);

        first = 0;
        while (first != second) {
            first = nums[first];
            second = nums[second];
        }
        return first;
    }
};
***********************************************************************************
/*
抽屉原理
二分中点
计算左右两区间的数的个数，不可能有两边同时小于苹果个数的情况
*/
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int l = 1,r = nums.size() - 1;
        while(l < r)
        {
            int mid = l + r >> 1;
            int cnt = 0;
            for(auto x : nums)
                if(x >= l && x <= mid)
                    cnt ++;

            if(cnt > mid - l + 1) r = mid;
            else l = mid + 1;
        }
        return r;
    }
};
```

## LeetCode 275. H指数 II

![](275.png)

![](275_1.png)

```c++
/*
找到一个h使得至少存在h个数大于等于h，h最大为多少。
h不一定在数组里
0 <= h <= n
具有二分性质 
*/
class Solution {
public:
    int hIndex(vector<int>& citations) {
        if (citations.empty()) return 0;
        int l = 0, r = citations.size() - 1;
        while (l < r)
        {
            int mid = (l + r) / 2;
            if (citations.size() - mid <= citations[mid]) r = mid;
            else l = mid + 1;
        }
        if (citations.size() - l <= citations[l]) return citations.size() - l;
        return 0;
    }
};
***********************************************************************************
class Solution {
public:
    int hIndex(vector<int>& citations) {
        if(citations.empty()) return 0;
        int l = 0,r = citations.size();
        while(l < r)
        {
            int mid = l + r + 1>> 1;
            if(citations[citations.size() - mid] >= mid) l = mid;
            else r = mid - 1;
        }
        return r;
    }
};
```

