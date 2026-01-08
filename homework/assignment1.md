# Assignment #1: 虚拟机，Shell & 大语言模型

Updated 1745 GMT+8 Sep 8, 2025

2025 fall, Complied by <mark>曹津睿数学科学学院 </mark>



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
>3. **延迟提交：****如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。  
>
>请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### E27653: Fraction类

http://cs101.openjudge.cn/pctbook/E27653/

请练习用OOP方式实现。

思路：
用时20分钟


代码：

```python
import math
def main():

    class Fraction():
        def __init__(self,a,b):
            d=math.gcd(a,b)
            a=a//d
            b=b//d
            self.num=a
            self.den=b
        def __add__(self,object):
            num1=self.num*object.den+self.den*object.num
            den1=self.den*object.den
            ans=Fraction(num1,den1)
            return ans
        def __str__(self):
            return str(self.num)+"/"+str(self.den)
    value=list(map(int,input().split()))
    a=Fraction(value[0],value[1])
    b=Fraction(value[2],value[3])
    print(a+b)
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image.png)</mark>





### M1760.袋子里最少数目的球

binary search, https://leetcode.cn/problems/minimum-limit-of-balls-in-a-bag/




思路：
二分查找


代码：

```python
class Solution(object):
    def minimumSize(self, nums, maxOperations):
        """
        :type nums: List[int]
        :type maxOperations: int
        :rtype: int
        """
        def check(nums,maxOperations,expected_max_in_each_bag):
            operating_times=0
            for i in nums:
                if i!=0:
                    
                    operating_times+=((i-1)//expected_max_in_each_bag)
                if operating_times>maxOperations:
                    return False
            return True
        low=1
        high=max(nums)+1
        ans=-1
        while low<high:
            mid=(high+low)//2
            if check(nums,maxOperations,mid):
                ans=mid
                high=mid
            else:
                low=mid+1
        return ans
```



代码运行截图 <mark>![alt text](image-1.png)</mark>





### M04135: 月度开销

binary search, http://cs101.openjudge.cn/pctbook/M04135/



思路：
用时25分，二分查找


代码：

```python
def main():
    def judge(t):
        total = 1
        sum = 0
        i = 0
        while i < N:
            if sum + expense[i] <=t:
                sum += expense[i]
                i += 1
            else:
                sum = expense[i]
                i += 1
                total += 1
                if total > M:
                    return False
        return True

    a=input().split()
    N=int(a[0])
    M=int(a[1])
    expense=[0]*N
    for i in range(N):
        expense[i]=int(input())
    low=max(expense)
    high=sum(expense)
    ans=high-1
    while low<high:
        mid=(low+high)//2
        if judge(mid):
            ans=mid
            high=mid
        else:
            low=mid+1
    print(ans)
if __name__=="__main__":
    main()

```



代码运行截图 <mark>![alt text](image-2.png)</mark>





### M27300: 模型整理

sortings, AI, http://cs101.openjudge.cn/pctbook/M27300/



思路：
这题Presentation Error了好久


代码：

```python
def main():
    n=int(input())
    all_models={}
    for i in range(n):
        model=(input())
        model_information=model.split('-')
        model_name=model_information[0]
        model_value=model_information[1]
        all_models.setdefault(model_name,[]).append(model_value)
    model_names=list(all_models.keys())
    model_names.sort()
    for x in model_names:
        x_values=all_models[x]
        x_values.sort(key=lambda m :float(m[:-1])*1000 if m[-1]=='B' else float(m[:-1]) )
        print(f"{x}: {', '.join(all_models[x])}", end="\n")
if __name__=="__main__":
    main()
```



代码运行截图 <mark>![alt text](image-3.png)</mark>





### Q5. 熟悉云虚拟机Linux环境与大语言模型（LLM）本地部署

本项目包括两个任务：

1）通过云虚拟机（如 https://clab.pku.edu.cn/ 提供的资源）熟悉Linux系统操作环境。

2）完成大语言模型（LLM）的本地部署与功能测试。

LLM 部署可选择使用图形化工具（如 LM Studio, https://lmstudio.ai）以简化配置流程，提升部署效率。部署完成后，需对模型进行实际能力测试。

测试内容包括：从主流在线编程评测平台（如 OpenJudge、Codeforces、LeetCode 或洛谷等）选取若干编程题目，提交由本地部署的 LLM 生成的代码解决方案，并确保其能够通过全部测试用例，获得“Accepted”状态。选题时应避免与已知可被 AI 正确解答的题目重复。当前已确认可通过的 AI 解题列表可参考以下 GitHub 仓库： 

https://github.com/GMyhf/2025spring-cs201/blob/main/AI_accepted_locally.md



请提供你的项目进展，内容应该包括：关键操作步骤的截图以及遇到的技术问题及相应的解决方法。这将有助于全面掌握项目推进情况，并为后续优化与扩展提供依据。





### Q6. 阅读《Build a Large Language Model (From Scratch)》第一章

阅读了一部分，记了一些要点
![alt text](image-4.png)
![alt text](image-5.png)
![alt text](image-6.png)



## 2. 学习总结和个人收获

<mark>做了一些基础题，学会了Python的一些基本语法</mark>





