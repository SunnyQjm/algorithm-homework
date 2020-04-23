# Algorithm Homework 8

> 姓名：阙建明
>
> 学号：1901213139

## 作业要求：

Leetcode: 72, 312, 729

## 题解：

- ### Leetcode problem 72

  > [编辑距离](https://leetcode-cn.com/problems/edit-distance/)

  - **算法思路：** 
      1. `dp[i][j] `表示word1的前i个字母和word2的前j个字母的编辑距离
      2. 在word1中删除一个字符，等价于在word2中适当位置添加一个字符
      3. 如果`word1[i - 1]` == `word2[j - 1]`:
          - `dp[i - 1][j]`的基础上在word1上增加一个字符 => 操作+1
          - `dp[i][j - 1]`的基础上在word2上增加一个字符 => 操作+1
          - `dp[i - 1][j - 1]`的基础上不做操作 => 操作+0
      3. 如果word1[i - 1] != word2[j - 1]
          - `dp[i - 1][j]`的基础上在word1上增加一个字符 => 操作+1
          - `dp[i][j - 1]`的基础上在word2上增加一个字符 => 操作+1
          - `dp[i - 1][j - 1]`的基础上修改`word1[i]`，使得`word1[i] = word2[j]` => 操作+1

  - **Leetcode提交结果：**

    ![image-20200423115932875](AlgorithmHomework8.assets/image-20200423115932875.png)

  - **代码：**

    ```python
    class Solution(object):
        def minDistance(self, word1, word2):
            """
            :type word1: str
            :type word2: str
            :rtype: int
            """
            n, m = len(word1), len(word2)
            if n * m == 0:
                return n + m
            dp = [[0] * (m + 1) for _ in range(n + 1)]
            for i in range(n + 1):
                dp[i][0] = i
            for j in range(m + 1):
                dp[0][j] = j
    
            for i in range(1, n + 1):
                for j in range(1, m + 1):
                    tmp1 = dp[i - 1][j] + 1
                    tmp2 = dp[i][j - 1] + 1
                    tmp3 = dp[i - 1][j - 1]
                    if word1[i - 1] != word2[j - 1]:
                        tmp3 += 1
                    dp[i][j] = min(tmp1, tmp2, tmp3)
            return dp[-1][-1]
    ```

- ### Leetcode problem 312

  > [ 戳气球](https://leetcode-cn.com/problems/burst-balloons/)

  - **算法思路：** 

    - 采用动态规划的思想，`dp[left][right]` 表示戳破开区间（left, right）中的气球所能得到的最大金币数量；
  
  - 为了处理边缘的数据，这边在初始数组的左右两边各加一个值为1的元素；
  
  - 考虑从没有气球开始把气球逐个加入，则添加第i个气球后能得到的最大金币数为：
  
    $nums[left] * nums[i] * nums[right] + dp(left, i) + dp(i, right)$
  
- **Leetcode提交结果：**
  
    ![image-20200423211525664](AlgorithmHomework8.assets/image-20200423211525664.png)
  
  - **代码：**
  
    ```python
    class Solution(object):
        def maxCoins(self, nums):
            """
            :type nums: List[int]
            :rtype: int
            """
            nums = [1] + nums + [1]
            n = len(nums)
            dp = [[0] * n for _ in range(n)]
            for left in range(n-2, -1, -1):
                for right in range(left+2, n):
                    dp[left][right] = max(nums[left] * nums[i] * nums[right] + dp[left][i] + dp[i][right] for i in range(left+1, right))
            return dp[0][n-1]
    ```
  
- ### Leetcode problem 729

  > [我的日程安排表 I](https://leetcode-cn.com/problems/my-calendar-i/)

  - **算法思路：** 

    - 每次判断现有的行程中有没有和当前要添加的行程有重叠的部分；
  - 如果有重叠，则直接返回Fase
    - 如果没有重叠，则将新的行程插入到特定位置，保证行程数组是按序插入的

  - **Leetcode提交结果：**

    ![image-20200423221735514](AlgorithmHomework8.assets/image-20200423221735514.png)

  - **代码：**

    ```python
    class MyCalendar(object):
    
        def __init__(self):
            self.calendar = []
    
        def book(self, start, end):
            """
            :type start: int
            :type end: int
            :rtype: bool
            """
    
            if len(self.calendar) == 0:
                self.calendar.append([start, end])
                return True

            for i in range(len(self.calendar)):
                if self.calendar[i][0] >= end:
                    if i == 0:
                        self.calendar.insert(0, [start, end])
                        return True
                    elif self.calendar[i - 1][1] <= start:
                        self.calendar.insert(i, [start, end])
                        return True
            if self.calendar[-1][1] <= start:
                self.calendar.append([start, end])
                return True
            return False
    ```
    
    