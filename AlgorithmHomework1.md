# Algorithm Homework 1
> 姓名：阙建明
>
> 学号：1901213139

- ## 作业要求
  3道上课讲的LeetCode题目1，69，70

- ## Leetcode problem 1 (两数之和)
  
  - 算法思路：
    
  
  用字典记录数字，以便在找到成对的另一个数的时候，可以快速的检索到前一个数的索引
  
  - Leetcode提交结果
  
    ![result](AlgorithmHomework1.assets/7222676-c485f42b177afce2.png)
    
  - 代码：
    ```python
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        myDict = {}
        for index, num in enumerate(nums):
            if num in myDict:
                return [myDict[num], index]
          else:
                myDict[target - num] = index
    ```
  
- ## Leetcode problem 69 (x 的平方根)
  - 算法思路：二分法
  
  - Leetcode提交结果
  
    ![result](AlgorithmHomework1.assets/7222676-6a5936bab1f54a06.png)
  - 代码：
    ```python
    def mySqrt(self, x: int) -> int:
        if x in [1, 2, 3]:
            return 1
        left = 0
        right = x // 2
        while True:
            mid = (left + right) // 2
            if left + 1 >= right:
                break
            if mid < x / mid:  # mid to small
                left = mid
            else:
                right = mid
        return right if right * right <= x else left
    ```

- ## Leetcode problem 70 (爬楼梯)
  - 算法思路：动态规划。反向遍历，由于当前状态只取决于前两个状态，所以dp table可以精简为只用两个数表示
  
  - Leetcode提交结果
  
    ![image-20200416230211927](AlgorithmHomework1.assets/image-20200416230211927.png)
  - 代码：
    ```python
    class Solution(object):
        def minCostClimbingStairs(self, cost):
            f1 = f2 = 0	# 顶部cost为0
            for x in reversed(cost):
                f1, f2 = x + min(f1, f2), f1
            return min(f1, f2)
    ```
