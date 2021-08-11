## 题目

在 MATLAB 中，有一个非常有用的函数 reshape ，它可以将一个 m x n 矩阵重塑为另一个大小不同（r x c）的新矩阵，但保留其原始数据。

给你一个由二维数组 mat 表示的 m x n 矩阵，以及两个正整数 r 和 c ，分别表示想要的重构的矩阵的行数和列数。

重构后的矩阵需要将原始矩阵的所有元素以相同的 行遍历顺序 填充。

如果具有给定参数的 reshape 操作是可行且合理的，则输出新的重塑矩阵；否则，输出原始矩阵。

 

示例 1：

![](https://assets.leetcode.com/uploads/2021/04/24/reshape1-grid.jpg)

输入：mat = [[1,2],[3,4]], r = 1, c = 4
输出：[[1,2,3,4]]
示例 2：

![](https://assets.leetcode.com/uploads/2021/04/24/reshape2-grid.jpg)

输入：mat = [[1,2],[3,4]], r = 2, c = 4
输出：[[1,2],[3,4]]


提示：

m == mat.length
n == mat[i].length
1 <= m, n <= 100
-1000 <= mat[i][j] <= 1000
1 <= r, c <= 300

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reshape-the-matrix
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

首先判断原矩阵元素个数和转换后矩阵元素个数是否相等，若不相等直接返回原矩阵。否则，设置一个数组row存储新矩阵一行的元素，遍历原矩阵，将元素依次添加到row中，当row中的元素等于c时，进行换行操作：将row添加到res中，计数器清零，row清空。

时间复杂度O(m * n)， 空间复杂度O(1)。

```c++
class Solution {
public:
    vector<vector<int>> matrixReshape(vector<vector<int>>& mat, int r, int c) {
        if (r * c != mat.size() * mat[0].size()) {
            return mat;
        }
        vector<vector<int>> res;
        vector<int> row;
        int index = 0;
        for (int i = 0; i < mat.size(); i++) {
            for (int j = 0; j < mat[0].size(); j++) {
                row.push_back(mat[i][j]);
                index++;
                if (index == c) { //换行操作
                    res.push_back(row);
                    index = 0;
                    row.clear();
                    continue;
                }
            }
        }
        return res;
    }
};
```

