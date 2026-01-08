# Assignment #4: Matrix, 链表 & Backtracking

Updated 2226 GMT+8 Sep 29, 2025

2025 fall, Complied by <mark>曹津睿数学科学学院</mark>





>**说明：**
>
>1. **解题与记录：**
>
>     对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
>2. **提交安排：**提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的本人头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
> 
>3. **延迟提交：**如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。  
>
>请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### E18161: 矩阵运算

matrices, http://cs101.openjudge.cn/pctbook/E18161/

请使用`@`矩阵相乘运算符。

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### E19942: 二维矩阵上的卷积运算

matrices, http://cs101.openjudge.cn/pctbook/E19942/


思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### M06640: 倒排索引

data structures, http://cs101.openjudge.cn/pctbook/M06640/

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### E160.相交链表

two pinters, https://leetcode.cn/problems/intersection-of-two-linked-lists/

思路：
两个链表都一次走一个，走到头时就再从另一个链表的开头继续，两者一定会在交点处相遇


代码：

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """
        p=headA
        q=headB
        while p or q:
            if p==q:
                return p
            if p:
                p=p.next
            else:
                p=headB
            if q:
                q=q.next
            else:
                q=headA
        return None
```



代码运行截图 <mark>![alt text](image-17.png)</mark>





### E206.反转链表

three pinters, recursion, https://leetcode.cn/problems/reverse-linked-list/

思路：
递归，注意最后要把head的下一个设置为None，否则会成环


代码

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def reverseList(self, head):
        """
        :type head: Optional[ListNode]
        :rtype: Optional[ListNode]
        """
        if head==None or head.next==None:
            return head
        rev_head=self.reverseList(head.next)
        tail=head.next
        tail.next=head
        head.next=None
        return rev_head
```



<mark>![alt text](image-18.png)</mark>





### T02488: A Knight's Journey

backtracking, http://cs101.openjudge.cn/practice/02488/

思路：



代码

```python
# 

```



<mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和个人收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025fall每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>





