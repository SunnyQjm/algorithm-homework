# Algorithm Homework 3

> 姓名：阙建明
>
> 学号：1901213139

## 作业要求：

Leetcode: 35,58,83

## 题解：

- ### Leetcode problem 35

  > [搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

  - **算法思路：** 

    - 尝试使用二分法找到target的索引，找到则返回
    - 没有找到则判断target与左右索引的大小关系，返回应该插入的位置

  - **Leetcode提交结果：**

    ![image-20200313210334196](AlgorithmHomework3.assets/image-20200313210334196.png)

  - **代码：**

    ```python
    class Solution:
        def searchInsert(self, nums: List[int], target: int) -> int:
            left, right = 0, len(nums) - 1
            while left < right - 1:
                pos = (left + right) // 2
                if nums[pos] > target:
                    right = pos
                elif nums[pos] < target:
                    left = pos
                else:
                    return pos
            if nums[left] == target or target < nums[left]:
                return left
            elif nums[right] == target or target < nums[right]:
                return right
            else:
                return right + 1
    ```

- ### Leetcode problem 58

  > [最后一个单词的长度](https://leetcode-cn.com/problems/length-of-last-word/)

  - **算法思路：** 

    - 思路1：使用`split`将字符串用空格分割成数组，然后反序遍历，取第一个非空字符串的长度，无则返回0；
    - 思路2：使用`strip`截掉字符串前后的空格，然后反序遍历统计最后一个单次的长度，无则返回0。

  - **Leetcode提交结果：**

    ![image-20200313211905775](AlgorithmHomework3.assets/image-20200313213736914.png)

  - **代码：**

    ```python
    # 思路1代码
    class Solution:
        def lengthOfLastWord(self, s: str) -> int:
            strs = s.split(' ')
            for i in range(0, len(strs))[::-1]:
                if len(strs[i]) > 0: return len(strs[i])
            return 0
    
    # 思路2代码（性能更优）
    class Solution:
        def lengthOfLastWord(self, s: str) -> int:
            s = s.strip()
            count = 0
            for i in range(0, len(s))[::-1]:
                if s[i] == ' ':
                    return count
                else:
                    count += 1
            return count
    ```

- ### Leetcode problem 83

  > [删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

  - **算法思路：** 
  
  - 用一个set记录当前遍历过的值
    
- 遍历到每一个节点，用set检查当前值是否存在于set，如果存在则删除该节点
    
- **Leetcode提交结果：**
  
  ![image-20200313213736914](AlgorithmHomework3.assets/image-20200313211905775.png)
  
  - **代码：**
  
    ```python
    class Solution:
        def deleteDuplicates(self, head: ListNode) -> ListNode:
            if head == None:
                return head
            s, pre, cur = set(), head, head.next
            s.add(pre.val)
            while cur is not None:
                if cur.val in s:
                    pre.next, cur = cur.next, cur.next
                    continue
                else:
                    s.add(cur.val)
                pre, cur = cur, cur.next
          return head
    ```
  
    