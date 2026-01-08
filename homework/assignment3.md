# Assignment #3: Stack, DP & Backtracking

Updated 2226 GMT+8 Sep 22, 2025

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

### 1078: Bigram分词

https://leetcode.cn/problems/occurrences-after-bigram/

思路：
用split分开，之后依次比较每一个位置


代码：

```python
class Solution(object):
    def findOcurrences(self, text, first, second):
        """
        :type text: str
        :type first: str
        :type second: str
        :rtype: List[str]
        """
        f=0
        s=0
        t=0
        l=text.split()
        ans=[]
        while t<len(l)-2:
            if l[t]==first and l[t+1]==second:
                ans.append(l[t+2])
            t+=1
        return ans
```



代码运行截图 <mark>![alt text](image-13.png)</mark>





### 283.移动零

stack, two pinters, https://leetcode.cn/problems/move-zeroes/


思路：
双指针，前面的指针用于探测非0元素，后面的指针用于改变元素的值，当前面的指针到头的时候，后面的指针继续移动把剩余元素变为0


代码：

```python
class Solution(object):
    def moveZeroes(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        i=0
        j=0
        while j<len(nums):
            if nums[j]!=0:
                nums[i]=nums[j]
                i+=1
            j+=1
        while i<len(nums):
            nums[i]=0
            i+=1
```



代码运行截图 <mark>![alt text](image-14.png)</mark>





### 20.有效的括号

stack, https://leetcode.cn/problems/valid-parentheses/

思路：
用一个栈结构，配对成功的括号就出栈，如果加入右括号但是不能和栈顶的括号配对，就会配对失败


代码：

```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        l=[]
        for x in range(len(s)):
            if s[x]==")":
                if len(l)!=0 and l[-1]=='(':
                    l.pop()
                else:
                    return False
            elif s[x]=='}':
                if len(l)!=0 and l[-1]=='{':
                    l.pop()
                else:
                    return False
            elif s[x]==']':
                if len(l)!=0 and l[-1]=='[':
                    l.pop()
                else:
                    return False
            else:
                l.append(s[x])
        if len(l)==0:
            return True
        return False
```



代码运行截图 <mark>![alt text](image-15.png)</mark>





### 118.杨辉三角

dp, https://leetcode.cn/problems/pascals-triangle/

思路：
一个简单的递推


代码：

```python
class Solution(object):
    def generate(self, numRows):
        """
        :type numRows: int
        :rtype: List[List[int]]
        """
        ans=[]
        for i in range(numRows):
            ans.append([0]*(i+1))
        ans[0][0]=1
        i=1
        while i<numRows:
            ans[i][0]=1
            ans[i][i]=1
            for j in range(1,i):
                ans[i][j]=ans[i-1][j-1]+ans[i-1][j]
            i+=1
        return ans
```



代码运行截图 <mark>![alt text](image-16.png)</mark>





### 46.全排列

backtracking, https://leetcode.cn/problems/permutations/

思路：



代码

```python

```



<mark>（至少包含有"Accepted"）</mark>





### 78.子集

backtracking, https://leetcode.cn/problems/subsets/

思路：



代码

```python
# 

```



<mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和个人收获

<mark>练习了几道二分查找的题目，这类题目的特点就是求最大值的最小值，难点在于judge函数的写法</mark>





