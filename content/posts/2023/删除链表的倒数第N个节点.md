---
title: "删除链表的倒数第N个节点"
date: 2022-01-01T00:37:04+08:00
draft: true
summary: "删除链表的倒数第N个节点的思路与代码"
tags: [
    "力扣刷题",
    "每日一题",
    "链表"
] 
categories: [
    "leetcode"
]
featuredImagePreview: https://w.wallhaven.cc/full/e7/wallhaven-e7vwwl.jpg
---
![General 3840x2103 Abrar Khan artwork ArtStation fantasy art digital art landscape mountains monks sky](https://w.wallhaven.cc/full/e7/wallhaven-e7vwwl.jpg)

## 删除链表的倒数第N个节点

**问题**

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。



**思路**

1. 找到倒数第N+1个节点（利用寻找单链表的倒数第k个节点的方法即可）
2. 删除第N个节点
3. 返回头节点



**代码实现**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // 虚拟头结点
        ListNode dummy = new ListNode(-1);
        dummy.next = head;

        // 找到倒数第N+1个节点
        ListNode p = findFromEnd(dummy, n+1);
        // 删除倒数第N个节点
        p.next = p.next.next;
        // 返回头结点
        return dummy.next;
    }


    ListNode findFromEnd(ListNode head, int k){
        ListNode p1 = head;
        // p1先走k步
        for (int i = 0; i<k; i++){
            p1 = p1.next;
        }
        ListNode p2 = head;
        while (p1 != null){
            p1 = p1.next;
            p2 = p2.next;
        }  
        return p2;
    }

}
```



> 注意： 我们又使用了虚拟头结点的技巧，也是为了防止出现空指针的情况，比如说链表总共有 5 个节点，题目就让你删除倒数第 5 个节点，也就是第一个节点，那按照算法逻辑，应该首先找到倒数第 6 个节点。但第一个节点前面已经没有节点了，这就会出错。
>
> 但有了我们虚拟节点 `dummy` 的存在，就避免了这个问题，能够对这种情况进行正确的删除。