# LeetCode: 378. 有序矩阵中第K小的元素

[TOC]



## 1、题目描述



给定一个 *n x n* 矩阵，其中每行和每列元素均按升序排序，找到矩阵中第k小的元素。
请注意，它是排序后的第k小元素，而不是第k个元素。

**示例:**

```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

返回 13。
```

**说明:** 
你可以假设 k 的值永远是有效的, $1 ≤ k ≤ n^2$ 。



## 2、解题思路



​	有一种思路，我们能够直接判断当前值所处的范围

左上角的都是小于当前值的，右下角的都是大于当前值的，因此，我们就能够直接得到当前值在整个矩阵中所可能处于的位置

```
(1,1) (2,4) (3,7)
(2,4) (4,6) (6,8)
(3,7) (6,8) (9,9)
```

假如我们想要找第8个元素，就判断(6,8)位置的数字

如果想要找第6个元素，就要判断(3,7),(4,6),(6,8)所处的位置

```
(1,1) 	(2,4) 	(3,7) 	(4,13)
(2,4) 	(4,6) 	(6,8) 	(8,14)
(3,7) 	(6,8) 	(9,9) 	(12,15)
(4,13)	(8,14)	(12,15)	(16,16)
```

​	不过直接定位还是有一点麻烦

换个思路，我们设置n个指针，放在每一行中，然后，将他的坐标同时保存，并且，放入堆中

每一次取出一个最小的，然后将这个指针右移，如果指针溢出，就不将右面的数组添加进堆中



举个例子

```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

首先，我们将第一列，也就是，1，10，12，连同坐标，放到一个堆里面：
[(1, 0, 0), (10, 1, 0), (12, 2, 0)]
然后我们取出最小的，(1,0,0),然后将5连同坐标放进去
[(5, 0, 1), (12, 2, 0), (10, 1, 0)]
然后重复上面的步骤，最后就得到第K个元素了
```



```python
class Solution:
    def kthSmallest(self, matrix, k):
        """
        :type matrix: List[List[int]]
        :type k: int
        :rtype: int
        """
        row = len(matrix)
        col = len(matrix[0])

        heap = [(matrix[i][0], i, 0) for i in range(row)]

        heapq.heapify(heap)
        for i in range(k - 1):
            temp = heapq.heappop(heap)
            if temp[2] + 1 < col:
                heapq.heappush(heap, (matrix[temp[1]][temp[2] + 1], temp[1], temp[2] + 1))

        res = heapq.heappop(heap)[0]
        return res
        
```

