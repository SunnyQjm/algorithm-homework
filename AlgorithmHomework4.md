# Algorithm Homework 4

> 姓名：阙建明
>
> 学号：1901213139

## 作业要求：

Leetcode: 16，17，19

## 题解：

- ### Leetcode problem 16

  >  [最接近的三数之和](https://leetcode-cn.com/problems/3sum-closest/)

  - **算法思路：** 

    - 首先对nums进行从小到达排序；
    - 然后依次遍历0 ~ size - 2，固定第一个数i，对i + 1 ~ size - 1的数用双指针法处理；
    - 通过忽略已处理过的数来实现第一个数选取时的剪枝操作

  - **Leetcode提交结果：**

    ![image-20200326200557425](AlgorithmHomework4.assets/image-20200326200557425.png)

  - **代码：**

    ```python
    class Solution:
        def threeSumClosest(self, nums: List[int], target: int) -> int:
            if len(nums) < 3:
                return 0
            elif len(nums) == 3:
                return nums[0] + nums[1] + nums[2]
            nums = sorted(nums)  # 先排序
            result, lastFirst = nums[0] + nums[1] + nums[2], nums[0] - 1
            for i in range(0, len(nums) - 2):
                if nums[i] == lastFirst:
                    continue
                left, right, lastFirst = i + 1, len(nums) - 1, nums[i]
                while left < right:
                    tmp = nums[i] + nums[left] + nums[right]
                    if abs(target - tmp) < abs(target - result):
                        result = tmp
                    if tmp < target:
                        left += 1
                    elif tmp > target:
                        right -= 1
                    else:
                        return result
            return result
    ```

- ### Leetcode problem 17

  > [电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

  - **算法思路：** 

    - 采用回溯法递归遍历
    
  - **Leetcode提交结果：**

    ![image-20200326210047592](AlgorithmHomework4.assets/image-20200326210047592.png)

  - **代码：**

    ```python
    class Solution:
        numMap = {'2': "abc", '3': "def", '4': "ghi", '5': "jkl", '6': "mno", '7': "pqrs", '8': "tuv", '9': "wxyz"}
        result = []
        def _letterCombinations(self, path, digits: str, index):
            if index == len(digits) - 1:
                for ch in self.numMap[digits[index]]:
                    self.result.append(path + ch)
                return
            for ch in self.numMap[digits[index]]:
                path = path + ch
                self._letterCombinations(path, digits, index + 1)
                path = path[:len(path) - 1]
    
        def letterCombinations(self, digits: str) -> List[str]:
            if digits == "":
                return []
            self.result = []
            self._letterCombinations("", digits, 0)
            return self.result
    ```

- ### Leetcode problem 19

  > [删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

  - **算法思路：** 
  
  - 用快慢指针法
  - 先让快指针走n步，然后让快慢指针同时右移，快指针为None时，慢指针指向的节点即为要删除的节点；
  - 对要删除的节点是头结点的情况进行特判。
  
- **Leetcode提交结果：**
  
  ![image-20200326211143566](AlgorithmHomework4.assets/image-20200326211143566.png)
  
  - **代码：**
  
    ```python
    class Solution:
        def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
            fast, slow, pre = head, head, self
            try:
                for i in range(n):
                    fast = fast.next
            except BaseException:
                return head
            if fast is None:
                return head.next
            while fast is not None:
                fast, slow, pre = fast.next, slow.next, slow
          pre.next = slow.next
            return head
    ```
  
    