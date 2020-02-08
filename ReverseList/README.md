# 题目: 反转链表
## 题目描述：
输入一个链表，反转链表后，输出新链表的表头。

# 本题考点：
  
  链表编程能力，递归与非递归方法的掌握
  
# 解题思路:
此题与LeetCode第206题一样，与剑指offer第6题从尾到头打印链表类似。

  方法一：非递归法，利用两个结点指针和一个中间结点指针temp(用来记录当前结点的下一个节点的位置)，分别指向当前结点和前一个结点，每次循环让当前结点的指针域指向前一个结点。
  
  方法二：递归法。
  
  先反转后面的链表，从最后面的两个结点开始反转（递归都是要递归到最深一层），依次向前，将后一个链表结点指向前一个结点，注意每次反转后要将原链表中前一个结点的指针域置空，表示将原链表中前一个结点指向后一个结点的指向关系断开。
  
# 代码

[C++](./ReverseList.cpp)

[Python](./ReverseList.py)

# C++: 
### 方法一：非递归法
```c++
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* ReverseList(ListNode* head) {
        if (head==NULL || head->next==NULL)
            return head;
        ListNode* prev = NULL;
        ListNode* cur = head;
        ListNode* temp = head->next;
        while(cur)
        {
            temp = cur->next;
            cur->next = prev;
            prev = cur;
            cur = temp;
        }
        return prev;
    }
};
```
### 方法二：递归法
```c++
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* ReverseList(ListNode* head) {
        if (head==NULL || head->next==NULL)
        {
            return head;
        }
        else
        {
            ListNode* newhead = ReverseList(head->next);// 先反转后面的链表，从最后面两个节点开始，
            head->next->next = head; // 将后一个链表节点指向前一个链表节点
            head->next = NULL;  // 将原链表中前一个节点指向后一个节点的指向关系断开
            return newhead;  
        }
    }
};
```

# Python:
### 方法一：非递归法
```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    # 返回ListNode
    def ReverseList(self, head):
        # write code here
        if head == None or head.next == None:
            return head
        prev = None
        cur = head
        temp = head.next
        while cur:
            temp = cur.next
            cur.next = prev
            prev = cur
            cur = temp
        return prev
```
### 方法二：递归法
```python
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    # 返回ListNode
    def ReverseList(self, head):
        # write code here
        if head == None or head.next == None:
            return head
        else:
            newhead = self.ReverseList(head.next)
            head.next.next = head
            head.next = None
            return newhead
```
## 参考
  -  [LeetCode—206题—反转链表](https://github.com/bryceustc/LeetCode_Note/blob/master/cpp/Reverse-Linked-List/README.md)
  -  [剑指offer—6题—从尾到头打印链表](https://github.com/bryceustc/CodingInterviews/blob/master/PrintListInReversedOrder/README.md)
