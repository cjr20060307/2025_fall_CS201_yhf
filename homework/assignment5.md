# Assignment #5: cs201 Mock Exam寒露第三天

Updated 1913 GMT+8 Oct 10, 2025

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

### E29952: 咒语序列

Stack, http://cs101.openjudge.cn/practice/29952/

思路：
关键点在于最长子串长度的更新方法，用一个变量last_matched记录上次不匹配的位置（初始值为-1）
如果当前栈为空，那么目前的子串长度为i-last_matched
如果当前栈非空，目前子串的长度为i-stack[-1]


代码：

```python
def main():
    s=input()
    ans=0
    last_matched=-1
    stack=[]
    right_blacket=[')','}',']']
    for i in range(len(s)):
        if s[i] in right_blacket:
            if len(stack)==0 or any([(s[i]=='}' and s[stack[-1]]!='{'),(s[i]==']' and s[stack[-1]]!='['),(s[i]==')' and s[stack[-1]]!='(')]):
                last_matched=i
                stack.clear()
            else:
                stack.pop()
                if len(stack)==0:
                    ans=max(ans,i-last_matched)
                else:
                    ans=max(ans,i-stack[-1])
        else:
            stack.append(i)
    print(ans)
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-21.png)</mark>





### M01328: Radar Installation

greedy, http://cs101.openjudge.cn/practice/01328/


思路：
最开始想错了，以为雷达的坐标都是整数


代码：

```python
from math import sqrt
def main():
    u=0
    while True:
        u+=1
        n,d=map(int,input().split())
        if n==0 and d==0:
            break
        islands=[]
        for _ in range(n):
            islands.append(list(map(int,input().split())))
        get_nothing=input()
        islands.sort(key=lambda x:-x[1])#纵坐标从大到小
        if islands[0][1]>d:
            print("Case "+str(u)+': '+'-1')
            continue
        interval=[(x[0]-sqrt(d*d-x[1]*x[1]),x[0]+sqrt(d*d-x[1]*x[1])) for x in islands]
        interval.sort(key=lambda x:x[1])
        ans=1
        location=interval[0][1]
        for i in range(len(interval)):
            if location<interval[i][0]:
                ans+=1
                location=interval[i][1]

        print("Case "+str(u)+': '+str(ans))
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-22.png)</mark>





### M02754: 八皇后

dfs, http://cs101.openjudge.cn/practice/02754/

思路：
已知前几行的放置情况后，下一行的8个数中满足1不与前几行的数相同2不与前几行的数处于同一条斜线，即行差等于列差，即可将这个数算作答案中的一个情况


代码：

```python
def main():
    ans=[]
    def judge(s,i):#判断当前面len（string）行的放置情况确定后，下一行能不能放在第i个位置(只判断斜着的）
        for j in range(1,len(s)+1):
            if (len(s)+1-j)==i-int(s[j-1]) or  -(len(s)+1-j)==i-int(s[j-1]):
                return False
        return True
    def dnf(s):
        if len(s)==8:
            ans.append(s)
        else:
            for i in range(1,9):
                if (str(i) not in s) and judge(s,i):
                    dnf(s+str(i))
    dnf('')
    n=int(input())
    for t in range(n):
        m=int(input())
        print(int(ans[m-1]))
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-23.png)</mark>





### M25570: 洋葱

matrices, http://cs101.openjudge.cn/practice/25570/

思路：
递归


代码：

```python
def main():
    n=int(input())
    onion=[]
    for i in range(n):
        onion.append(list(map(int,input().split())))
    def peel(onion):
        if len(onion)==1:
            return onion[0][0]
        elif len(onion)==2:
            return onion[0][1]+onion[0][0]+onion[1][0]+onion[1][1]
        else:
            total=sum(onion[0])+sum(onion[-1])
            new_onion=[]
            for i in range(1,len(onion)-1):
                total+=onion[i][0]
                total+=onion[i][-1]
                new_onion.append(onion[i][1:-1])

            return max(total,peel(new_onion))
    print(peel(onion))
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-24.png)</mark>







### M29954: 逃离紫罗兰监狱

bfs, http://cs101.openjudge.cn/practice/29954/

思路：



代码

```python

```



<mark>（至少包含有"Accepted"）</mark>





### T27256: 当前队列中位数

backtracking, http://cs101.openjudge.cn/practice/27256/

思路：



代码

```python

```



<mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和个人收获

<mark>![alt text](image-25.png)![alt text](image-26.png)![alt text](image-27.png)![alt text](image-28.png)![alt text](image-29.png)</mark>





