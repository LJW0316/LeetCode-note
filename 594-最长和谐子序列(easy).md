## 题目

和谐数组是指一个数组里元素的最大值和最小值之间的差别 正好是 1 。

现在，给你一个整数数组 nums ，请你在所有可能的子序列中找到最长的和谐子序列的长度。

数组的子序列是一个由数组派生出来的序列，它可以通过删除一些元素或不删除元素、且不改变其余元素的顺序而得到。

 

示例 1：

输入：nums = [1,3,2,2,5,2,3,7]
输出：5
解释：最长的和谐子序列是 [3,2,2,2,3]
示例 2：

输入：nums = [1,2,3,4]
输出：2
示例 3：

输入：nums = [1,1,1,1]
输出：0


提示：

1 <= nums.length <= 2 * 104
-109 <= nums[i] <= 109

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-harmonious-subsequence
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

## 枚举

该问题等价于求解，数组中两个相差为1的数在数组中出现的次数和的最大值。所以首先将数组排序，之后使用双指针统计相差为1的子数组长度（类似2 2 2 3 3 3 3这种长度为5）即可。

时间复杂度O(nlogn)，空间复杂度O(1).

```c++
class Solution {
public:
    int findLHS(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int res= 0;
        int begin = 0;
        for (int end = 0; end < nums.size(); end++) {
            while (nums[end] - nums[begin] > 1) {
                begin++;
            }
            if (nums[end] - nums[begin] == 1) {
                res = max(res, end - begin + 1);
            }
        }
        return res;
    }
};
```

## 哈希表

使用一个哈希表存储每个数出现的次数，再遍历一遍哈希表计算num 和num + 1出现的次数和的最大值即为结果。

时间复杂度O(n)，空间复杂度O(n)。

```c++
class Solution {
public:
    int findLHS(vector<int>& nums) {
        unordered_map<int, int>hash;
        for (auto num : nums) {
            hash[num]++;
        }
        int res = 0;
        for (auto [num, val] : hash) {
            if (hash.count(num + 1)) {
                res = max(res, val + hash[num + 1]);
            }
        }
        return res;
    }
};
```

