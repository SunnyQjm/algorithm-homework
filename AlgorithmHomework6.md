# Algorithm Homework 6

> 姓名：阙建明
>
> 学号：1901213139

## 作业要求：

Leetcode: 5, 64, 120

## 题解：

- ### Leetcode problem 5

  >   [最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

  - **算法思路：** 

    - 使用动态规划的思想；
    - 状态转移方程如下：
      
      $P(i, j)  = \begin{cases}True &\text{if } P(i + 1, j - 1)==True ,  S_i == S_j  \\False &\text{if 其它 } \end{cases}$

    - 首先判断长度为1的子串，再判断长度为2的子串，再判断长度为3的子串，以此类推。

  - **Leetcode提交结果：**

    ![image-20200407102849523](AlgorithmHomework6.assets/image-20200407102849523.png)

  - **代码：**

    ```python
    class Solution:
        def longestPalindrome(self, s: str) -> str:
            arr = [[False] * len(s) for i in range(len(s))]
            for i in range(len(s)):
                arr[i][i] = True
            maxLen, maxStrStart, maxStrEnd = 1, 0, 1
            for span in range(1, len(s)):
                left, right = 0, span
                while right < len(s):
                    arr[left][right] = True if s[left] == s[right] and (span == 1 or arr[left + 1][right - 1]) else False
                    if arr[left][right] and right - left + 1 > maxLen:
                        maxLen, maxStrStart, maxStrEnd = right - left + 1, left, right + 1
                    left, right = left + 1, right + 1
            return s[maxStrStart: maxStrEnd]
    ```

- ### Leetcode problem 64

  >   [最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)

  - **算法思路：** 

    - 采用动态规划的思想，自右下角向左上脚遍历。

    - 首先将最下面一排的路径开销自右往左以此叠加，把最右边一排的路径开销自下往上以此叠加，接着其它情况使用与下述状态转换方程：
      
      $P(i, j)  = \begin{cases}P(i, j) + P(i + 1, j) &\text{if } P(i, j + 1) >= P(j + 1) \\P(i, j) + P(i, j + 1) &\text{if }P(i, j + 1) < P(j + 1) \end{cases}$

  - **Leetcode提交结果：**

    ![image-20200407105454180](AlgorithmHomework6.assets/image-20200407105454180.png)

  - **代码：**

    ```python
    class Solution:
        def minPathSum(self, grid: List[List[int]]) -> int:
            rowNum, colNum = len(grid), len(grid[0])
            for col in range(colNum - 2, -1, -1):
                grid[rowNum - 1][col] = grid[rowNum - 1][col] + grid[rowNum - 1][col + 1]
            for row in range(rowNum - 2, -1, -1):
                grid[row][colNum - 1] = grid[row][colNum - 1] + grid[row + 1][colNum - 1]
            for col in range(colNum - 2, -1, -1):
                for row in range(rowNum - 2, -1, -1):
                    grid[row][col] = grid[row][col] + min(grid[row + 1][col], grid[row][col + 1])
            return grid[0][0]
    ```

- ### Leetcode problem 120

  >  [三角形最小路径和](https://leetcode-cn.com/problems/triangle/)

  - **算法思路：** 

    - 用动态规划的思想，自底向上一次计算到当前访问节点到叶子节点的最短路径；
  
    - 状态转移方程如下：
      
      $P(row, index)  = \begin{cases}P(row, index) &\text{if } row == len(triangle) - 1 \\P(row, index) + min(P(row + 1, index), P(row + 1, index + 1)) &\text{if }row < len(triangle) - 1 \end{cases}$
  
  - **Leetcode提交结果：**
  
    ![image-20200407122505839](AlgorithmHomework6.assets/image-20200407122505839.png)
  
  - **代码：**
  
    ```python
    class Solution:
      def minimumTotal(self, triangle: List[List[int]]) -> int:
            for row in range(len(triangle) - 2, -1, -1):
                for index in range(len(triangle[row])):
                    triangle[row][index] = triangle[row][index] + min(triangle[row + 1][index], triangle[row + 1][index + 1])
            return triangle[0][0]
    ```
    
  