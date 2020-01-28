# 题目:表示数值的字符串
## 题目描述：
请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。
# 本题考点：
  
  字符串编程能力，正则表达式的理解，思维的全面性。
  
# 解题思路:
  
  1.) 回溯 
  先分析下如何匹配一个字符，现在只考虑字符'.'，不考虑'\*'看一下：

如果字符串和模式串的当前字符相等，那么我们继续匹配它们的下一个字符；如果模式串中的字符是'.'，那么它可以匹配字符串中的任意字符，我们也可以继续匹配它们的下一个字符。

接下来，把字符'\*'考虑进去，它可以匹配任意次的字符，当然出现0次也可以。

我们分两种情况来看：

模式串的下一个字符不是'\*'，也就是上面说的只有字符'.'的情况。
如果字符串中的第一个字符和模式串中的第一个字符相匹配，那么字符串的模式串都向后移动一个字符，然后匹配剩余的字符串和模式串。如果字符串中的第一个字符和模式中的第一个字符不相匹配，则直接返回false。

模式串的下一个字符是'\*'，此时就要复杂一些。
因为可能有多种不同的匹配方式。

选择一：无论字符串和模式串当前字符相不相等，我们都将模式串后移两个字符，相当于把模式串中的当前字符和'\*'忽略掉，因为'\*'可以匹配任意次的字符，所以出现0次也可以。

选择二：如果字符串和模式串当前字符相等，则字符串向后移动一个字符。而模式串此时有两个选择：

1、我们可以在模式串向后移动两个字符，继续匹配；

2、也可以保持模式串不变，这样相当于用字符'\*'继续匹配字符串，也就是模式串中的字符'\*'匹配字符串中的字符多个的情况。

用一张图表示如下：

![](https://github.com/bryceustc/CodingInterviews/blob/master/RegularExpressionsMatching/Image/1.png)

除此之外，还要注意对空指针的处理


复杂度分析：
![](https://github.com/bryceustc/CodingInterviews/blob/master/RegularExpressionsMatching/Image/%E5%A4%8D%E6%9D%82%E5%BA%A6%E5%88%86%E6%9E%90.png)
# 代码

[C++](./NumericStrings.cpp)

[Python](./NumericStrings.py)

# C++: 
###
```c++

/*
    首先，考虑特殊情况：
         1>两个字符串都为空，返回true
         2>当第一个字符串不空，而第二个字符串空了，返回false（因为这样，就无法
            匹配成功了,而如果第一个字符串空了，第二个字符串非空，还是可能匹配成
            功的，比如第二个字符串是“a*a*a*a*”,由于‘*’之前的元素可以出现0次，
            所以有可能匹配成功）
    之后就开始匹配第一个字符，这里有两种可能：匹配成功或匹配失败。但考虑到pattern
    下一个字符可能是‘*’， 这里我们分两种情况讨论：pattern下一个字符为‘*’或
    不为‘*’：
          1>pattern下一个字符不为‘*’：这种情况比较简单，直接匹配当前字符。如果
            匹配成功，继续匹配下一个；如果匹配失败，直接返回false。注意这里的
            “匹配成功”，除了两个字符相同的情况外，还有一种情况，就是pattern的
            当前字符为‘.’,同时str的当前字符不为‘\0’。
          2>pattern下一个字符为‘*’时，稍微复杂一些，因为‘*’可以代表0个或多个。
            这里把这些情况都考虑到：
               a>当‘*’匹配0个字符时，str当前字符不变，pattern当前字符后移两位，
                跳过这个‘*’符号；
               b>当‘*’匹配1个或多个时，str当前字符移向下一个，pattern当前字符
                不变。（这里匹配1个或多个可以看成一种情况，因为：当匹配一个时，
                由于str移到了下一个字符，而pattern字符不变，就回到了上边的情况a；
                当匹配多于一个字符时，相当于从str的下一个字符继续开始匹配）
    之后再写代码就很简单了。
*/
class Solution {
public:
    bool match(char* str, char* pattern)
    {
        if (*str == '\0' && *pattern == '\0')
            return true;
        if (*str != '\0' && *pattern == '\0')
            return false;
        if (*(pattern +1) !='*')
        {
            if (* str == * pattern || (*str !='\0'&& *pattern =='.'))
                return match(str+1,pattern+1);
            else
                return false;
        }
        else
        {
            if (* str == * pattern || (*str!='\0')&& *pattern == '.')
                return (match(str,pattern+2) || match(str+1,pattern));
            else
                return (match(str,pattern+2));
        }
    }
};
```

### 方法二：动态规划
```c++
class Solution {
public:
    bool isMatch(string s, string p) {
        int n = s.size();
        int m = p.size();
        vector<vector<bool>> dp(n+1,vector<bool>(m+1,false));
        dp[0][0]=true;
        s = ' '+s;
        p = ' '+p;
        for (int i=0;i<=n;i++)  // i从0开始，因为dp(0,j)有可能是有意义的
        {
            for(int j=1;j<=m;j++)
            {
                if (i>0 && (s[i]==p[j] || p[j]=='.'))
                {
                    dp[i][j] = dp[i][j] | dp[i-1][j-1];  // | 表示按位或
                }
                if (p[j]=='*')
                {
                    if (j>=2)
                    {
                        dp[i][j] = dp[i][j] | dp[i][j-2];
                    }
                    if (i>0 && (s[i]==p[j-1] || p[j-1]=='.'))
                    {
                        dp[i][j] = dp[i][j] | dp[i-1][j];
                    }
                }
            }
        }
        return dp[n][m];        
    }
};
```
# Python:
### 方法一：回溯法
```python
# -*- coding:utf-8 -*-
class Solution:
    # s, pattern都是字符串
    def match(self, s, p):
        # write code here
        if len(s)==0 and len(p)==0:
            return True
        if len(s)!=0 and len(p)==0:
            return False
        if  len(p)>1 and p[1]=='*':
            if len(s)>0 and (s[0] == p[0] or p[0]=='.'):
                return self.match(s,p[2:]) or self.match(s[1:],p)
            else:
                return self.match(s,p[2:])
        if len(s)>0 and (s[0] == p[0] or p[0]=='.'):
            return self.match(s[1:],p[1:])
        else:
            return False
```
### 方法二：动态规划
```python
# -*- coding:utf-8 -*-
class Solution:
    # s, pattern都是字符串
    def match(self, s, p):
        # write code here
        n = len(s)
        m = len(p)
        dp = [[0 for _ in range(m+2)] for _ in range(n+2)]
        s = ' ' + s
        p = ' ' + p
        dp[0][0] = 1
        for i in range(n+1):
            for j in range(1,m+1):
                if i > 0 and (s[i]==p[j] or p[j]=='.'):
                    dp[i][j] = dp[i][j] or dp[i-1][j-1]
                if p[j]=='*':
                    if j>=2:
                        dp[i][j] = dp[i][j] or dp[i][j-2]
                    if i > 0 and (s[i]==p[j-1] or p[j-1]=='.'):
                        dp[i][j] = dp[i][j] or dp[i-1][j]
        return dp[n][m]
```


