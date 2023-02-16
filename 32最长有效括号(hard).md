## 题目

给你一个只包含 '(' 和 ')' 的字符串，找出最长有效（格式正确且连续）括号子串的长度。

 

示例 1：

输入：s = "(()"
输出：2
解释：最长有效括号子串是 "()"
示例 2：

输入：s = ")()())"
输出：4
解释：最长有效括号子串是 "()()"
示例 3：

输入：s = ""
输出：0


提示：

0 <= s.length <= 3 * 10<sup>4</sup>
s[i] 为 '(' 或 ')'

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/longest-valid-parentheses
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

### 动态规划

定义dp[i]表示以下标i字符结尾的最长有效括号长度。有效字串一定以')'结尾，因此以'('结尾的字串对应dp值一定为0，对与以')'结尾字串：

1. s[i] == ')' 且s[i - 1] =='('，即形如`......()`，dp[i] = dp[i - 2] + 2
2. s[i] == ')' 且s[i - 1] ==')'，即形如`......))`，若s[i - dp[i - 1] - 1] = '('，则dp[i] = dp[i - 1] + dp[i - dp[i - 1] - 2] + 2

若下标越界当成0处理即可。

时间复杂度O(n)，空间复杂度O(n).

```c++
class Solution {
public:
    int longestValidParentheses(string s) {
        vector<int> dp(s.size(), 0);
        int res = 0;
        for (int i = 1; i < s.size(); i++) {
            if (s[i] == ')') {
                if (s[i -1] == '(') {
                    dp[i] = (i >= 2 ? dp[i - 2] : 0) + 2;
                } else {
                    if (i - dp[i - 1] > 0 && s[i - dp[i - 1] - 1] == '(') {
                        dp[i] = (i >= dp[i - 1] + 2 ? dp[i - dp[i - 1] - 2] : 0) + dp[i - 1] + 2;
                    }
                }
            }
            if (dp[i] > res) {
                res = dp[i];
            }
        }
        return res;
    }
};
```

### 栈

我们始终保持栈底元素为当前已经遍历过的元素中「最后一个没有被匹配的右括号的下标」，这样的做法主要是考虑了边界条件的处理，栈里其他元素维护左括号的下标：

- 对于遇到的每个'('，将其下标放入栈中
- 对于遇到的每个')'，先弹出栈顶元素表示匹配了当前右括号：
  - 如果栈为空，说明当前右括号为没有被匹配的右括号，将其下标放入栈中更新我们之前提到的「最后一个没有被匹配的右括号的下标」
  - 如果栈不为空，当前右括号的下标减去栈顶元素即为「以该右括号为结尾的最长有效括号的长度」

如果一开始栈为空，第一个字符为左括号的时候我们会将其放入栈中，这样就不满足提及的「最后一个没有被匹配的右括号的下标」，为了保持统一，我们在一开始的时候往栈中放入一个值为 
-1的元素

时间复杂度O(n)，空间复杂度O(n).

```c++
class Solution {
public:
    int longestValidParentheses(string s) {
        int res = 0;
        stack<int> stk;
        stk.push(-1);
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(') {
                stk.push(i);
            } else {
                stk.pop();
                if (stk.empty()) {
                    stk.push(i);
                } else {
                    res = max(res, i - stk.top());
                }
            }
        }
        return res;
    }
};
```

### 贪心

我们利用两个计数器left和right 。首先，我们从左到右遍历字符串，对于遇到的每个 ‘(’，我们增加left 计数器，对于遇到的每个‘)’ ，我们增加right 计数器。每当left 计数器与right计数器相等时，我们计算当前有效字符串的长度，并且记录目前为止找到的最长子字符串。当right 计数器比left 计数器大时，我们将left和right计数器同时变回0。

这样的做法贪心地考虑了以当前字符下标结尾的有效括号长度，每次当右括号数量多于左括号数量的时候之前的字符我们都扔掉不再考虑，重新从下一个字符开始计算，但这样会漏掉一种情况，就是遍历的时候左括号的数量始终大于右括号的数量，即(()，这种时候最长有效括号是求不出来的。

解决的方法也很简单，我们只需要从右往左遍历用类似的方法计算即可，只是这个时候判断条件反了过来：

当left 计数器比right计数器大时，我们将left和right计数器同时变回0当left计数器与right 计数器相等时，我们计算当前有效字符串的长度，并且记录目前为止找到的最长子字符串
这样我们就能涵盖所有情况从而求解出答案。

时间复杂度O(n)，空间复杂度O(1)

```c++
class Solution {
public:
    int longestValidParentheses(string s) {
        int left = 0, right = 0, res = 0;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(') {
                left++;
            } else {
                right++;
                if (left == right) {
                    res = max(res, 2 * right);
                }
                if (right > left) {
                    left = 0;
                    right = 0;
                }
            }
        }
        left = 0;
        right = 0;
        for (int i = s.size() - 1;i >= 0; i--) {
            if (s[i] == ')') {
                right++;
            } else {
                left++;
                if (left == right) {
                    res = max(res, 2 * right);
                }
                if (right < left) {
                    left = 0;
                    right = 0;
                }
            }
        }
        return res;
    }
};
```



