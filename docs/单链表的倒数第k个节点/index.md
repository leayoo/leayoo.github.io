# 单链表的倒数第k个节点


![General 1920x1080 artwork digital art space spaceship science fiction Simon Heath Cryo Chamber](https://gitee.com/li_yue1999/picture-of-note/raw/master/img/wallhaven-6oe7ww.jpg)

### 单链表的倒数第k个节点

**问题**

寻找单链表的倒数第k个节点，要求只遍历一遍链表。



**思路**

假设 `k = 2`，思路如下：

首先，我们先让一个指针 `p1` 指向链表的头节点 `head`，然后走 `k` 步

![img](https://labuladong.gitee.io/algo/images/链表技巧/1.jpeg)

现在的 `p1`，只要再走 `n - k` 步，就能走到链表末尾的空指针了对吧？

趁这个时候，再用一个指针 `p2` 指向链表头节点 `head`：

![img](https://labuladong.gitee.io/algo/images/链表技巧/2.jpeg)

接下来就很显然了，让 `p1` 和 `p2` 同时向前走，`p1` 走到链表末尾的空指针时走了 `n - k` 步，`p2` 也走了 `n - k` 步，也就是链表的倒数第 `k` 个节点：

![img](https://labuladong.gitee.io/algo/images/链表技巧/3.jpeg)

这样，只遍历了一次链表，就获得了倒数第 `k` 个节点 `p2`。



1. 初始化指针p1，指向头结点
2. p1先走k步
3. 初始化指针p2，指向头结点
4. p1和p2一起走n-k步
5. 返回p2



**代码**

```java
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
```




