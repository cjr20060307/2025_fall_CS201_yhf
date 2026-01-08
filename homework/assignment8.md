# Assignment #8: 🌲 (2/3)

Updated 2223 GMT+8 Oct 27, 2025

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

### E108.将有序数组转换为二叉搜索树

https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/

思路：
分而治之


代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        def assign(l:list):
            if len(l)==0:
                return None
            if len(l)==1:
                r=TreeNode(l[0])
                return r
            mid=len(l)//2
            left_l=l[0:mid]
            right_l=l[mid+1:]
            r=TreeNode(l[mid])
            r.left=assign(left_l)
            r.right=assign(right_l)
            return r
        return assign(nums)
```



代码运行截图 <mark>![alt text](image-60.png)</mark>





### M07161: 森林的带度数层次序列存储

tree, http://cs101.openjudge.cn/practice/07161/


思路：
这题是层序序列存储，也就是读取的时候，要把一层的所有兄弟节点都读完，才会开始继续读第一个兄弟节点的子节点
这可以用双端队列实现，
如果改为，读取的时候要优先把每一个子节点下面的所有节点都读完，再开始读下一个兄弟节点，
那么可以用stack实现


代码：

```python
from collections import deque
class TreeNode():
    def __init__(self,value=None):
        self.val=value
        self.branches=[]
def main():
    n=int(input())
    trees=[0]*n
    for i in range(n):
        s=input().split()
        to_add_branches=deque()
        for j in range(0,len(s),2):
            Node_value=s[j]
            degree=int(s[j+1])
            treenode=TreeNode(Node_value)
            if to_add_branches:
                parent=to_add_branches.popleft()
                parent.branches.append(treenode)
            else:
                trees[i]=treenode
            for t in range(degree):
                to_add_branches.append(treenode)
    def postorder(root):
        if root:
            for i in range(len(root.branches)):
                postorder(root.branches[i])
            print(root.val,end=' ')
    for m in range(n):
        postorder(trees[m])
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-61.png)</mark>





### M27928: 遍历树

 adjacency list, dfs, http://cs101.openjudge.cn/practice/27928/

思路：
由于输入不按顺序，所以每个节点的branches里没法直接储存它的子节点，我们可以在branches里储存子节点对应的数字，并且在输入时，branches就要排序，以方便之后确定遍历的顺序
再用TreeNode_dic去储存每个数字对应的节点
另一个关键是如何找到根节点，只要把输入中所有节点的子节点对应的数字并起来，没出现的数字就是不作为子节点出现的节点，也就是根节点
代码：

```python
class TreeNode():
    def __init__(self,value=None):
        self.val=value
        self.branches=None
def main():
    n=int(input())
    TreeNode_dic={}
    for i in range(n):
        s=input().split()
        if len(s)==1:
            t=TreeNode(s[0])
            TreeNode_dic[s[0]]=t
        else:
            t=TreeNode(s[0])
            b=s[1:]
            b.sort(key=lambda x:int(x))
            t.branches=b
            TreeNode_dic[s[0]]=t
    def dfs(root):
        if root:
            if not root.branches:
                print(int(root.val))

            elif root.branches:
                judge=1
                for i in range(0,len(root.branches)):
                    if judge and int(root.branches[i])>int(root.val) :
                        print(int(root.val))
                        judge=0
                    dfs(TreeNode_dic[root.branches[i]])
                if judge:
                    print(int(root.val))

    all_nodes=list(TreeNode_dic.keys())
    all_branches=list(TreeNode_dic.values())
    all_branches=[x.branches for x in all_branches if x.branches]
    for i in range(len(all_nodes)):
        judge=True
        for j in range(len(all_branches)):
            if all_nodes[i] in all_branches[j]:
                judge=False
        if judge:
            dfs(TreeNode_dic[all_nodes[i]])
            break
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-59.png)</mark>





### M129.求根节点到叶节点数字之和

dfs, https://leetcode.cn/problems/sum-root-to-leaf-numbers/

思路：
用字符串做，可以避免考虑数位的问题


代码

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumNumbers(self, root: Optional[TreeNode]) -> int:
        def dfs(root,s:str):
            if root:
                if not root.left and not root.right:
                    ans.append(s+str(root.val))
                dfs(root.left,s+str(root.val))
                dfs(root.right,s+str(root.val))
                
        ans=[]
        dfs(root,'')
        values=list(map(int,ans))
        return sum(values)
```



代码运行截图<mark>![alt text](image-57.png)</mark>





### M24729: 括号嵌套树

dfs, stack, http://cs101.openjudge.cn/practice/24729/

思路：



代码

```python
class TreeNode():
    def __init__(self,value=None):
        self.branches=None
        self.val=value
def main():
    s=input()
    def generate(s:str):
        if len(s)==1:
            return TreeNode(s)
        else:
            left_bracket=0
            last_separate=2
            separate=0
            branches_str=[]
            while separate<len(s):
                if s[separate]=='(':
                    left_bracket+=1
                elif s[separate]==')':
                    left_bracket-=1
                elif s[separate]==',' and left_bracket==1:
                    branches_str.append(s[last_separate:separate])
                    last_separate=separate+1
                separate+=1
            branches_str.append(s[last_separate:separate-1])
            b=[generate(x) for x in branches_str]
            present_Node=TreeNode(s[0])
            present_Node.branches=b
            return present_Node
    tree=generate(s)
    def root_first(root,ans=''):
        if root:
            ans+=root.val
            if root.branches:
                for i in range(len(root.branches)):
                    ans=root_first(root.branches[i],ans)
            return ans
    def branches_first(root,ans=''):
        if root:
            if root.branches:
                for i in range(len(root.branches)):
                    ans=branches_first(root.branches[i],ans)
            ans+=root.val
            return ans
    ans1=root_first(tree)
    ans2=branches_first(tree)
    print(ans1)
    print(ans2)
if __name__=="__main__":
    main()
```



代码运行截图<mark>![alt text](image-58.png)</mark>





### T02775: 文件结构“图”

tree, http://cs101.openjudge.cn/practice/02775/

思路：
文件夹dir是树的节点
为了在']'的时候更方便的返回上一级文件夹，在树节点里面添加parent方法，指向它的父节点

代码：

```python
class dir():
    def __init__(self,name='ROOT'):
        self.subdirs=[]
        self.files=[]
        self.name=name
        self.parent=None
def output(root:dir,depth=0):
    print('|     '*depth+root.name)
    for i in range(len(root.subdirs)):
        output(root.subdirs[i],depth+1)
    for i in range(len(root.files)):
        print('|     '*depth+root.files[i])
def main():
    case=0
    roots=[dir()]
    now_dir = roots[case]
    while True:
        a=input()
        if a=='#':
            break
        elif a=='*':
            if case:
                print('')
            roots.append(dir())#为下一个case做准备
            print('DATA SET '+str(case+1)+':')
            output(roots[case])
            case+=1
            now_dir=roots[case]
            continue
        else:
            if a[0]=='f':
                now_dir.files.append(a)
                now_dir.files.sort()
            elif a[0]=='d':
                next_dir=dir(a)
                now_dir.subdirs.append(next_dir)
                next_dir.parent=now_dir
                now_dir=next_dir
            elif a[0]==']':
                now_dir=now_dir.parent
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-62.png)</mark>



## 2. 学习总结和个人收获

<mark></mark>
![alt text](image-63.png)
![alt text](image-64.png)
![alt text](image-65.png)
![alt text](image-66.png)
![alt text](image-67.png)
![alt text](image-68.png)
![alt text](image-69.png)
![alt text](image-70.png)
![alt text](image-71.png)
![alt text](image-72.png)
![alt text](image-73.png)
![alt text](image-74.png)
![alt text](image-75.png)
![alt text](image-76.png)
![alt text](image-77.png)
![alt text](image-78.png)
![alt text](image-79.png)
![alt text](image-80.png)




