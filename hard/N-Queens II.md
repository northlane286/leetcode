# [52. N皇后 II](https://leetcode-cn.com/problems/n-queens-ii/)
n 皇后问题研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。

![img](https://github.com/woai3c/leetcode/blob/master/imgs/8-queens.png)

上图为 8 皇后问题的一种解法。

给定一个整数 n，返回 n 皇后不同的解决方案的数量。

示例:
```
输入: 4
输出: 2
解释: 4 皇后问题存在如下两个不同的解法。

[
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
```

## 解法
N 皇后在放置的时候要考虑每个皇后在纵、横、斜线一共 8 个方向上不能同时存在其他皇后。
### 实现一
回溯
```js
/**
 * @param {number} n
 * @return {number}
 */
 var totalNQueens = function(n) {
    let result = 0
    const path = [] // 用来放置皇后的数组

    function dfs(row) {
        // 放置完 n 个皇后
        if (row == n) return result++
        for (let col = 0; col < n; col++) {
            path[row] = col
            // 横、竖、斜三者不能相同，如果有一个相同则跳过
            if (judge(path, row)) continue
            // 不冲突则继续放置下一个皇后
            dfs(row + 1)
        }
    }

    dfs(0)
    return result
};

function judge(path, curRow) {
    for(let row = 0; row < curRow; row++) {
        // 主对角线 由左上至右下的对角线 同一主对角线上的 row - col 差相同
        // 次对角线 右上至左下的对角线 同一次对角线上的 row + col 和相同
        // 符合以下三个条件之一，则冲突
        // if(
        //     path[row] == path[curRow] 同列
        //     || (row - path[row] == curRow - path[curRow]) 主对角线相同
        //     || (path[row] + row == path[curRow] + curRow) 次对角线相同
        // ) {
        //     return true
        // }

        // 使用下列方法判断冲突更加方便
        // 通过观察可以发现，同一对角线上的每个点，它们的行相减的差值等于列相减的差值
        if(path[row] == path[curRow] || Math.abs(curRow - row) == Math.abs(path[curRow] - path[row])) {
            return true
        }
    }

    return false
}
```
