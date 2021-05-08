## 题目

给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数 大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

 

示例 1：

输入：[3,2,3]
输出：3
示例 2：

输入：[2,2,1,1,1,2,2]
输出：2


进阶：

尝试设计时间复杂度为 O(n)、空间复杂度为 O(1) 的算法解决此问题。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/majority-element
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

### 哈希表

先遍历一遍nums数组使用哈希表cnt统计nums数组中每个元素出现的次数，再遍历一遍哈希表cnt，找出其中值最大的键即可。

时间复杂度O(n)，空间复杂度O(n)。

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
       unordered_map<int, int> cnt;
       for (int i = 0; i < nums.size(); i++) {
           cnt[nums[i]]++;
       } 
       int max = 0, t;
       for(auto x : cnt) {
           if(x.second > max) {
               max = x.second;
               t = x.first;
           }
       }
       return t;
    }
};
```

### 排序

对数组排序，因为出现次数大于 `⌊ n/2 ⌋`所以返回排序后的中间元素即为结果。

时间复杂度O(nlogn)，空间复杂度O(logn)。

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        return nums[nums.size() / 2];
    }
};
```

### 分治

如果数a是数组nums的众数，我们将nums分为左右两部分，则a至少是一部分的众数。

使用递归分治方法进行处理，每次求得的左右区间的众数，再判断求得的值是否为整体的众数。

时间复杂度O(nlogn)，空间复杂度O(logn)。

```c++
class Solution {
public:
    bool countMajority(vector<int>& nums, int target, int low, int high) { //判断是否为众数
        int cnt = 0;
        for (int i = low; i <= high; i++) {
            if (nums[i] == target) {
                cnt++;
            }
        }
        if (cnt > ((high - low + 1) / 2)) {
            return true;
        } else {
            return false;
        }
    }
    int majorityElementFind(vector<int>& nums, int low, int high) { //递归分治处理
        if (low == high) {  //递归边界条件
            return nums[low];
        }
        int mid = (low + high) / 2;
        int leftMajority = majorityElementFind(nums, low, mid); //递归处理左侧区间
        int rightMajority = majorityElementFind(nums, mid + 1, high); //递归处理右侧区间
        if (countMajority(nums, leftMajority, low, high)) { //判断左侧的值是否为整体众数
            return leftMajority;
        }
        if (countMajority(nums, rightMajority, low, high)) { //判断右侧的值是否为整体正数
            return rightMajority;
        }
        return -1;
    }
    int majorityElement(vector<int>& nums) {
        return majorityElementFind(nums, 0, nums.size() - 1);
    }
};
```

### Boyer-Moore投票算法

使用candidate记录当前候选的众数，初始值为nums[0]，遍历一遍nums，如果nums[i] = candidate，计数统计cnt + 1，否则cnt  - 1，当cnt小于0时，将candidate更换为nums[i]，cnt重新初始为1。

时间复杂度O(n)，空间复杂度O(1)。

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int cnt = 0, candidate = nums[0];
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] == candidate) {
                cnt++;
            } else if (--cnt < 0) {
                candidate = nums[i];
                cnt = 1;
            }
        }
        return candidate;
    }
};
```

