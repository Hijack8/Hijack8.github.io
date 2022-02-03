---
title: Floyd判圈算法-双指针
date: 2022-02-03 11:28:26
tags:
- 算法
- 双指针
categories:
- 算法
---

## 题目描述

给你一个链表的头节点 head ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。注意：pos 不作为参数进行传递 。仅仅是为了标识链表的实际情况。

如果链表中存在环 ，则返回 true 。 否则，返回 false 。

[力扣（LeetCode）]: https://leetcode-cn.com/problems/linked-list-cycle

<!--more-->

## 题解

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *p1 = head;
        ListNode *p2 = head;
        while(p1!= NULL && p2!=NULL && p1->next != NULL){
            p1=p1->next->next;
            p2 = p2->next;
            if(p1 == p2)return true;
        }
        return false;
    }
};
```

## 分析

这就是**Floyd判圈算法**
考虑相对位移步数， 一个指针运动1步/次，另一个指针运动2步/次，所以说二者的相对运动为1步/次，因此如果有环，二者在一圈之内必定相遇。

