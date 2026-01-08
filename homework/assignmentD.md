# Assignment #D: Mock Exam

Updated 1955 GMT+8 Dec 5, 2025

2025 fall, Complied by <mark>曹津睿数学科学学院</mark>



>**说明：**
>
>1. Dec⽉考： AC6<mark>3</mark> 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
>
>2. 解题与记录：对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
>3. 提交安排：提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的本人头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
> 
>4. 延迟提交：如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。  
>
>请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。





## 1. 题目

### E02734:十进制到八进制 

http://cs101.openjudge.cn/practice/02734

思路：



代码

```python
def main():
    n=int(input())
    stack=[]
    while n:
        b=n%8
        n=n//8
        stack.append(b)
    ans=''
    while stack:
        s=str(stack.pop())
        ans=ans+s
    print(int(ans))

if __name__ == "__main__":
    main()
```



代码运行截图 <mark>![alt text](image-104.png)</mark>





### M21509:序列的中位数

heap, http://cs101.openjudge.cn/practice/21509

思路：



代码

```python
import heapq
def main():
    n=int(input())
    read=list(map(int,input().split()))
    heap1=[(-read[0],read[0])]
    heap2=[]
    heapq.heapify(heap1)
    heapq.heapify(heap2)
    print(read[0])
    i=1
    while i<len(read)-1:
        a,b=read[i],read[i+1]
        if a>=b:
            a,b=b,a
        if b<=heap1[0][1]:
            _,p=heapq.heappop(heap1)
            heapq.heappush(heap1,(-a,a))
            heapq.heappush(heap1,(-b,b))
            heapq.heappush(heap2,(p,p))
        elif heap2 and a>=heap2[0][0]:
            _, p = heapq.heappop(heap2)
            heapq.heappush(heap2, (a, a))
            heapq.heappush(heap2, (b, b))
            heapq.heappush(heap1, (-p, p))
        else:
            heapq.heappush(heap1,(-a, a) )
            heapq.heappush( heap2,(b, b))
        print(heap1[0][1])
        i+=2
if __name__ == "__main__":
    main()
```



代码运行截图 <mark>![alt text](image-105.png)</mark>





### M27306: 植物观察

disjoint set, bfs, http://cs101.openjudge.cn/practice/27306/

思路：



代码

```python
class Unionfind:
    def __init__(self,size):
        self.value=list(range(2*size))
    def find(self,start):
        while self.value[start]!=self.value[self.value[start]]:
            self.value[start]=self.find(self.value[start])
        return self.value[start]
    def examine(self,a,b):
        if self.find(a)==self.find(b):
            return True
        else:
            return False
    def union(self,a,b):
        if self.find(a)!=self.find(b):
            self.value[a]=self.value[b]
        return None
def main():
    n,m=map(int,input().split())
    u=Unionfind(n)
    for _ in range(m):
        a,b,judge=map(int,input().split())
        if not judge:
            if u.examine(a,b+n) or u.examine(a+n,b):
                print("NO")
                return None
            u.union(a,b)
            u.union(a+n,b+n)
        else:
            if u.examine(a,b) or u.examine(a+n,b+n):
                print("NO")
                return None
            u.union(a,b+n)
            u.union(b,a+n)
    print("YES")


if __name__ == "__main__":
    main()
```



代码运行截图 <mark>![alt text](image-106.png)</mark>





### M29740:神经网络

Topological order, http://cs101.openjudge.cn/practice/29740/


思路：
有两个易错点
一是计算入度时重复计算
二是拓扑排序时，邻居入度减少并不依赖于当前神经元被成功激活


代码

```python
class Vertex:
    def __init__(self,key):
        self.key=key
        self.neighbors={}#int:int
    def add_neighbor(self,neighbor,w):
        if neighbor in self.neighbors:
            u=self.neighbors[neighbor]
            self.neighbors[neighbor]=u+w
        else:
            self.neighbors[neighbor]=w
class Graph:
    def __init__(self,n):
        self.vertices=[Vertex(i) for i in range(n+1)]
    def add_edge(self,fv,tv,w):
        p=self.vertices[fv]
        p.add_neighbor(tv,w)
def main():

    n,m=map(int,input().split())
    g=Graph(n)
    in_degree=[0]*(n+1)
    in_degree[0]=-1
    out_degree=[0]*(n+1)
    states=[0]*(n+1)
    ui=[0]*(n+1)
    for i in range(1,n+1):
        a,b=map(int,input().split())
        states[i]=a
        ui[i]=b
    for _ in range(m):
        fv,tv,w=map(int,input().split())

        if tv not in g.vertices[fv].neighbors:
            in_degree[tv]+=1
            out_degree[fv]+=1
        g.add_edge(fv,tv,w)
    total=0
    output=[x for x in range(1,n+1) if out_degree[x]==0]
    in_put=[x for x in range(1,n+1) if in_degree[x]==0]
    for x in in_put:
        ui[x]=0
    ans=[]
    while total<n:
        has_ring=True#如果还有节点未访问，但是却没有入度为0的点，说明有环
        for i in range(1,n+1):
            if in_degree[i]==0:
                in_degree[i]=-1
                has_ring=False
                if states[i]-ui[i]>0:
                    out=states[i]-ui[i]
                    if i in output:
                        ans.append((i,out))
                    for y in g.vertices[i].neighbors:
                        states[y]+=out*g.vertices[i].neighbors[y]
                for y in g.vertices[i].neighbors:
                    in_degree[y]-=1
                break
        if has_ring:
            print("NULL")
            return None
        total+=1
    if not ans:
        print("NULL")
        return None
    else:
        ans.sort()
        for x,y in ans:
            print(x,y)


if __name__ == "__main__":
    main()
```



代码运行截图 <mark>![alt text](image-107.png)</mark>





### T27351:01最小生成树 

mst, http://cs101.openjudge.cn/practice/27351/

思路：



代码

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### T30193:哈密顿激活层

DFS+剪枝, http://cs101.openjudge.cn/practice/30193/

思路：



代码

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

如果作业题目简单，有否额外练习题目，比如：OJ“计概2025fall每日选做”、CF、LeetCode、洛谷等网站题目。





