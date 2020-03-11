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

