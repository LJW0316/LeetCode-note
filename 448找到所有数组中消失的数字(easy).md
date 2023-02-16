## 题目

给你一个含 n 个整数的数组 nums ，其中 nums[i] 在区间 [1, n] 内。请你找出所有在 [1, n] 范围内但没有出现在 nums 中的数字，并以数组的形式返回结果。

 

示例 1：

输入：nums = [4,3,2,7,8,2,3,1]
输出：[5,6]
示例 2：

输入：nums = [1,1]
输出：[2]


提示：

n == nums.length
1 <= n <= 105
1 <= nums[i] <= n
进阶：你能在不使用额外空间且时间复杂度为 O(n) 的情况下解决这个问题吗? 你可以假定返回的数组不算在额外空间内。

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

### 哈希表

使用一个哈希表存放nums中的所有元素，再遍历一遍1到n若i不在哈希表中则说明i缺失。

时间复杂度O(n)，空间复杂度O(n)

```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        unordered_set<int> dic;
        for (int num : nums) {
            dic.insert(num);
        }
        vector<int> res;
        for (int i = 1; i <= nums.size(); i++) {
            if (dic.find(i) == dic.end()) {
                res.push_back(i);
            }
        }
        return res;
    }
};
```

### 原地修改

由于数组范围均在[1,n]中，我们也可以利用一个长度为n的数组替换哈希表。然而nums数组的长度也正好为n，能否让nums充当哈希表呢？

由于nums中数字范围均在[1, n]中，故可以利用这一范围外的数字，来表达是否存在的含义。

遍历nums，每遇到一个数x，就让nums[x - 1]增加n。增加后数字必然大于n，最后我们遍历nums，若nums[i]未大于n，说明没有遇到过数i + 1.

注意，当我们遍历到某个位置时，其中的数可能已经被增加过，因此需要对n取模来还原出它本来的值。

时间复杂度O(n)，空间复杂度O(1)。

```c++
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int> res;
        int n = nums.size();
        for (int num : nums) {
            int x = (num - 1) % n;
            nums[x] += n;
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] <= n) {
                res.push_back(i + 1);
            }
        }
        return res;
    }
};
```

