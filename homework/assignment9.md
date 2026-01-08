# Assignment #9: Mock Exam立冬

Updated 1856 GMT+8 Nov 7, 2025

2025 fall, Complied by <mark>曹津睿数学科学学院</mark>



>**说明：**
>
>1. Nov⽉考： AC6<mark>2</mark> 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
>
>2. 解题与记录：对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
>3. 提交安排：提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的本人头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
> 
>4. 延迟提交：如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。  
>
>请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。





## 1. 题目

### M02255: 重建二叉树

http://cs101.openjudge.cn/practice/02255/

思路：
1，前序遍历的第一个字母永远是当前的根节点
2，将中序遍历按照当前根节点分开，则左右两部分分别是当前左子树和右子树的中序遍历，
3，再按2中得到的左右子树节点个数，将前序遍历也分成两部分，就得到了左右子树的前序遍历
4，重复这个过程，构建出整个树


代码

```python
class TreeNode():
    def __init__(self,value=None):
        self.left=None
        self.right=None
        self.val=value
def main():
    def generate(preorder:str,inorder:str):
        if len(preorder)==1:
            root=TreeNode(preorder[0])
            return root
        elif len(preorder)==0:
            return None
        root=TreeNode(preorder[0])
        inorder1,inorder2=inorder.split(preorder[0])
        l=len(inorder1)
        preorder1,preorder2=preorder[1:1+l],preorder[1+l:]
        root.left=generate(preorder1,inorder1)
        root.right=generate(preorder2,inorder2)
        return root
    def postorder(root):
        if root:
            postorder(root.left)
            postorder(root.right)
            ans[0]+=root.val

    while True:
        try:
            preorder,inorder=input().split()
            root=generate(preorder,inorder)
            ans=['']
            postorder(root)
            print(ans[0])
        except EOFError:
            break
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-85.png)</mark>





### M02774: 木材加工

http://cs101.openjudge.cn/practice/02774/

思路：
二分查找


代码

```python
def main():
    def judge(mid,K):
        total = 0
        for i in range(len(log)):
            total+=log[i]//mid
        if total<K:
            return False
        else:
            return True
    N,K=map(int,input().split())
    log=[]
    for i in range(N):
        log.append(int(input()))
    low=1
    high=max(log)
    ans=0
    while low<high:
        mid=(low+high)//2
        if judge(mid,K):
            ans=mid
            low=mid+1
        else:
            high=mid
    print(ans)
if __name__=='__main__':
    main()
```



代码运行截图 <mark>![alt text](image-81.png)</mark>





### M02788: 二叉树（2）

http://cs101.openjudge.cn/practice/02788/

思路：
并不用构建树


代码

```python
def  main():
    while True:
        m,n=map(int,input().split())
        if m==0 and n==0:
            break
        else:
            i=0
            j=0
            while m>=2**i:
                i+=1
            while n>=2**j:
                j+=1
            if i==j:
                print(1)
            else:
                gap=j-i
                left=m*(2**gap)
                right=left+(2**gap)-1
                if n<left:
                    ans=2**gap-1
                elif n>right:
                    ans=2**(gap+1)-1
                else:
                    ans=2**gap-1+n-left+1
                print(ans)
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-86.png)</mark>





### M04081: 树的转换 

http://cs101.openjudge.cn/practice/04081/


思路：
首先看到u这个返回父节点的操作，就要在建立树节点类的时候加一个方法，来指向它的父节点
其次，这道题并不用真的把树转换成长子兄弟，因为子节点在branches中的索引大小就可以反映出它在长子兄弟表示下，离根节点有多远
因此用depth和distant两个方法来记录一个节点的深度和到根节点的距离（长子兄弟表示下的深度）
depth就是父节点的depth  ＋  1
ditant就是父节点的distant  +  该节点在父节点branches中的索引  ＋  1

代码

```python
class TreeNode():
    def __init__(self):
        self.branches=[]
        self.father=None
        self.distant=0
        self.depth=0

def main():
    s=input()
    now_root=TreeNode()
    ans=0
    depth=0
    for i in range(len(s)):
        if s[i]=='d':
            p=TreeNode()
            p.father=now_root
            now_root.branches.append(p)
            p.distant=p.father.distant+len(p.father.branches)
            p.depth=p.father.depth+1
            ans=max(ans,p.distant)
            depth=max(depth,p.depth)
            now_root=p
        elif s[i]=='u':
            now_root=now_root.father
    print(str(depth)+' => '+str(ans))
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-87.png)</mark>





### M04117: 简单的整数划分问题

dfs, dp, http://cs101.openjudge.cn/practice/04117/

思路：
考试的时候被这题给的输入样例坑了，以为输入只有一行
改成接收多组输入后又遇到了超时的问题，用一个字典mem记忆就解决了


代码

```python
def main():
    def separate(n,m):
        if (n,m) in mem:
            return mem[(n,m)]
        
        if n==0:
            return 1
        elif n<0:
            return 0
        total=0
        for i in range(1,m+1):
            total+=separate(n-i,i)
        mem[(n,m)]=total
        return total
    mem={}
    while True:
        try:
            n=int(input())
            ans=separate(n,n)
            print(ans)
        except EOFError:
            break
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-84.png)</mark>





### M04137:最小新整数

monotonous-stack, http://cs101.openjudge.cn/practice/04137/

思路：
递归地列出所有可能，选出最小的


代码

```python
def main():
    def dfs(n,t,s):
        if len(s)==t:
            ans[0]=min(ans[0],int(s))
        if n:
            dfs(n[1:],t,s+n[0])
        if len(n)>=1:
            dfs(n[1:],t,s)

    x=int(input())
    for _ in range(x):
        n,k=input().split()
        m=len(n)
        k=int(k)
        ans=[int(n)]
        dfs(n,m-k,'')
        print(ans[0])

if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-83.png)</mark>





## 2. 学习总结和收获

如果作业题目简单，有否额外练习题目，比如：OJ“计概2025fall每日选做”、CF、LeetCode、洛谷等网站题目。
这周一直在准备期中考试
之前您对我作业的评论我已收到，感谢您的解答！





