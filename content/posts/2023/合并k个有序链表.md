---
title: "合并k个有序链表"
date: 2021-12-31T18:45:03+08:00
draft: true
summary: "合并k个有序链表的思路与代码（包括二叉堆与优先级队列）"
tags: [
    "力扣刷题",
    "每日一题",
    "链表"
] 
categories: [
    "leetcode"
]
featuredImagePreview: https://pic2.zhimg.com/v2-db0f7831a7eda4eb07f951657fe5a8e0_r.jpg?source=1940ef5c
---
![](https://pic2.zhimg.com/v2-db0f7831a7eda4eb07f951657fe5a8e0_r.jpg?source=1940ef5c)

### 合并k个有序链表

#### **问题**

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。



#### **思路**

合并 `k` 个有序链表的逻辑类似合并两个有序链表，难点在于，<u>如何快速得到 `k` 个节点中的最小节点</u>，接到结果链表上？

用到 **优先级队列（二叉堆）**这种数据结构，把链表节点放入一个最小堆，就可以每次获得 `k` 个节点中的最小节点

1. **初始化**：初始化虚拟头结点，初始化p指向虚拟头结点
2. **构造优先级队列**：构造最小堆（Comparator如何设置）
3. **结点加入最小堆**：将k个链表的头结点加入最小堆（使用add方法）
4. **获取最小节点并加入新链表，p指针和最小节点处指针后移动**：如果优先级队列不为空，则获取最小结点，接到结果链表中（使用poll方法获取结点，接到后p指针后移），获取完后将节点后移，并加入到最小堆中。
5. **结束**：返回`dummy.next`



#### **代码**

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
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) return null;
        // 虚拟头结点
        ListNode dummy = new ListNode(-1);
        ListNode p = dummy;
        // 构建优先级队列，最小堆
        PriorityQueue<ListNode> pq = new PriorityQueue<>(lists.length, (a, b) -> (a.val - b.val));
        // 将k个链表的头结点加入最小堆
        for(ListNode head : lists){
            if (head != null){
                pq.add(head);
            }
        }

        while(!pq.isEmpty()){
            // 获取最小结点，接到结果链表中
            ListNode node = pq.poll();
            p.next = node;
            // p后移
            p = p.next;

            // 获取下一个结点并加入优先级队列中
            if (node.next!=null){
                pq.add(node.next);
            }
        }

        return dummy.next;

    }
}
```





#### **拓展**

##### 二叉堆与优先级队列概念

Q1：二叉堆是什么，二叉堆和二叉树是什么关系？

二叉堆在逻辑上其实是一种特殊的二叉树（完全二叉树），只不过**存储在数组里**。一般的链表二叉树，我们有操作节点的指针，而在数组里，我们把数组索引作为指针。

```java
int parent(int root){
    return root / 2;
}
int left(int root){
    return root * 2;
}
int right(int root){
    return root * 2 + 1;
}
```



画个图你立即就能理解了，比如 `arr` 是一个字符数组，注意数组的第一个索引 0 空着不用：

![img](https://gitee.com/li_yue1999/picture-of-note/raw/master/img/1.png)

你看到了，因为这棵二叉树是「完全二叉树」，所以把 `arr[1]` 作为整棵树的根的话，每个节点的父节点和左右孩子的索引都可以通过简单的运算得到，这就是二叉堆设计的一个巧妙之处。

二叉堆还分为最大堆和最小堆。**最大堆的性质是：每个节点都大于等于它的两个子节点**。类似的，最小堆的性质是：每个节点都小于等于它的子节点。

对于一个最大堆，根据其性质，显然堆顶，也就是 `arr[1]` 一定是所有元素中最大的元素。



Q2：二叉堆操作

`sink`（下沉）和`swim`（上浮）



Q3：二叉堆的应用

1. 堆排序
2. 优先级队列



Q4：什么是优先级队列

优先级队列这种数据结构有一个很有用的功能，你插入或者删除元素的时候，元素会自动排序，这底层的原理就是二叉堆的操作。

数据结构的功能无非增删查该，优先级队列有两个主要 API，分别是 `insert` 插入一个元素和 `delMax` 删除最大元素（如果底层用最小堆，那么就是 `delMin`）。



##### 优先级队列实现

下面我们实现一个简化的优先级队列，先看下代码框架：

> PS：这里用到 Java 的泛型，`Key` 可以是任何一种可比较大小的数据类型，比如 Integer 等类型。

```java
public class MaxPQ
    <Key extends Comparable<Key>> {
    // 存储元素的数组
    private Key[] pq;
    // 当前 Priority Queue 中的元素个数
    private int N = 0;

    public MaxPQ(int cap) {
        // 索引 0 不用，所以多分配一个空间
        pq = (Key[]) new Comparable[cap + 1];
    }

    /* 返回当前队列中最大元素 */
    public Key max() {
        return pq[1];
    }

    /* 插入元素 e */
    public void insert(Key e) {...}

    /* 删除并返回当前队列中最大元素 */
    public Key delMax() {...}

    /* 上浮第 k 个元素，以维护最大堆性质 */
    private void swim(int k) {...}

    /* 下沉第 k 个元素，以维护最大堆性质 */
    private void sink(int k) {...}

    /* 交换数组的两个元素 */
    private void exch(int i, int j) {
        Key temp = pq[i];
        pq[i] = pq[j];
        pq[j] = temp;
    }

    /* pq[i] 是否比 pq[j] 小？ */
    private boolean less(int i, int j) {
        return pq[i].compareTo(pq[j]) < 0;
    }

    /* 还有 left, right, parent 三个方法（看上文）*/
}

```



空出来的四个方法（`insert`,`delMax`,`sink`,`swim`）是二叉堆和优先级队列的奥妙所在，下面用图文来逐个理解。



###### **实现 swim 和 sink**

为什么要有上浮 `swim` 和下沉 `sink` 的操作呢？为了维护堆结构。

我们要讲的是最大堆，每个节点都比它的两个子节点大，但是在插入元素和删除元素时，难免破坏堆的性质，这就需要通过这两个操作来恢复堆的性质了。

对于最大堆，会破坏堆性质的有有两种情况：

> 1. 如果某个节点 A 比它的子节点（中的一个）小，那么 A 就不配做父节点，应该下去，下面那个更大的节点上来做父节点，这就是对 A 进行**下沉**。
> 2. 如果某个节点 A 比它的父节点大，那么 A 不应该做子节点，应该把父节点换下来，自己去做父节点，这就是对 A 的**上浮**。

当然，错位的节点 A 可能要上浮（或下沉）很多次，才能到达正确的位置，恢复堆的性质。所以代码中肯定有一个 `while` 循环。

那么，这两个操作不是互逆吗，所以上浮的操作一定能用下沉来完成，为什么我还要费劲写两个方法？

是的，操作是互逆等价的，但是最终我们的操作只会在堆底和堆顶进行（等会讲原因），显然堆底的「错位」元素需要上浮，堆顶的「错位」元素需要下沉。



**上浮的代码实现：**

```java
private void swim(int k) {
    // 如果浮到堆顶，就不能再上浮了
    while (k > 1 && less(parent(k), k)) {
        // 如果第 k 个元素比上层大
        // 将 k 换上去
        exch(parent(k), k);
        k = parent(k);
    }
}
```



**下沉的代码实现：**

下沉比上浮略微复杂一点，因为<u>上浮某个节点 A，只需要 A 和其父节点比较大小即可</u>；但是<u>下沉某个节点 A，需要 A 和其**两个子节点**比较大小</u>，如果 A 不是最大的就需要调整位置，要把较大的那个子节点和 A 交换。

```java
private void sink(int k) {
    // 如果沉到堆底，就沉不下去了
    while (left(k) <= N) {
        // 先假设左边节点较大
        int older = left(k);
        // 如果右边节点存在，比一下大小确定older
        if (right(k) <= N && less(older, right(k)))
            older = right(k);
        // 结点 k 比俩孩子都大，就不必下沉了
        if (less(older, k)) break;
        // 否则，不符合最大堆的结构，下沉 k 结点
        exch(k, older);
        k = older;
    }
}

```



至此，二叉堆的主要操作就讲完了，一点都不难吧，代码加起来也就十行。明白了 `sink` 和 `swim` 的行为，下面就可以实现优先级队列了。



###### **实现 delMax 和 insert**

这两个方法就是建立在 `swim` 和 `sink` 上的。

**`insert` 方法先把要插入的元素添加到堆底的最后，然后让其上浮到正确位置**。

```java
public void insert(Key e) {
    N++;
    // 先把新元素加到最后
    pq[N] = e;
    // 然后让它上浮到正确的位置
    swim(N);
}

```



**`delMax` 方法先把堆顶元素 `A` 和堆底最后的元素 `B` 对调，然后删除 `A`，最后让 `B` 下沉到正确位置**。

```java
public Key delMax() {
    // 最大堆的堆顶就是最大元素
    Key max = pq[1];
    // 把这个最大元素换到最后，删除之
    exch(1, N);
    pq[N] = null;
    N--;
    // 让 pq[1] 下沉到正确位置
    sink(1);
    return max;
}

```



至此，一个优先级队列就实现了，插入和删除元素的时间复杂度为 `O(logK)`，`K` 为当前二叉堆（优先级队列）中的元素总数。因为我们时间复杂度主要花费在 `sink` 或者 `swim` 上，而不管上浮还是下沉，最多也就树（堆）的高度，也就是 log 级别。



##### java中使用优先级队列（PriorityQueue）

自 JDK 1.5 起，Java 容器类库中提供了优先级队列的工具 — `PriorityQueue`。

> An unbounded priority {@linkplain Queue queue} based on a priority heap. The elements of the priority queue are ordered according to their {@linkplain Comparable natural ordering}, or by a {@link Comparator} provided at queue construction time, depending on which constructor is used.  A priority queue does not permit {@code null} elements. A priority queue relying on natural ordering also does not permit insertion of non-comparable objects (doing so may result in {@code ClassCastException}).

一个基于优先级堆的无界优先级队列。优先级队列的元素，根据他们的 [natural ordering](../../java/lang/Comparable.html)，或通过 [`Comparator`](../../java/util/Comparator.html)方法进行排序。

一个优先队列不允许  `null`元素。依靠自然排序的优先级队列也不允许插入不可比较的对象（这样做可能导致  `ClassCastException`）。 

这个类和它的迭代器实现所有的可选方法的[`Collection`](../../java/util/Collection.html)和[`Iterator`](../../java/util/Iterator.html)接口。方法[`iterator()`](../../java/util/PriorityQueue.html#iterator--)提供的迭代器不能保证遍历优先级队列的元素在任何特定的顺序。如果你需要有序遍历，考虑使用`Arrays.sort(pq.toArray())`。

> 注：``PriorityQueue` 是非线程安全的，如果想要在多线程的环境下使用，可以使用 `PriorityBlockingQueue`。

（更多参考jdk文档）



**构造方法**（Constructor）

| Constructor and Description                                  |
| ------------------------------------------------------------ |
| `PriorityQueue()`  创建一个默认的初始容量  `PriorityQueue`（11），命令其元素按其 [natural ordering](../../java/lang/Comparable.html)。 |
| `PriorityQueue(Collection<? extends E> c)`  创建一个 `PriorityQueue`包含在指定集合的元素。 |
| `PriorityQueue(Comparator<? super E> comparator)`  创建一个默认的初始容量和它的元素是按指定的比较器  `PriorityQueue`。 |
| `PriorityQueue(int initialCapacity)`  创建一个具有指定的初始容量，命令其元素按其 [natural  ordering](../../java/lang/Comparable.html) `PriorityQueue`。 |
| `PriorityQueue(int initialCapacity,  Comparator<? super E> comparator)`  创建一个具有指定的初始容量，命令其元素根据指定的比较器  `PriorityQueue`。 |
| `PriorityQueue(PriorityQueue<? extends E> c)`  创建一个 `PriorityQueue`包含指定的优先级队列中的元素。 |
| `PriorityQueue(SortedSet<? extends E> c)`  创建一个 `PriorityQueue`含有指定排序的集合中的元素。 |



**方法**(Method)

| Modifier and Type       | Method and Description                                       |
| ----------------------- | ------------------------------------------------------------ |
| `boolean`               | `add(E e)`  将指定的元素插入到该优先级队列中。               |
| `void`                  | `clear()`  从这个优先级队列中移除所有的元素。                |
| `Comparator<? super E>` | `comparator()`  返回用于为该队列中的元素的比较，或 `null`如果这个队列是根据其元素的  [natural  ordering](../../java/lang/Comparable.html)排序。 |
| `boolean`               | `contains(Object o)`  返回 `true`如果此队列包含指定的元素。  |
| `Iterator<E>`           | `iterator()`  返回此队列中元素的迭代器。                     |
| `boolean`               | `offer(E e)`  将指定的元素插入到该优先级队列中。             |
| `E`                     | `peek()`  检索，但不删除，这个队列头，或返回 `null`如果队列为空。 |
| `E`                     | `poll()`  检索并移除此队列的头，或返回 `null`如果队列为空。  |
| `boolean`               | `remove(Object o)`  从该队列中移除指定元素的一个实例，如果它是存在的。 |
| `int`                   | `size()`  返回此集合中的元素的数目。                         |
| `Spliterator<E>`        | `spliterator()`  创建一个后期绑定和快速失败 [`Spliterator`](../../java/util/Spliterator.html)在队列中的元素。 |
| `Object[]`              | `toArray()`  返回一个包含此队列中所有元素的数组。            |
| `<T> T[]`               | `toArray(T[] a)`  返回包含此队列中的所有元素的数组；返回数组的运行时类型是指定的数组的运行时类型。 |



`add(E e)`和`offer(E e)`的语义相同，都是向优先队列中插入元素，只是`Queue`接口规定二者对插入失败时的处理不同，前者在插入失败时抛出异常，后则则会返回`false`。对于*PriorityQueue*这两个方法其实没什么差别。

`element()`和`peek()`的语义完全相同，都是获取但不删除队首元素，也就是队列中权值最小的那个元素，二者唯一的区别是当方法失败时前者抛出异常，后者返回null。根据小顶堆的性质，堆顶那个元素就是全局最小的那个；由于堆用数组表示，根据下标关系，0下标处的那个元素既是堆顶元素。所以直接返回数组0下标处的那个元素即可。

`remove()`和`poll()`方法的语义也完全相同，都是获取并删除队首元素，区别是当方法失败时前者抛出异常，后者返回`null`。由于删除操作会改变队列的结构，为维护小顶堆的性质，需要进行必要的调整。



**java优先级队列源码分析**

继承图

![PriorityQueue继承图](https://gitee.com/li_yue1999/picture-of-note/raw/master/img/image-20211231132528856.png)



可见，`PriorityQueue`属于集合框架下的类。它具有集合的特点，所以集合中的方法它能够使用。``PriorityQueue`继承自`AbstractQueue`，不允许插入`null`元素。

`PriorityQueue`继承自`Queue`, 所以在使用上也继承了``Queue`的 `offer()`与`poll()`。鉴于`PriorityQueue`的有序特点，它重写了该方法。



**优先队列的插入**
为了保持PriorityQueue元素的有序性，在进行插入时，我们总是对插入后的数组做调整，这种调整的方式如下：

- 首先将新增元素放到队尾
- 将插入元素与它的父节点比较，如果父节点比元素大，将元素与父节点元素调换位置
- 继续将该元素与父节点比较，直到该元素为根节点或比父节点的元素大为止

图示

![二叉堆的插入](https://gitee.com/li_yue1999/picture-of-note/raw/master/img/20201230170223733.png)



这个时候得到的队列就是有序队列了。



**源码**

以下是PriorityQueue的offer有关源码（jdk1.8）：

```java
/**
 * Inserts the specified element into this priority queue.
 *
 * @return {@code true} (as specified by {@link Queue#offer})
 * @throws ClassCastException if the specified element cannot be
 *         compared with elements currently in this priority queue
 *         according to the priority queue's ordering
 * @throws NullPointerException if the specified element is null
 */
public boolean offer(E e) {
    if (e == null)
        throw new NullPointerException();
    modCount++;
    int i = size;
    if (i >= queue.length)
        grow(i + 1);
    siftUp(i, e);
    size = i + 1;
    return true;
}
```



```java
    /**
     * Increases the capacity of the array.
     *
     * @param minCapacity the desired minimum capacity
     */
    private void grow(int minCapacity) {
        int oldCapacity = queue.length;
        // Double size if small; else grow by 50%
        int newCapacity = ArraysSupport.newLength(oldCapacity,
                minCapacity - oldCapacity, /* minimum growth */
                oldCapacity < 64 ? oldCapacity + 2 : oldCapacity >> 1
                                           /* preferred growth */);
        queue = Arrays.copyOf(queue, newCapacity);
    }
```



```java
    /**
     * Inserts item x at position k, maintaining heap invariant by
     * promoting x up the tree until it is greater than or equal to
     * its parent, or is the root.
     *
     * To simplify and speed up coercions and comparisons, the
     * Comparable and Comparator versions are separated into different
     * methods that are otherwise identical. (Similarly for siftDown.)
     *
     * @param k the position to fill
     * @param x the item to insert
     */
    private void siftUp(int k, E x) {
        if (comparator != null)
            siftUpUsingComparator(k, x, queue, comparator);
        else
            siftUpComparable(k, x, queue);
    }

    private static <T> void siftUpComparable(int k, T x, Object[] es) {
        Comparable<? super T> key = (Comparable<? super T>) x;
        while (k > 0) {
            int parent = (k - 1) >>> 1;
            Object e = es[parent];
            if (key.compareTo((T) e) >= 0)
                break;
            es[k] = e;
            k = parent;
        }
        es[k] = key;
    }

    private static <T> void siftUpUsingComparator(
        int k, T x, Object[] es, Comparator<? super T> cmp) {
        while (k > 0) {
            int parent = (k - 1) >>> 1;
            Object e = es[parent];
            if (cmp.compare(x, (T) e) >= 0)
                break;
            es[k] = e;
            k = parent;
        }
        es[k] = x;
    }

```



#### **原文**

[一文搞懂单链表的六大解题套路](https://labuladong.gitee.io/algo/1/6/)

[二叉堆详解实现优先级队列](https://labuladong.gitee.io/algo/2/20/50/)

[java优先级队列](https://blog.csdn.net/weixin_43320847/article/details/83043484)

[PriorityQueue的用法和底层实现原理](https://blog.csdn.net/u010623927/article/details/87179364)