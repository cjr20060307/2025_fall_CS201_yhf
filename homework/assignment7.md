# Assignment #7: bfs、🌲

Updated 0851 GMT+8 Oct 21, 2025

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

### M23555: 节省存储的矩阵乘法

implementation, matrices, http://cs101.openjudge.cn/practice/23555

要求用节省内存的方式实现，不能还原矩阵的方式实现。

思路：
a_m,k与b_k,n相乘，得到的数加到c_m,n位置
因此对于第一个矩阵中的每个元素，只需要找第二个矩阵中行数等于它的列数的元素，相乘
用一个字典来记录所有要加到(m,n)位置的数，最后把每个位置的所有数加起来


代码：

```python
def main():
    n,m1,m2=map(int,input().split())
    matrix1=[]
    matrix2=[]
    for _ in range(m1):
        matrix1.append(tuple(map(int,input().split())))
    for _ in range(m2):
        matrix2.append(tuple(map(int, input().split())))
    dic={}
    for i in range(len(matrix1)):
        a=matrix1[i][0]
        b=matrix1[i][1]
        val1=matrix1[i][2]
        for j in range(len(matrix2)):
            if matrix2[j][0]==b:
                c=matrix2[j][1]
                val2=matrix2[j][2]
                dic.setdefault((a,c),[]).append(val1*val2)
    not_0_element=list(dic.keys())
    ans=[(x[0],x[1],sum(dic[x])) for x in not_0_element]
    ans.sort()
    for t in range(len(ans)):
        print(str(ans[t][0])+' '+str(ans[t][1])+' '+str(ans[t][2]))
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-37.png)</mark>





### M102.二叉树的层序遍历

bfs, https://leetcode.cn/problems/binary-tree-level-order-traversal/


思路：
这题深搜也很简单


代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        ans=[]
        def dfs(root,depth):
            if root:
                if len(ans)<depth:
                    ans.append([])
                ans[depth-1].append(root.val)
                dfs(root.left,depth+1)
                dfs(root.right,depth+1)
        dfs(root,1)
        return ans
```



代码运行截图 <mark>![alt text](image-42.png)</mark>





### M131.分割回文串

dp, backtracking, https://leetcode.cn/problems/palindrome-partitioning/

思路：
最开始我的答案中出现重复
例如有一个输入为‘fff',我的输出中有两个'fff'
因为它既可以是f-->fff，也可以是f-->ff-->fff
解决方法是：如果之前的最后一个回文部分不是单字符，那么不考虑由它延长产生的结果，因为这个结果也是（被延长来得到它的那个单字符）的延长

代码：

```python
class Solution(object):
    def partition(self, s):
        """
        :type s: str
        :rtype: List[List[str]]
        """
        r = len(s)
        ans = []
        def divide(i, have_divided,p):
            if i == r:
                ans.append(have_divided[:])

            else:
                divide(i + 1, have_divided[:] + [s[i]],True)
                if p:
                    u = have_divided[-1]
                    for l in range(r - i ):
                        u += s[i + l ]
                        t = len(u)

                        judge = True
                        for m in range(t // 2 + 1):
                            if u[m] != u[t - m - 1]:
                                judge = False
                                break
                        if judge:
                            divide(l + i + 1, have_divided[:-1] + [u],False)

        divide(1, [s[0]],True)

        return ans
```



代码运行截图 <mark>![alt text](image-38.png)</mark>





### M200.岛屿数量

dfs, bfs, https://leetcode.cn/problems/number-of-islands/ 

思路：
每数一块陆地，就把它相连的所有陆地都移除，最后数出多少块陆地，就有多少个岛屿


代码

```python
class Solution(object):
    def numIslands(self, grid):
        """
        :type grid: List[List[str]]
        :rtype: int
        """
        m=len(grid)
        n=len(grid[0])
        def submerge(i,j):
            if grid[i][j]=='0':
                return None
            else:
                grid[i][j]='0'
                if i+1<m and grid[i+1][j]=="1":
                    submerge(i+1,j)
                if i-1>=0 and grid[i-1][j]=='1':
                    submerge(i-1,j)
                if j+1<n and grid[i][j+1]=='1':
                    submerge(i,j+1)
                if j-1>=0 and grid[i][j-1]=='1':
                    submerge(i,j-1)
        ans=0
        i=0
        j=0
        for i in range(m):
            for j in range(n):
                if grid[i][j]=="1":
                    ans+=1
                    submerge(i,j)
        return ans 
```



<mark>![alt text](image-39.png)</mark>





### 1123.最深叶节点的最近公共祖先

dfs, https://leetcode.cn/problems/lowest-common-ancestor-of-deepest-leaves/

思路：
深搜，用deepest记录出现过的最大深度，用paths记录当前时刻深度等于最大深度的元素的路径，最后dfs遍历完整个树时，paths就是最深元素的路径
然后找到paths中路径的前面的最大相同步数，对应的那个节点就是公共祖先


代码

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def lcaDeepestLeaves(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        deepest=[0]
        paths=[]
        def dfs(root,path_to_now):
            if root:#root is not None
                if len(path_to_now)>deepest[0]:#发现了一个更深的元素
                    deepest[0]=len(path_to_now)
                    paths.clear()           
                    paths.append(path_to_now)
                elif len(path_to_now)==deepest[0]:
                    paths.append(path_to_now)

                dfs(root.left,path_to_now[:]+[0])
                dfs(root.right,path_to_now[:]+[1])
        dfs(root,[])
        print(paths)
        if len(paths[0])==0:
            return root
        else:
            ans=root
            for i in range(deepest[0]):
                if all([x[i]==1 for x in paths]):
                    ans=ans.right
                elif all([x[i]==0 for x in paths]):
                    ans=ans.left
                else:
                    break
            return ans
```



<mark>![alt text](image-41.png)</mark>





### M79.单词搜索

回溯，https://leetcode.cn/problems/word-search/

思路：
DFS


代码：

```python
class Solution(object):
    def exist(self, board, word):
        """
        :type board: List[List[str]]
        :type word: str
        :rtype: bool
        """
        M=len(board)
        N=len(board[0])
        def step(m,n,i):#目前位置是（m，n），下一个要找第i+1个字符
            if i==len(word)-1:
                return True
            record=board[m][n]
            board[m][n]=0
            judge1,judge2,judge3,judge4=False,False,False,False
            if m+1<M and board[m+1][n]==word[i+1]:
                judge1=step(m+1,n,i+1)
            if m-1>=0 and board[m-1][n]==word[i+1]:
                judge2=step(m-1,n,i+1)
            if n+1<N and board[m][n+1]==word[i+1]:
                judge3=step(m,n+1,i+1)
            if n-1>=0 and board[m][n-1]==word[i+1]:
                judge4=step(m,n-1,i+1)
            board[m][n]=record
            return any([judge1,judge2,judge3,judge4])
        for i in range(M):
            for j in range(N):
                if board[i][j]==word[0]:
                    if step(i,j,0):
                        return True
        return False
```



代码运行截图 <mark>![alt text](image-40.png)</mark>



## 2. 学习总结和个人收获

<mark></mark>
![alt text](image-43.png)
![alt text](image-44.png)
![alt text](image-45.png)
![alt text](image-46.png)
![alt text](image-47.png)
![alt text](image-48.png)
![alt text](image-49.png)
![alt text](image-50.png)
![alt text](image-51.png)
![alt text](image-52.png)
![alt text](image-53.png)
![alt text](image-54.png)
![alt text](image-55.png)
![alt text](image-56.png)