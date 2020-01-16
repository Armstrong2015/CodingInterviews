# 题目描述:圆圈中最后剩下的数字
## 题目：
0,1，···n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。
# 本题考点：
  
  约瑟夫环问题
  
# 解题思路:
  
  1.) 用数组列表模拟一下。创建一个数组存储0-n-1的数组，但数组长度大于1时，就计算
  时间复杂度:O(n),空间复杂度:O(n)

# 代码

[C++](./LastNumberInCircle.cpp)

[Python](./LastNumberInCircle.py)

# C++:
```c++
#include <iostream>
#include <vector>
using namespace std;

class Solution {
public:
    int LastRemaining_Solution(int n, int m)
    {
        int res =-1;
        if (n<=0 || m<0)
            return res;
        vector<int> nums;
        for (int i=0;i<=n;i++)
        {
            nums.push_back(i);
        }
        int start =0;
        while(nums.size()>1)
        {
            int end = (start+m-1)%(nums.size());
            nums.erase(nums.begin()+end);
            start = end;
        }
        res = nums[0];
        return res;
    }
};

int main()
{
    int n = 5;
    int m = 3;
    int res = 0;
    res = Solution().LastRemaining_Solution(n,m);
    cout <<res << endl;
    system("pause");
    return 0;
}
```

# Python:
```python
# -*- coding:utf-8 -*-
class Solution:
    def LastRemaining_Solution(self, n, m):
        # write code h;
        res = -1
        if n<=0 or m<0:
            return res
        nums =range(n)## 0-n-1数组
        start=0
        while len(nums)>1:
            end = (start+m-1)%(len(nums))
            del nums[end]
            start = end
        return nums[0]

if __name__ == '_ main__':
    n=5
    m=3
    res = Solution().LastRemaining_Solution(n,m)    
    print(res)
```
