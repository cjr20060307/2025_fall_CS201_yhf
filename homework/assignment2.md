# Assignment #2: DSA & OOP

Updated 1748 GMT+8 Sep 15, 2025

2025 fall, Complied by <mark>曹津睿数学科学学院</mark>



**作业的各项评分细则及对应的得分**

| 标准                                 | 等级                                                         | 得分 |
| ------------------------------------ | ------------------------------------------------------------ | ---- |
| 按时提交                             | 完全按时提交：1分<br/>提交有请假说明：0.5分<br/>未提交：0分  | 1 分 |
| 源码、耗时（可选）、解题思路（可选） | 提交了4个或更多题目且包含所有必要信息：1分<br/>提交了2个或以上题目但不足4个：0.5分<br/>少于2个：0分 | 1 分 |
| AC代码截图                           | 提交了4个或更多题目且包含所有必要信息：1分<br/>提交了2个或以上题目但不足4个：0.5分<br/>少于：0分 | 1 分 |
| 清晰头像、PDF文件、MD/DOC附件        | 包含清晰的Canvas头像、PDF文件以及MD或DOC格式的附件：1分<br/>缺少上述三项中的任意一项：0.5分<br/>缺失两项或以上：0分 | 1 分 |
| 学习总结和个人收获                   | 提交了学习总结和个人收获：1分<br/>未提交学习总结或内容不详：0分 | 1 分 |
| 总得分： 5                           | 总分满分：5分                                                |      |
>
>
>
>**说明：**
>
>1. **解题与记录：**
>
>      对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
>2. **课程平台：**课程网站位于Canvas平台（https://pku.instructure.com ）。该平台将在<mark>第2周</mark>选课结束后正式启用。在平台启用前，请先完成作业并将作业妥善保存。待Canvas平台激活后，再上传你的作业。
>
>3. **提交安排：**提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的本人头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
>3. **延迟提交：**如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。  
>
>请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### 20742: 泰波拿契數

http://cs101.openjudge.cn/practice/20742/

思路：
用一个数组rem去记忆，避免重复计算


代码：

```python
def main():
    n=int(input())
    if n==0:
        print(0)
    elif n==2 or n==1:
        print(1)
    else:
        rem=[0]*(n+1)
        rem[1]=1
        rem[2]=1
        for i in range(3,n+1):
            rem[i]=rem[i-1]+rem[i-2]+rem[i-3]
        print(rem[n])
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-7.png)</mark>





### 58A. Chat room

greedy/strings, 1000, http://codeforces.com/problemset/problem/58/A


思路：
从头依次往后比较


代码：

```python
def main():
    s=input()
    next_letter=0
    hello=['h','e','l','l','o']
    for i in range(len(s)):
        if s[i]==hello[next_letter]:
            next_letter+=1
            if next_letter==5:
                print("YES")
                return None
    print("NO")
    return None
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-9.png)</mark>





### 118A. String Task

implementation/strings, 1000, http://codeforces.com/problemset/problem/118/A

思路：
str.lower()方法将字符串变为小写


代码：

```python
def main():
    yuanyin=['a','e','i','o','u','A','E',"I","O","U","y","Y"]
    a=input()
    ans=""
    for i in range(len(a)):
        if a[i] not in yuanyin:
            ans+='.'
            ans+=a[i].lower()
    print(ans)
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-8.png)</mark>





### 22359: Goldbach Conjecture

http://cs101.openjudge.cn/practice/22359/

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### E23563: 多项式时间复杂度

http://cs101.openjudge.cn/pctbook/E23563/

思路：
注意以0开头的


代码

```python
def main():
    a=input().split('+')
    ans=0
    for i in range(len(a)):
        b=a[i].split("n^")
        if b[0]!='0':
            ans=max(ans,int(b[1]))
    print("n",end='^')
    print(str(ans))

if __name__=="__main__":
    main()
```



<mark>![alt text](image-10.png)</mark>





### 24684: 直播计票

http://cs101.openjudge.cn/practice/24684/

思路：



代码

```python
# 

```



<mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和个人收获

<mark>平时做了一些题目，并把思路或者要点记在了本子上![alt text](image-11.png)![alt text](image-12.png)</mark>





