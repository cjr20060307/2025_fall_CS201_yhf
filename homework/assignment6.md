# Assignment #6: 链表、栈和排序

Updated 2143 GMT+8 Oct 13, 2025

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

### E24588: 后序表达式求值

Stack, http://cs101.openjudge.cn/practice/24588/

思路：
需要注意输出格式，利用字符串格式化'{:.2f}'.format()
另外题中给的乘除优先于加减的条件好像没有用到


代码：

```python
def main():
    def cal(a,b,s):
        if s=='+':
            return a+b
        elif s=='-':
            return b-a
        elif s=='*':
            return a*b
        elif s=='/':
            return b/a
    signal=['+','-','*','/']
    n=int(input())
    for i in range(n):
        formular=input().split()
        s=0#记录符号个数
        t=0#记录数字个数
        to_be_cal=[]
        for j in range(len(formular)):
            if formular[j] in signal:
                a=to_be_cal.pop()
                b=to_be_cal.pop()
                to_be_cal.append(cal(a,b,formular[j]))
            else:
                to_be_cal.append(float(formular[j]))
        print("{:.2f}".format(to_be_cal[0]))
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-33.png)</mark>





### M234.回文链表

linked list, https://leetcode.cn/problems/palindrome-linked-list/

<mark>请用快慢指针实现</mark> `O(1)` 空间复杂度。


思路：
先用快慢指针找到链表中点，然后反转后半链表，然后与前半链表依次比较


代码：

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def isPalindrome(self, head):
        """
        :type head: Optional[ListNode]
        :rtype: bool
        """
        i=head
        j=head
        j=j.next
        while j:
            j=j.next
            if j==None:
                break
            j=j.next
            i=i.next#如果长度是偶数，i停在中间线左侧
        def reverse(head):
            if head==None or head.next==None:
                return head
            rev=reverse(head.next)
            tail=head.next
            tail.next=head
            head.next=None
            return rev
        t=i.next
        r=reverse(t)
        p=head
        while r:
            if p.val!=r.val:
                return False
            p=p.next
            r=r.next
        return True
```



代码运行截图 <mark>![alt text](image-30.png)</mark>





### M27217: 有多少种合法的出栈顺序

http://cs101.openjudge.cn/practice/27217/

思路：
一共2n步操作，只要两种操作的顺序不同，结果就不同。任意时刻push操作的次数必须大于等于pop操作的次数
因此答案就是卡特兰数，直接带入公式C2n,n/n+1即可


代码：

```python
def main():
    n=int(input())
    ans=1
    for i in range(n):
        ans*=(n+i+1)
    for i in range(n):
        ans=ans//(i+1)
    print(ans//(n+1))
if __name__=="__main__":
    main()
#又用正常方法写了一遍
'''
def main():
    n=int(input())
    mem={}
    def count(push,pop):#已经做了push步入栈和pop步出栈之后，还有多少种可能的情形
        if (push,pop) in mem:
            return mem[(push,pop)]
        elif push<pop:
            return 0
        elif push==n:
            return 1
        else:
            mem[(push,pop)]=count(push+1,pop)+count(push,pop+1)
            return mem[(push,pop)]
    for i in range(n,-1,-1):
        count(i,i)
    ans=mem[(0,0)]
    print(ans)
if __name__=="__main__":
    main()
'''
```



代码运行截图 <mark>![alt text](image-31.png)![alt text](image-32.png)</mark>





### M24591:中序表达式转后序表达式

http://cs101.openjudge.cn/practice/24591/

思路：
注意到所有数字之间的顺序不变，因此要做的就是重新将运算符插入这些数字
如果运算符后面没有'(',则直接将这个运算符加到下个数字的后面，如果后面有括号，则等到所有括号都匹配完成后再加这个运算符
2*（3+4）而且，在同一个位置加入的符号（比如这个式子在4后），应该是越晚出现的符号，在结果中越靠前
代码

```python
def main():
    n=int(input())
    signal=['+','-','*','/']
    brackets=['(',')']
    for i in range(n):
        read=['']
        to_be_inserted=[]
        s=input()
        match=[]#match和to_be_inserted里的符号是一一对应的，match里为0就说明括号已匹配，此时可以插入这个符号
        judge=[]#judge是记录运算符出现之后是否至少已经出现了一个数字，防止括号骗出运算符
        for j in range(len(s)):
            if s[j] not in signal and s[j] not in brackets:
                read[-1]+=s[j]
                if judge:
                    judge[-1]=True
            else:
                if read and read[-1] :
                    read.append('')
                while match and (not match[-1]) and judge[-1] and not(to_be_inserted[-1] in ["+",'-'] and s[j] in ['*','/']):
                    read[-1]+=to_be_inserted[-1]
                    match.pop()
                    to_be_inserted.pop()
                    judge.pop()
                    read.append('')
                if s[j] in signal:
                    match.append(0)
                    judge.append(False)
                    to_be_inserted.append(s[j])
                else:
                    if s[j]=='(':
                        match=[x+1 for x in match]
                    else:
                        match=[x-1 for x in match]
        if not read[-1]:
            read.pop()
        while to_be_inserted:
            read.append(to_be_inserted[-1])
            to_be_inserted.pop()
        print(' '.join(read))

if __name__=="__main__":
    main()
```



<mark>![alt text](image-34.png)</mark>





### M02299:Ultra-QuickSort

merge sort, http://cs101.openjudge.cn/practice/02299/

思路：
每一次交换都会使整体的逆序数减1，因此问题等价于求一开始的逆序数
用分治策略求逆序数，时间复杂度是nlogn
分成左右两部分，两部分之间的逆序数的求法容易出错

代码

```python
def main():
    def count(l:list):
        if len(l)==1:
            return 0
        else:
            mid=len(l)//2
            a=count(l[:mid])
            b=count(l[mid:])
            between=0
            left=l[:mid]
            left.sort()
            right=l[mid:]
            right.sort()
            i=0
            j=0
            while i<len(left) and j<len(right):
                if left[i]>right[j]:
                    between+=len(left)-i
                    j+=1
                else:
                    i+=1
            return between+a+b

    while True:
        n=int(input())
        if n==0:
            break
        l=[]
        for i in range(n):
            l.append(int(input()))
        ans=count(l)
        print(ans)
if __name__=="__main__":
    main()
```



<mark>![alt text](image-35.png)</mark>





### M146.LRU缓存

hash table, doubly-linked list, https://leetcode.cn/problems/lru-cache/

思路：
建立一个双向链表，建立一个字典，字典的值是键在链表中的位置
以哨兵节点为起点和结尾，当链表长度超出时，只需删除尾部哨兵节点的前一个节点，当调用put方法时，先通过字典确定键对应节点的位置，然后把这个节点移到头部哨兵节点之后
另外，最好将对链表的操作封装成独立方法
代码：

```python
class ListNode():
    def __init__(self,key=None,value=None):
        self.next=None
        self.pre=None
        self.k=key
        self.v=value
    

class LRUCache(object):

    def __init__(self, capacity):
        """
        :type capacity: int
        """
        self.cache={}
        self.head=ListNode()
        self.tail=ListNode()
        self.head.next=self.tail
        self.tail.pre=self.head
        self.size=capacity
    def add_node(self,p):
        q=self.head.next
        self.head.next=p
        p.next=q
        q.pre=p
        p.pre=self.head
        self.cache[p.k]=p
    def del_node(self,p):
        a=p.pre
        b=p.next
        a.next=b
        b.pre=a
        del self.cache[p.k]
    

    def get(self, key):
        """
        :type key: int
        :rtype: int
        """
        if key in self.cache:
            a=self.cache[key].v
            p=self.cache[key]
            self.del_node(p)
            self.add_node(p)
            return a
        else:
            return -1
        

    def put(self, key, value):
        """
        :type key: int
        :type value: int
        :rtype: None
        """
        if key in self.cache:
            self.cache[key].v=value
            p=self.cache[key]
            self.del_node(p)
            self.add_node(p)
        else:
            p=ListNode(key,value)
            self.add_node(p)
            if len(self.cache)>self.size:
                self.del_node(self.tail.pre)

        
                

            
        


# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```



代码运行截图 <mark>![alt text](image-36.png)</mark>



## 2. 学习总结和个人收获

<mark>![alt text](a0b7e2545f8193a636fc728af890d563.jpg)</mark>
在In Love这道题我学会了懒删除的方法，以及如果想要让heapq.push按照指定函数排序，可以将元素写成（f(x),x)的元组的方法





