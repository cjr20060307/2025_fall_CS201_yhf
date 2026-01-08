# Assignment #A: 递归回溯、🌲 (3/4)

Updated 2203 GMT+8 Nov 3, 2025

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

### T51.N皇后

backtracking, https://leetcode.cn/problems/n-queens/

思路：



代码：

```python
class Solution(object):
    def solveNQueens(self, n):
        """
        :type n: int
        :rtype: List[List[str]]
        """
        ans=[]
        def put(l, i):  # l记录前i行放置的位置，l是字符串构成的列表，已经放置了i行,序号为0到i-1
            if i == n:
                ans.append(l)
            else:
                for j in range(n):
                    if all([x[j] != 'Q' for x in l]):
                        judge=0
                        for t in range(i):
                            if j-i+t>=0 and l[t][j+t-i]=='Q':
                                judge+=1
                            if j+i-t<n and l[t][j+i-t]=='Q':
                                judge+=1
                            if judge!=0:
                                break
                        if judge==0:
                            if j==0:
                                next_row = 'Q' + '.' * (n - 1)
                            else:
                                next_row='.'*j+'Q'+'.'*(n-j-1)
                            put(l[:]+[next_row[:]], i + 1)

        put([], 0)
        return ans
```



代码运行截图 <mark>![alt text](image-88.png)</mark>





### M22275: 二叉搜索树的遍历

http://cs101.openjudge.cn/practice/22275/


思路：
分而治之


代码：

```python
class TreeNode:
    def __init__(self,value=None):
        self.left=None
        self.right=None
        self.val=value
def main():
    def postorder(root):
        if root:
            postorder(root.left)
            postorder(root.right)
            ans.append(str(root.val))
    n=int(input())
    s=list(map(int,input().split()))
    def generate(s):
        if len(s)==0:
            return None
        else:
            p=TreeNode(s[0])
            i=1
            while i<len(s) and s[i]<s[0]:
                i+=1
            s1=s[1:i]
            s2=s[i:]
            p.left=generate(s1)
            p.right=generate(s2)
            return p
    root=generate(s)
    ans=[]
    postorder(root)
    print(' '.join(ans))
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-89.png)</mark>





### M25145: 猜二叉树（按层次遍历）

http://cs101.openjudge.cn/practice/25145/

思路：
和月考题一样
分而治之


代码：

```python
from collections import deque
class TreeNode():
    def __init__(self,value=None):
        self.left=None
        self.right=None
        self.val=value
def main():
    def generate(postorder:str,inorder:str):
        if len(postorder)==1:
            root=TreeNode(postorder[0])
            return root
        elif len(postorder)==0:
            return None
        root=TreeNode(postorder[-1])
        inorder1,inorder2=inorder.split(postorder[-1])
        l=len(inorder1)
        postorder1,postorder2=postorder[0:l],postorder[l:-1]
        root.left=generate(postorder1,inorder1)
        root.right=generate(postorder2,inorder2)
        return root

    n=int(input())
    for _ in range(n):
        inorder=input()
        postorder=input()
        root=generate(postorder,inorder)
        ans=''
        stack=deque([root])
        while stack:
            for i in range(len(stack)):
                ans+=stack[0].val
                if stack[0].left:
                    stack.append(stack[0].left)
                if stack[0].right:
                    stack.append(stack[0].right)
                stack.popleft()
        print(ans)
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-90.png)</mark>





### T20576: printExp（逆波兰表达式建树）

http://cs101.openjudge.cn/practice/20576/

思路：
这是到目前为止做过的最难的一题
思路就是 
1、Shunting Yard转后序 2、构建解析树 3、用递归函数实现打印
其中
1、Shunting Yard转后序要注意，在把运算符压入stack前不仅要移除stack尾部更高级的运算符，还要移除同级运算符
2、用一个stack实现，遇到True和False直接构建节点压入stack，遇到not取出最后一个节点，将其变为not为根的节点的左子节点，再将not节点压入，遇到or和and的话就是取出两个节点分别做成左右子节点
3、用递归实现，比较当前节点和左右子节点的优先级来决定是否要加括号。注意not和or，and的输出格式不一样，not是'not'在前，左子节点在后，而另外两个是‘左’+‘or’+‘右’的形式

代码

```python
class TreeNode:
    def __init__(self,value=None):
        self.left=None
        self.right=None
        self.father=None
        self.val=value

def main():
    signals=['not','or','and']
    order={'not':3,'or':1,'and':2,'True':4,'False':4,'(':0}
    s=input().split()
    stack=[]
    postorder=[]
    for i in range(len(s)):
        if s[i]=='(':
            stack.append(s[i])
        elif s[i]==')':
            while stack[-1]!='(':
                postorder.append(stack.pop())
            stack.pop()
        elif s[i] in signals:

            while stack and order[s[i]]<=order[stack[-1]]:
                postorder.append(stack.pop())
            stack.append(s[i])
        else:
            postorder.append(s[i])
    while stack:
        postorder.append(stack.pop())

    for i in range(len(postorder)):
        if postorder[i] in ['True','False']:
            p=TreeNode(postorder[i])
            stack.append(p)
        elif postorder[i] =='not':
            p=stack.pop()
            q=TreeNode('not')
            q.left=p
            stack.append(q)
        else:
            q=stack.pop()
            p=stack.pop()
            r=TreeNode(postorder[i])
            r.left=p
            r.right=q
            stack.append(r)
    root=stack[-1]

    def printout(root):
        l=[]
        r=[]
        if root.val in ['or','and']:
            l=printout(root.left)
            if order[root.left.val]<order[root.val]:
                l=['(']+l+[')']
            r=printout(root.right)
            if order[root.right.val]<order[root.val]:
                r=['(']+r+[')']
        elif root.val == 'not':
            l = printout(root.left)
            if order[root.left.val] < order[root.val]:
                l = ['('] + l + [')']
            return ['not']+l

        return l+[root.val]+r
    ans=printout(root)
    print(' '.join(ans))
if __name__=="__main__":
    main()
```



代码运行截图<mark>![alt text](image-91.png)</mark>





### T04080:Huffman编码树

greedy, http://cs101.openjudge.cn/practice/04080/

思路：
不断合并两个最小权重的子树，并将他们权重的和作为它们父节点的权重
这样得到的树结构就是使得加权和最小的树结构，最后计算一下加权和即可


代码

```python
import heapq
class TreeNode:
    def __init__(self,weight=0):
        self.left=None
        self.right=None
        self.weight=weight
        self.value=weight
    def __lt__(self,other):
        return True
def main():
    n=int(input())
    w=list(map(int,input().split()))
    heap0=[TreeNode(x) for x in w]
    heap=[(x.weight,x) for x in heap0]
    heapq.heapify(heap)
    while len(heap)>1:
        q=heapq.heappop(heap)[1]
        p=heapq.heappop(heap)[1]
        r=TreeNode()
        r.left=p
        r.right=q
        r.weight=p.weight+q.weight
        heapq.heappush(heap,(r.weight,r))
    def cal(root,depth=0):
        if root:
            return root.value*depth+cal(root.left,depth+1)+cal(root.right,depth+1)
        else:
            return 0
    ans=cal(heap[0][1],0)
    print(ans)
if __name__=="__main__":
    main()
```



代码运行截图<mark>![alt text](image-92.png)</mark>





### M04078: 实现堆结构

http://cs101.openjudge.cn/practice/04078/

要求手搓堆实现。

思路：
1、列表作为堆，节点存放逻辑：索引为i的节点的子节点索引为2*i+1，2*i+2
2、用上浮和下沉操作来维护堆中节点的序，注意下沉操作要将父节点与更小的那个子节点互换，而不能任选一个


代码：

```python
from collections import deque
def main():
    def up():
        now=len(heap)-1
        while now>0:
            if heap[(now-1)//2]>heap[now]:
                heap[(now-1)//2],heap[now]=heap[now],heap[(now-1)//2]
                now=(now-1)//2
            else:
                break
        return None
    def down():
        now=0
        while 2*now+1<len(heap):
            if 2*now+2>=len(heap):
                smaller=2*now+1
            else:
                if heap[2*now+1]>heap[2*now+2]:
                    smaller=2*now+2
                else:
                    smaller=2*now+1
            if heap[now]>heap[smaller]:
                heap[now],heap[smaller]=heap[smaller],heap[now]
                now=smaller
            else:
                break
        return None
    def heappush(element):
        heap.append(element)
        up()
        return None
    def heappop():
        heap[0],heap[-1]=heap[-1],heap[0]
        a=heap.pop()
        print(a)
        down()
        return None
    heap=deque([])
    n=int(input())
    for i in range(n):
        operation=input().split()
        if len(operation)==2:
            k=int(operation[1])
            heappush(k)
        else:
            heappop()
if __name__=="__main__":
    main()

```



代码运行截图 <mark>![alt text](image-93.png)</mark>



## 2. 学习总结和个人收获

<mark>学习笔记以PDF形式上传</mark>





