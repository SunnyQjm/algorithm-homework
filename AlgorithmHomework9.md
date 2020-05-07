# Algorithm Homework 9

> 姓名：阙建明
>
> 学号：1901213139

## 作业要求：

Leetcode: 235, 110, 257

## 题解：

- ### Leetcode problem 235

  >  [二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

  - **算法思路：** 

    1. 首先判断节点p和节点q的大小，小的记作left，大的记作right；
    2. 接着对二叉搜索进行递归前序遍历；
    3. 根据二叉搜索树的特性，目标节点的值要满足大于等于left节点的值，且小于等于right节点的值

  - **Leetcode提交结果：**

    ![image-20200507221022918](AlgorithmHomework9.assets/image-20200507221022918.png)

  - **代码：**

    ```python
    class Solution(object):
        def lowestCommonAncestor(self, root, p, q):
            """
            :type root: TreeNode
            :type p: TreeNode
            :type q: TreeNode
            :rtype: TreeNode
            """
    
            def preOrderTraversal(root, left, right):
                if root is None or (left.val <= root.val <= right.val):
                    return root
    
                leftResult = preOrderTraversal(root.left, left, right)
                rightResult = preOrderTraversal(root.right, left, right)
                return leftResult if leftResult is not None else rightResult
    
            left = p if p.val < q.val else q
            right = p if p.val >= q.val else q
            return preOrderTraversal(root, left, right)
    ```

- ### Leetcode problem 110

  > [平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)

  - **算法思路：** 

    1. 直接递归计算每个节点左右子树的高度；
    2. 根据AVL树的特性，如果任意一个节点的左右子树的高度差大于一，则该树不是AVL树，标记器高度为-1;
    3. 如果某个子树的高度计算为-1,则表示该子树不是AVL树，-1会一直传递到顶层。

  - **Leetcode提交结果：**

  ![image-20200507222442113](AlgorithmHomework9.assets/image-20200507222442113.png)

  - **代码：**

    ```python
    class Solution(object):
        def isBalanced(self, root):
            """
            :type root: TreeNode
            :rtype: bool
            """
            def preOrderTraversal(root):
                if root is None:
                    return 0
                leftNum = preOrderTraversal(root.left)
                rightNum = preOrderTraversal(root.right)
                if leftNum == -1 or rightNum == -1 or abs(leftNum - rightNum) > 1:
                    return -1
                return 1 + max(leftNum, rightNum)
    
            return preOrderTraversal(root) != -1
    ```

- ### Leetcode problem 729

  > [二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)

  - **算法思路：** 

    1. 直接使用递归进行深度搜索；
    2. 一旦检索到一个叶子节点，就将当前遍历的路径添加到结果当中。

  - **Leetcode提交结果：**
  
  ![image-20200507225110291](AlgorithmHomework9.assets/image-20200507225110291.png)
  
  - **代码：**
  
    ```python
    class Solution(object):
        def binaryTreePaths(self, root):
            """
            :type root: TreeNode
            :rtype: List[str]
            """
    
            def preOrderTraversal(root, s, list):
                if root is None:
                    return
                if root.left is None and root.right is None:
                    list.append(s + "->" + str(root.val))
                preOrderTraversal(root.left, s + "->" + str(root.val), list)
                preOrderTraversal(root.right, s + "->" + str(root.val), list)
    
            if root is None:
                return []
            if root.left is None and root.right is None:
                return [str(root.val)]
            s = str(root.val)
            list = []
                
            preOrderTraversal(root.left, s, list)
            preOrderTraversal(root.right, s, list)
            return list
    ```
    
    