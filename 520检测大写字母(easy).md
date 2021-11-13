## 题目

我们定义，在以下情况时，单词的大写用法是正确的：

全部字母都是大写，比如 "USA" 。
单词中所有字母都不是大写，比如 "leetcode" 。
如果单词不只含有一个字母，只有首字母大写， 比如 "Google" 。
给你一个字符串 word 。如果大写用法正确，返回 true ；否则，返回 false 。

 

示例 1：

输入：word = "USA"
输出：true
示例 2：

输入：word = "FlaG"
输出：false


提示：

1 <= word.length <= 100
word 由小写和大写英文字母组成

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/detect-capital
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

按照题目要求进行模拟即可。

时间复杂度O(n)，空间复杂度O(1)。

```c++
class Solution {
public:
    bool detectCapitalUse(string word) {
        if (word[0] <= 'z' && word[0] >= 'a') { //首字母小写情况
            for (int i = 1; i < word.size(); i++) {
                if (word[i] < 'a') {
                    return false;
                }
            }
            return true;
        } else { //首字母大写情况
            bool check = false;
            for (int i = 1; i < word.size(); i++) {  
                if (i == 1 && word[i] <= 'z' && word[i] >= 'a') {
                    check = true;
                    continue;
                }
                if (!check && word[i] <= 'z' && word[i] >= 'a') { //第二个字母为大写后面出现小写
                    return false;
                }
                if (check && word[i] < 'a') { //第二给字母小写后面出现大写
                    return false;
                }
            }
            return true;
        }
    }
};
```

