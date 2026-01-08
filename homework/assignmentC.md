# Assignment #C: 图 (2/4)

Updated 2329 GMT+8 Nov 24, 2025

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

### M909.蛇梯棋

bfs, https://leetcode.cn/problems/snakes-and-ladders/


思路：
这题将数字转换成坐标的方法是要好好想想的
另外，BFS将节点加入visited的时机是在节点入队时，而不是出队时（否则一个节点会加入队列多次导致超时），这点也要注意

代码：

```python
class Solution(object):
    def snakesAndLadders(self, board):
        """
        :type board: List[List[int]]
        :rtype: int
        """
        from collections import defaultdict
        from collections import deque
        class Graph:
            def __init__(self,n):
                self.vertices=defaultdict(set)
                self.number=n
            def add_edge(self,fv,tv):
                self.vertices[fv].add(tv)
        n=len(board)
        g=Graph(n)
        locations={}
        def locate(m,n):
            if (m,n) in locations:
                return locations[(m,n)]
            if m>n*n:
                return None,None
            row=(m-1)//n
            col=(m-1)%n
            if row%2==1:
                col=n-col-1
            row=n-row-1
            locations[(m,n)]=(row,col)
            return (row,col)
        def build_graph(n,m):
            for i in range(6,0,-1):
                x,y=locate(n+i,m)
                if x!=None:
                    if board[x][y]!=-1:
                        g.add_edge(n,board[x][y])
                    else:
                        g.add_edge(n,n+i)
        for i in range(1,n*n):
            build_graph(i,n)
        queue=deque([(1,0)])
        visited=set([1])
        while queue:
            now,time=queue.popleft()
            for x in g.vertices[now]:
                if x not in visited:
                    if x==n*n:
                        return time+1
                    else:
                        queue.append((x,time+1))
                        visited.add(x)
        return -1
```



代码运行截图 <mark>![alt text](image-102.png)</mark>





### sy382: 有向图判环 中等

dfs, topological sort, https://sunnywhy.com/sfbj/10/3/382


思路：
dfs,用visited记录访问过的所有节点，用另一个集合stack来记录当前路径（虽然是集合，但是逻辑上确实是后进先出）
另解：拓扑排序，只将入度为0的节点加入栈，如果最终访问的结点数小于总结点数，说明有环

代码：

```python
from collections import deque
from collections import defaultdict
class graph:
    def __init__(self,n):
        self.vertices=defaultdict(list)
        self.number=n
    def add_edge(self,fv,tv):
        self.vertices[fv].append(tv)
    def is_cyclic(self,v,visited,stack):
        visited.add(v)
        stack.add(v)
        for x in self.vertices[v]:
            if x not in visited:
                if self.is_cyclic(x,visited,stack):
                    return True
            elif x in stack:
                return True
        stack.remove(v)
        return False
    def is_cyclic_whole(self):
        visited=set()

        for i in range(self.number):
            stack=set()
            if i not in visited:
                if self.is_cyclic(i,visited,stack):
                    return True
        return False
def main():
    n,m=map(int,input().split())
    g=graph(n)
    for i in range(m):
        fv,tv=map(int,input().split())
        g.add_edge(fv,tv)
    if g.is_cyclic_whole():
        print("Yes")
    else:
        print("No")
if __name__=="__main__":
    main()
'''
from collections import deque
from collections import defaultdict
class graph:
    def __init__(self,n):
        self.vertices=defaultdict(list)
        self.number=n
    def add_edge(self,fv,tv):
        self.vertices[fv].append(tv)
    def is_cyclic(self):
        in_degree=[0]*self.number
        for u in self.vertices:
            for v in self.vertices[u]:
                in_degree[v]+=1
        queue=deque()
        for i in range(self.number):
            if in_degree[i]==0:
                queue.append(i)
        count=0
        visited=set()
        while queue:
            now=queue.popleft()
            visited.add(now)
            count+=1
            for u in self.vertices[now]:
                if u not in visited:
                    in_degree[u]-=1
                    if in_degree[u]==0:
                        queue.append(u)
        return self.number!=count
def main():
    n,m=map(int,input().split())
    g=graph(n)
    for i in range(m):
        fv,tv=map(int,input().split())
        g.add_edge(fv,tv)
    if g.is_cyclic():
        print("Yes")
    else:
        print("No")
if __name__=="__main__":
    main()
'''
```



代码运行截图 <mark>![alt text](image-101.png)</mark>





### M28046: 词梯

bfs, http://cs101.openjudge.cn/practice/28046/

思路：
将这个问题拆解为四步，就很清晰了
1，读入数据
2，创建桶
3，在每个桶内单词之间建边
4，BFS
代码：

```python
import sys
from collections import deque
def main():
    all_words=sys.stdin.readlines()
    last_line=all_words[-1]
    all_words=all_words[1:-1]
    start,end=last_line.split()

    buckets={}
    for line in all_words:
        word=line.strip()
        for i,_ in enumerate(word):
            bucket=f'{word[0:i]}_{word[i+1:]}'
            buckets.setdefault(bucket,set()).add(word)

    graph={}
    for similar_words in buckets.values():
        for word1 in similar_words:
            for word2 in similar_words-{word1}:
                graph.setdefault(word1,set()).add(word2)


    stack=deque([(start,[start])])
    visited=set([start])
    judge=1
    while stack:
        now,path=stack.popleft()
        if now==end:
            print(' '.join(path))
            judge=0
            break
        else:
            for x in graph[now]:
                if x not in visited:
                    visited.add(x)
                    stack.append((x,path+[x]))
    if judge:
        print("NO")
    return 0
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-99.png)</mark>





### M433.最小基因变化

bfs, https://leetcode.cn/problems/minimum-genetic-mutation/

思路：
建了一个图


代码

```python
from collections import deque
class Solution(object):
    def minMutation(self, startGene, endGene, bank):
        """
        :type startGene: str
        :type endGene: str
        :type bank: List[str]
        :rtype: int
        """
        class vertex:
            def __init__(self,key):
                self.key=key
                self.neighbors={}#key:str value:vertex
            def set_neighbor(self,other):
                self.neighbors[other.key]=other
        class Graph:
            def __init__(self):
                self.vertices={}#key:str value:vertex
            def add_vertex(self,key):
                self.vertices[key]=vertex(key)
            def add_verge(self,from_vertex,to_vertex):
                if from_vertex not in self.vertices:
                    self.add_vertex(from_vertex)
                if to_vertex not in self.vertices:
                    self.add_vertex(to_vertex)
                self.vertices[from_vertex].set_neighbor(self.vertices[to_vertex])
                self.vertices[to_vertex].set_neighbor(self.vertices[from_vertex])
        bank.append(startGene)
        if endGene not in bank:
            return -1
        g=Graph()
        for i in range(len(bank)):
            for j in range(i+1,len(bank)):
                count=0
                for k in range(8):
                    if bank[i][k]!=bank[j][k]:
                        count+=1
                    if count>1:
                        break
                if count==1:
                    g.add_verge(bank[i],bank[j])
            
        queue=deque([(startGene,0)])
        visited=set([])
        while queue:
            now,time=queue.popleft()
            for x in g.vertices[now].neighbors:
                if x not in visited:
                    visited.add(x)
                    queue.append((x,time+1))
                    if x ==endGene:
                        return time+1
        return -1
```



代码运行截图<mark>![alt text](image-98.png)</mark>





### M05443: 兔子与樱花

Dijkstra, http://cs101.openjudge.cn/practice/05443/

思路：
竟然一次就AC了


代码

```python
from collections import deque
from collections import defaultdict
class vertex:
    def __init__(self,key:str):
        self.key=key
        self.neighbors=defaultdict(int)
    def add_neighbor(self,other:str,dis:int):
        self.neighbors[other]=dis
class Graph:
    def __init__(self):
        self.vertices={}#str:vertex
    def add_vertex(self,key:str):
        self.vertices[key]=vertex(key)
    def add_edge(self,fv:str,tv:str,dis:int):
        self.vertices[fv].add_neighbor(tv,dis)
        self.vertices[tv].add_neighbor(fv,dis)
def main():
    def output(ans):
        n=len(ans)
        distance=[0]*(n-1)
        for i in range(n-1):
            distance[i]=g.vertices[ans[i]].neighbors[ans[i+1]]
        for i in range(n-1):
            print(ans[i]+'->('+f'{str(distance[i])}'+')->',end='')
        print(ans[-1])
    g=Graph()
    p=int(input())
    for i in range(p):
        key=input()
        g.add_vertex(key)
    q=int(input())
    for i in range(q):
        fv,tv,dis=input().split()
        dis=int(dis)
        g.add_edge(fv,tv,dis)
    r=int(input())
    for _ in range(r):
        start,end=input().split()
        visited=set(start)
        paths=[]
        if start==end:
            print(start)
            continue
        queue=deque([(start,[start],0)])#3元组，(now,path,distance)
        while queue:
            now,path,distance=queue.popleft()
            for neighbor in g.vertices[now].neighbors:
                if neighbor not in path:

                    if neighbor == end:
                        paths.append((path+[end],distance+g.vertices[now].neighbors[end]))
                    else:
                        queue.append((neighbor,path+[neighbor],distance+g.vertices[now].neighbors[neighbor]))
        paths.sort(key=lambda x:x[1])
        ans=paths[0][0]
        output(ans)
if __name__=="__main__":
    main()
```



代码运行截图<mark>![alt text](image-103.png)</mark>





### M28050: 骑士周游

dfs, http://cs101.openjudge.cn/practice/28050/

思路：
先遍历所有点建图
然后dfs，用一个visited集合来记录当前走过的格点，在当前格点的所有邻居dfs结果都是false时，从visited中删去当前格点来回溯
dfs返回True的边界条件是参数time==n*n-1，即走到了最后一个格点
每个格点遍历neighbor的顺序采用了Warnsdorff规则


代码：

```python
def main():
    n=int(input())
    x,y=map(int,input().split())
    graph={}
    directions=[(-1,2),(1,2),(-2,1),(2,1),(-2,-1),(2,-1),(-1,-2),(1,-2)]
    for i in range(n):
        for j in range(n):
            for dx,dy in directions:
                if 0<=i+dx<n and 0<j+dy<n:
                    graph.setdefault((i,j),set()).add((i+dx,j+dy))
    visited={(x,y)}
    def worst_vai(locate:tuple,n):
        x,y=locate
        next_step=[]
        for dx,dy in directions:
            if 0<=x+dx<n and 0<=y+dy<n and(x+dx,y+dy) not in visited:
                c=0
                for di, dj in directions:
                    if 0 <= x + dx+di < n and 0 <= y + dy +dj< n and (x + dx+di, y + dy+dj) not in visited:
                        c+=1
                next_step.append((c,(x+dx,y+dy)))
        next_step.sort(key=lambda x:x[0])
        return [x[1] for x in next_step]

    def dfs(locate:tuple,time,n):
        if time==n*n-1:
            return True
        x,y=locate
        visited.add((x,y))
        for a,b in worst_vai(locate,n):
            if dfs((a,b),time+1,n):
                return True



        else:
            visited.discard((x,y))
        return False

    if dfs((x,y),0,n):
        print('success')
    else:
        print('fail')
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-100.png)</mark>



## 2. 学习总结和个人收获

<mark></mark>





