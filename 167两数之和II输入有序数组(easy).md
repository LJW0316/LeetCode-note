## 题目

## 题解

这题与两数之和类似，只是增加了一个条件，所以两数之和的方法均可用于该题，略。

### 二分查找

因为数组是有序的，所以对于一个数，我们可以在数组中使用二分查找，其后面的数有没有和它相加为target的数，有则返回。

时间复杂度O(nlogn)，空间复杂度O(1)。

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        for (int i = 0; i < numbers.size(); i++) {
            int left = i + 1, right = numbers.size() - 1;
            while (left <= right) { // 二分查找
                int mid = (left + right) / 2;
                if (numbers[mid] + numbers[i] == target) {
                    return {i + 1, mid + 1};
                }
                if (target - numbers[i] < numbers[mid]) {
                    right = mid - 1;
                } else{
                    left = mid + 1;
                }
            }
        }
        return {-1, -1};
    }
};
```

### 双指针

使用双指针法，开始时两个指针i指向开头，j指向结尾，如果numbers[i] + numbers[j] = target则返回，否则当相加结果大于target，说明需要缩小，则j--，当相加结果小于target，说明需要增大，则i++。

*具体证明见：[一张图告诉你 O(n) 的双指针解法的本质原理（C++/Java）](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/solution/yi-zhang-tu-gao-su-ni-on-de-shuang-zhi-zhen-jie-fa/)*

时间复杂度O(n)，空间复杂度O(1)。

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int i = 0, j = numbers.size() - 1;
        while (i < j) {
            int sum = numbers[i] + numbers[j];
            if (sum == target) {
                return vector<int>{i + 1, j + 1};
            }
            if (sum < target) {
                i++;
            } else {
                j--;
            }
        }
        return vector<int>{-1, -1};
    }
};
```

