# Assignment #B: 图 (1/4)

Updated 2031 GMT+8 Nov 17, 2025

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

### E07218: 献给阿尔吉侬的花束

bfs, http://cs101.openjudge.cn/practice/07218/

思路：
广度优先搜索，在遍历到一个节点的时候立刻做标记，而不是从队列中移除的时候才做标记


代码：

```python
from collections import deque
def main():
    directions=[(0,1),(0,-1),(1,0),(-1,0)]
    cases=int(input())
    for _ in range(cases):
        grid=[]
        m,n=map(int,input().split())
        start=None
        end=None
        for i in range(m):
            grid.append(list(input().strip()))
            for j in range(n):
                if grid[-1][j]=='S':
                    start=(i,j,0)
        queue=deque([start])
        grid[start[0]][start[1]]='#'
        while queue:
            x,y,times=queue.popleft()
            for dx,dy in directions:
                nx=x+dx
                ny=y+dy
                if 0<=nx<m and 0<=ny<n and grid[nx][ny]!='#':
                    if grid[nx][ny]=="E":
                        print(times+1)
                        end=(nx,ny)
                        break
                    grid[nx][ny]='#'
                    queue.append((nx,ny,times+1))

            if end:
                break
        if not end :
            print("oop!")
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-94.png)</mark>





### M27925: 小组队列

dict, queue, http://cs101.openjudge.cn/practice/27925/


思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### M04089: 电话号码

trie, http://cs101.openjudge.cn/practice/04089/

思路：
有三种情况会导致冲突：
1，当前单词已经全部填入Trie，但是节点now仍然有子节点
2，当前单词还没填完，但是节点now已经是单词末尾
3，当前单词恰好填完，在把节点now标记为末尾时，发现它已经被标记过（两单词重复）


代码：

```python
class TreeNode:
    def __init__(self):
        self.is_end=False
        self.children={}#key:letter value:TreeNode
class Trie:
    def __init__(self):
        self.root=TreeNode()
    def add_word(self,word:str):
        now=self.root
        for i in range(len(word)):
            if now.is_end==True:
                    return False
            if word[i] in now.children:
                    now=now.children[word[i]]
            else:
                next_node=TreeNode()
                now.children[word[i]]=next_node
                now=next_node
        if now.is_end==True:
            return False
        now.is_end=True
        if now.children:
            return False
        return True
def main():
    n=int(input())
    for i in range(n):
        m=int(input())
        tt=Trie()
        valid=True
        for j in range(m):
            word=input()
            judge=tt.add_word(word)
            if not judge:
                valid=False
        if valid:
            print("YES")
        else:
            print("NO")
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-96.png)</mark>





### M3532.针对图的路径存在性查询I

disjoint set, https://leetcode.cn/problems/path-existence-queries-in-a-graph-i/

思路：
并查集，对每个数字a都找到和它联通的最大数字f(a)，两个数字a,b连通等价于他们对应的最大数字f(a)==f(b)
另外，为了避免重复地比较，我们需要注意到如果一个数字a的前一个数字a-1对应的最大数字f(a-1)超过了a,那么一定有f(a)=f(a-1)


代码

```python
class Solution(object):
    def pathExistenceQueries(self, n, nums, maxDiff, queries):
        """
        :type n: int
        :type nums: List[int]
        :type maxDiff: int
        :type queries: List[List[int]]
        :rtype: List[bool]
        """
        l=list(range(n))
        def find(i):
            if i>=1 and l[i]<=l[i-1]:
                l[i]=l[i-1]
                return l[i]
            while l[i]+1<len(nums) and nums[l[i]]>=nums[l[i]+1]-maxDiff:
                l[i]+=1
            return l[i]
        for i in range(n):
            find(i)
        return ([l[x[0]]==l[x[1]] for x in queries]) 
```



代码运行截图<mark>![alt text](image-97.png)</mark>





### M19943: 图的拉普拉斯矩阵

OOP, graph, implementation, http://cs101.openjudge.cn/pctbook/E19943/

要求创建Graph, Vertex两个类，建图实现。

思路：



代码

```python
class Vertex:
    def __init__(self,key):
        self.neighbors={}
        self.key=key
    def set_neighbor(self,other):
        self.neighbors[other]=0
        other.neighbors[self]=0
class Graph:
    def __init__(self):
        self.vertices={}
    def set_vertex(self,key):
        self.vertices[key]=Vertex(key)
    def add_edge(self,key1,key2):
        self.vertices[key1].set_neighbor(self.vertices[key2])
def main():
    g=Graph()
    n,m=map(int,input().split())
    for i in range(n):
        g.set_vertex(i)
    for i in range(m):
        key1,key2=map(int,input().split())
        g.add_edge(key1,key2)
    ans=[]
    for _ in range(n):
        ans.append([0]*n)
    for i in range(n):
        for j in range(n):
            if g.vertices[j] in g.vertices[i].neighbors:
                ans[i][j]=-1
    for i in range(n):
        ans[i][i]+=len(g.vertices[i].neighbors)
    for i in range(n):
        print(' '.join(list(map(str,ans[i]))))
if __name__=="__main__":
    main()
```



代码运行截图<mark>![alt text](image-95.png)</mark>





### T25353: 排队

http://cs101.openjudge.cn/pctbook/T25353/

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



## 2. 学习总结和个人收获

<mark></mark>





