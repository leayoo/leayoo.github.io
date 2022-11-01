---
title: "Comparator与lambda表达式"
date: 2021-12-31T18:45:28+08:00
draft: false
summary: "Comparator与lambda表达式 从概念到示例"
tags: [
    "Java",
    "Comparator",
    "lambda",
] 
categories: [
    "Java 8"
]
featuredImagePreview: https://w.wallhaven.cc/full/l3/wallhaven-l3xk6q.jpg
---
![](https://w.wallhaven.cc/full/l3/wallhaven-l3xk6q.jpg)



## Comparator与lambda表达式

### 前言

1. 本文有些地方懒得做翻译(不过不难)

2. 看不懂的地方别纠结，一直往下看，也许会明白（因为有些部分重复了）。



### 概述

**参数类型** 

`T` -可以通过该比较器比较对象的类型 

**Functional Interface:** 

这是一个功能接口，因此可以作为赋值的目标一个<u>lambda表达式</u>或方法参考。 

（因为是一个功能性接口，所以也可以通过lambda表达式实现。）



> A comparison function, which imposes a total ordering on some collection of objects. Comparators can be passed to a sort method (such as Collections.sort or Arrays.sort) to allow precise control over the sort order. Comparators can also be used to control the order of certain data structures (such as sorted sets or sorted maps), or to provide an ordering for collections of objects that don't have a natural ordering.

一个比较函数，比较器可以通过某种方法（如 [`Collections.sort`](../../java/util/Collections.html#sort-java.util.List-java.util.Comparator-)或  [`Arrays.sort`](../../java/util/Arrays.html#sort-T:A-java.util.Comparator-)）允许精确控制排序顺序。

比较器也可以用来控制特定的数据结构顺序（如 [`sorted sets`](../../java/util/SortedSet.html)或 [`sorted maps`](../../java/util/SortedMap.html)），或提供的对象不集合排序有  [`natural ordering`](../../java/lang/Comparable.html)。 



所以，Comparator 是比较器接口。

我们若需要控制某个类的次序，而该类本身不支持排序(即没有实现Comparable接口)；那么，我们可以建立一个“该类的比较器”来进行排序。这个“比较器”只需要实现Comparator接口即可。

也就是说，我们可以通过“**实现Comparator类来新建一个比较器**”，然后通过该比较器对类进行排序。



**Comparator 定义**

Comparator 接口仅仅只包括两个个函数，它的定义如下：

```java
package java.util;
public interface Comparator<T> {
    int compare(T o1, T o2);
    boolean equals(Object obj);
}
```

说明：

1. 若一个类要实现Comparator接口：它一定要实现compareTo(T o1, T o2) 函数，但可以不实现 equals(Object obj) 函数。

为什么可以不实现 equals(Object obj) 函数呢？ 因为任何类，默认都是已经实现了equals(Object obj)的。 Java中的一切类都是继承于java.lang.Object，在Object.java中实现了equals(Object obj)函数；所以，其它所有的类也相当于都实现了该函数。

2. `int compare(T o1, T o2)` 是<u>“比较o1和o2的大小”。返回“负数”，意味着“o1比o2小”；返回“零”，意味着“o1等于o2”；返回“正数”，意味着“o1大于o2”</u>。



### Comparator使用示例

首先，我们先创建一个Player类

```java
public class Player {
    private int ranking;
    private String name;
    private int age;
    
    // constructor, getters, setters  
}
```

然后我们尝试对其进行初始化及分类

```java
public static void main(String[] args) {
    List<Player> footballTeam = new ArrayList<>();
    Player player1 = new Player(59, "John", 20);
    Player player2 = new Player(67, "Roger", 22);
    Player player3 = new Player(45, "Steven", 24);
    footballTeam.add(player1);
    footballTeam.add(player2);
    footballTeam.add(player3);

    System.out.println("Before Sorting : " + footballTeam);
    Collections.sort(footballTeam);
    System.out.println("After Sorting : " + footballTeam);
}
```

会出现编译错误

```
The method sort(List<T>) in the type Collections 
  is not applicable for the arguments (ArrayList<Player>)
```



使用Comparator对其进行改造

> **The `Comparator` interface defines a `compare(arg1, arg2)` method** with two arguments that represent compared objects, and works similarly to the *Comparable.compareTo()* method.



**Creating Comparators**

To create a *Comparator,* we have to implement the *Comparator* interface.

For our first example, we'll create a *Comparator* to use the *ranking* attribute of *Player* to sort the players:

```java
public class PlayerRankingComparator implements Comparator<Player> {

    @Override
    public int compare(Player firstPlayer, Player secondPlayer) {
       return Integer.compare(firstPlayer.getRanking(), secondPlayer.getRanking());
    }

}
```



Similarly, we can create a *Comparator* to use the *age* attribute of *Player* to sort the players:

```java
public class PlayerAgeComparator implements Comparator<Player> {

    @Override
    public int compare(Player firstPlayer, Player secondPlayer) {
       return Integer.compare(firstPlayer.getAge(), secondPlayer.getAge());
    }

}
```



**Comparators in Action**

To demonstrate the concept, let's modify our *PlayerSorter* by introducing a second argument to the *Collections.sort* method*,* which is actually the instance of *Comparator* we want to use.

**Using this approach, we can override the natural ordering**:

```java
PlayerRankingComparator playerComparator = new PlayerRankingComparator();
Collections.sort(footballTeam, playerComparator);
```

Now let's run our *PlayerRankingSorter to* see the result:

```java
Before Sorting : [John, Roger, Steven]
After Sorting by ranking : [Steven, John, Roger]
```

If we want a different sorting order, we only need to change the *Comparator* we're using:

```java
PlayerAgeComparator playerComparator = new PlayerAgeComparator();
Collections.sort(footballTeam, playerComparator);
```

Now when we run our *PlayerAgeSorter*, we can see a different sort order by *age:*

```java
Before Sorting : [John, Roger, Steven]
After Sorting by age : [Roger, John, Steven]
```



**Java 8 Comparators**

<u>Java 8 provides new ways of defining *Comparators* by using lambda expressions, and the *comparing()* static factory method.</u>

Let's see a quick example of how to use a lambda expression to create a *Comparator*:

```java
Comparator byRanking = 
  (Player player1, Player player2) -> Integer.compare(player1.getRanking(), player2.getRanking());
```

The *Comparator.comparing* method takes a method calculating the property that will be used for comparing items, and returns a matching *Comparator* instance:

```java
Comparator<Player> byRanking = Comparator
  .comparing(Player::getRanking);
Comparator<Player> byAge = Comparator
  .comparing(Player::getAge);
```

To explore the Java 8 functionality in-depth, check out our [Java 8 Comparator.comparing](https://www.baeldung.com/java-8-comparator-comparing) guide.



**Avoiding the Subtraction Trick**

Over the course of this tutorial, we've used the *Integer.compare()* method to compare two integers. However, one might argue that we should use this clever one-liner instead:

```java
Comparator<Player> comparator = (p1, p2) -> p1.getRanking() - p2.getRanking();
```

**Although it's much more concise than other solutions, it can be a victim of integer overflows in Java**:

```java
Player player1 = new Player(59, "John", Integer.MAX_VALUE);
Player player2 = new Player(67, "Roger", -1);

List<Player> players = Arrays.asList(player1, player2);
players.sort(comparator);
```

Since -1 is much less than the *Integer.MAX_VALUE*, “Roger” should come before “John” in the sorted collection. **However, due to integer overflow, the `“Integer.MAX_VALUE – (-1)”` will be less than zero**. So based on the *Comparator/Comparable* contract, the *Integer.MAX_VALUE* is less than -1, which is obviously incorrect.

Therefore, despite what we expected, “John” comes before “Roger” in the sorted collection:

```java
assertEquals("John", players.get(0).getName());
assertEquals("Roger", players.get(1).getName());
```



### 使用Comparator进行排序

Comparator接口的身影在JDK库中随处可见，从查找到排序，再到反转操作，等等。Java 8里它变成了一个函数式接口，这样的好处就是我们可以使用流式语法来实现比较器了。

我们用几种不同的方式来实现一下Comparator，看看新式语法的价值所在。你的手指头会感谢你的，不用实现匿名内部类少敲了多少键盘啊。



（动手实践一下，能帮助理解）

下面这个例子将使用不同的比较方法，来将一组人进行排序。我们先来创建一个Person的JavaBean。

```java
public class Person {

    private final String name;

    private final int age;

    public Person(final String theName, final int theAge) {
        name = theName;
        age = theAge;

    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public int ageDifference(final Person other) {
        return age - other.age;

    }

    public String toString() {
        return String.format("%s - %d", name, age);
    }
}


```



我们可以通过Person类来实现Comparator接口，不过这样我们只能使用一种比较方式。我们希望能比较不同的属性——比如名字，年龄，或者这些的组合。为了可以灵活的进行比较，我们可以使用Comparator，当我们需要进行比较的时候，再去生成相关的代码。



我们先来创建一个Person的列表，每个人都有不同的名字和年龄。

```java
        final List<Person> people = Arrays.asList(
                new Person("John", 20),
                new Person("Sara", 21),
                new Person("Jane", 21),
                new Person("Greg", 35));
```

我们可以通过人的名字或者年龄来对他们进行升序或者降序的排序。

一般的方法就是使用<u>匿名内部类</u>来实现Comparator接口。这样写的话只有比较相关的代码是有意义的，其它的都只是走走形式而已。而使用lambda表达式则可以聚焦到比较的本质上来。



**按照年龄排序**

我们先按<u>年龄从小到大</u>对他们进行排序。

既然我们已经有了一个List对象，我们可以用它的sort()方法来进行排序。不过这个方法也有它的问题。<u>这是一个void方法，也就是说当我们调用这个方法的时候，这个列表会发生改动。要保留原始列表的话，我们得先拷贝出一份来，然后再调用sort()方法。</u>这简直太费劲了。这个时候我们得求助下Stream类了。

我们可以从List那获取一个Stream对象，然后调用它的sorted()方法。它会返回一个排好序的集合，而不是在原来的集合上做修改。使用这个方法的话可以很方便的配置Comparator的参数。

```java
List<Person> ascendingAge =
                people.stream()
                        .sorted((person1, person2) -> person1.ageDifference(person2))
                        .collect(toList());
        printPeople("Sorted in ascending order by age: ", ascendingAge);
```



1. 我们先通过stream()方法将列表转化成一个Stream对象。

2. 然后调用它的sorted()方法。这个方法接受一个Comparator参数。由于Comparator是一个函数式接口，我们可以传入一个lambda表达式。

3. 最后我们调用collect方法，让它把结果存储到一个列表里。collect方法是一个归约器，它能把迭代过程中的对象输出成某种特定的格式或者类型。toList()方法是Collectors类的一个静态方法。



Comparator的抽象方法compareTo()接收两个参数，也就是要比较的对象，并返回一个int类型的结果。为了兼容这个，我们的lambda表达式也接收两个参数，两个Person对象，它们的类型是由编译器自动推导的。我们返回一个int类型，表明比较的对象是否相等。

因为要按年龄进行排序，所以我们会比较两个对象的年龄，然后返回比较的结果。<u>如果他们一样大，则返回0。否则如果第一个人更年轻的话就返回一个负数，更年长的话就返回正数。</u>（相当于第一个减去第二个）



sorted()方法会遍历目标集合的每个元素并调用指定的Comparator，来确定出元素的排序顺序。sorted()方法的执行方式有点类似前面说到的reduce()方法。reduce()方法把列表逐步归约出一个结果。而sorted()方法则通过比较的结果来进行排序。

一旦我们排好序后我们想要把结果打印出来，因此我们调用了一个`printPeople()`方法;下面来实现下这个方法。

```java
public static void printPeople(
final String message, final List<Person> people) {
    System.out.println(message);
    people.forEach(System.out::println);
}
```

这个方法中，我们先打印了一个消息，然后遍历列表，打印出里面的每个元素。

我们来调用下sorted()方法看看，它会将列表中的人按年龄从小到大进行排列。

```
Sorted in ascending order by age:
John - 20
Sara - 21
Jane - 21
Greg - 35
```



我们再看一下sorted()方法，来做一个改进。

```java
.sorted((person1, person2) -> person1.ageDifference(person2))
```

在传入的这个lambda表达式里，我们只是简单的路由了下这两个参数——<u>第一个参数作为ageDifference()方法的调用目标，而第二个作为它的参数</u>。但是我们可以不这么写，而是用一个office-space模式——也就是使用方法引用，让Java编译器去做路由。

这里用到的参数路由和前面看到的有点不同。我们之前看到的，要么参数是作为调用目标，要么是作为调用参数。而现在，我们有两个参数，我们希望能分成两个部分，<u>一个是作为方法调用的目标，第二个则作为参数</u>。别担心，Java编译器会告诉你，“这个我来搞定”。

我们可以把前面的sorted()方法里面的lambda表达式替换成一个短小精悍的ageDifference方法。

```java
people.stream().sorted(Person::ageDifference)
```

这段代码非常简洁，这多亏了Java编译器提供的方法引用。编译器接收到两个person实例的参数，<u>把第一个用作ageDifference()方法的调用目标，而第二个作为方法参数</u>。<u>我们让编译器去做这个工作，而不是自己直接去写代码。当使用这种方式的时候，我们必须确定第一个参数就是引用的方法的调用目标，而剩下那个就是方法的入参。</u>



### 重用Comparator

我们很容易就将列表中的人按年龄从小到大排好序了，当然从大到小进行排序也很容易。我们来试一下。

```java
printPeople("Sorted in descending order by age: ", people.stream().sorted((person1, person2) -> person2.ageDifference(person1)).collect(toList()));
```

我们调用了sorted()方法并<u>传入一个lambda表达式，它正好能适配成Comparator接口</u>，就像前面的例子那样。唯一不同的就是这个lambda表达式的实现——我们把要比较的人调了下顺序。结果应该是按他们的年龄由从大到小排列的。我们来看一下。

```
Sorted in descending order by age:
Greg - 35
Sara - 21
Jane - 21
John - 20
```



只是改一下比较的逻辑费不了太多劲。但我们没法把这个版本重构成方法引用的，因为参数的顺序不符合方法引用的参数路由的规则；第一个参数并不是用作方法的调用目标，而是作为方法参数。有一个方法能解决这个问题，同时它还能减少重复的工作。我们来看下如何实现。



前面我们已经创建了两个lambda表达式：一个是按年龄从小到大排序，一个是从大到小排序。这么做的话，会出现代码冗余和重复，并破坏了DRY原则。如果我们只是想要调整下排序顺序的话，JDK提供了一个reverse方法，它有一个特殊的方法修饰符，default。我们会在77页中的default方法来讨论它，这里我们先用下这个reversed()方法来去除冗余性。

```java
Comparator<Person> compareAscending =
(person1, person2) -> person1.ageDifference(person2);
Comparator<Person> compareDescending = compareAscending.reversed();
```

我们先创建了一个Comparator，compareAscending，来将人按年龄从小到大进行排序。为了反转比较顺序，而不是再写一次这个代码，我们只需要调用一下这个第一个Comparator的reversed()方法就可以获取第二个Comparator对象。在reversed()方法底层，它创建了一个比较器，来交换了比较的参数的顺序。这说明reversed也是一个高阶方法——它创建并返回了一个无副作用的函数。我们把这个两个比较器用到代码里。

```java
printPeople("Sorted in ascending order by age: ",
      people.stream().sorted(compareAscending)
			.collect(toList()));

printPeople("Sorted in descending order by age: ",
	  people.stream().sorted(compareDescending)
            .collect(toList()));
```



从代码中明显可以看到，Java8的这些新特性极大的减少了代码的冗余及复杂度，不过好处远不止这些，JDK里还有无限可能等着你去探索。



**按照名字排序**

我们已经可以按年龄进行排序了，想按名字来排序的话也很简单。我们来按名字进行字典序排列，同样的，只需要改下lambda表达式里的逻辑就好了。

```java
printPeople("Sorted in ascending order by name: ",
people.stream().sorted((person1, person2) ->
person1.getName().compareTo(person2.getName()))
.collect(toList()));
```

输出的结果里会按名字的字典序进行排列。

```java
Sorted in ascending order by name:
Greg - 35
Jane - 21
John - 20
Sara - 21
```

现在为止，我们要么就按年龄排序，要么就按名字排序。我们可以让lambda表达式的逻辑更智能一些。

比如我们可以**同时按年龄和名字排序**。

我们来选出列表中**最年轻的**人来。我们可以先按年龄从小到大排序然后选中结果中的第一个。不过其实用不着那样，Stream有一个min()方法可以实现这个。这个方法同样也接受一个Comparator，不过返回的是集合中最小的对象。我们来用下它。

```java
people.stream().min(Person::ageDifference)
.ifPresent(youngest -> System.out.println("Youngest: " + youngest));
```

调用min()方法的时候我们用了ageDifference这个方法引用。

<u>min()方法返回的是一个Optinal对象</u>，因为列表可能为空并且里面可能不止一个年纪最小的人。<u>接着我们通过Optinal的ifPrsend()方法获取到年纪最小的那个人，并打印出他的详细信息。</u>来看下输出结果。

```java
Youngest: John - 20
```

输出年纪最大的同样也很简单。只要把这个方法引用传给一个max()方法就好了。

```java
people.stream().max(Person::ageDifference)
.ifPresent(eldest -> System.out.println("Eldest: " + eldest));
```

我们来看下最年长那位的名字和年龄。

```java
Eldest: Greg - 35
```

有了lambda表达式和方法引用之后，比较器的实现变得更简洁也更方便了。JDK也给Compararor类引入了不少便利的方法，使得我们可以更流畅的进行比较，下面我们将会看到。



### 多重比较和流式比较

我们来看下Comparator接口提供了哪些方便的新方法，并用它们来进行多个属性的比较。

我们还是继续使用上节中的那个例子。按名字排序的话，我们上面是这么写的：

```java
people.stream()
.sorted((person1, person2) ->
person1.getName().compareTo(person2.getName()));
```

和上个世纪的内部类的写法比起来，这种写法简直太简洁了。不过如果用了Comparator类里面的一些函数能让它变得更简单，使用这些函数能够让我们更流畅的表述自己的目的。比如说，要按名字排序的话，我们可以这么写：

```java
final Function<Person, String> byName = person -> person.getName();
people.stream().sorted(comparing(byName));
```

这段代码中我们导入了Comparator类的静态方法comparing()。<u>comparing()方法使用传入的lambda表达式来生成一个Comparator对象。也就是说，它也是一个高阶函数，**接受一个函数入参并返回另一个函数**。除了能让语法变得更简洁外，这样的代码读起来也能更好的表述我们想要解决的实际问题。</u>

有了它，进行多重比较的时候也能变得更加流畅。比如，下面这段按名字和年龄比较的代码就能说明一切：

```java
final Function<Person, Integer> byAge = person -> person.getAge();
final Function<Person, String> byTheirName = person -> person.getName();
printPeople("Sorted in ascending order by age and name: ",
people.stream()
.sorted(comparing(byAge).thenComparing(byTheirName))
.collect(toList()));
```

我们先是创建了两个lambda表达式，一个返回指定人的年龄，一个返回的是他的名字。在调用sorted()方法的时候我们把这两个表达式组合 到了一起，这样就能进行多个属性的比较了。comparing()方法创建并返回了一个按年龄比较的Comparator ,我们再调用这个返回的Comparator上面的thenComparing()方法来创建一个组合的比较器，它会对年龄和名字两项进行比较。下面的输出是先按年龄再按名字进行排序后的结果。

```
Sorted in ascending order by age and name:
John - 20
Jane - 21
Sara - 21
Greg - 35
```

可以看到，使用lambda表达式和JDK提供的新的工具类，可以很容易的将Comparator的实现进行组合。



### 本节代码汇总

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.function.Function;

import static java.util.Comparator.comparing;
import static java.util.stream.Collectors.toList;

public class Person {

    private final String name;

    private final int age;

    public Person(final String theName, final int theAge) {
        name = theName;
        age = theAge;

    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public int ageDifference(final Person other) {
        return age - other.age;

    }

    public String toString() {
        return String.format("%s - %d", name, age);
    }

    public static void printPeople(
            final String message, final List<Person> people) {
        System.out.println(message);
        people.forEach(System.out::println);
    }

    public static void main(String[] args) {
        final List<Person> people = Arrays.asList(
                new Person("John", 20),
                new Person("Sara", 21),
                new Person("Jane", 21),
                new Person("Greg", 35));

        // 按照年龄排序，版本1
        List<Person> ascendingAge = people.stream()
                .sorted((person1, person2) -> person2.ageDifference(person1))
                .collect(toList());
//        List<Person> ascendingAge = people.stream()
//                .sorted(Person::ageDifference)
//                .collect(toList());

        printPeople("Sorted in ascending order by age: ", ascendingAge);

        // 按照年龄排序，版本2
        Comparator<Person> compareAscending = (person1, person2) -> person1.ageDifference(person2);
        Comparator<Person> compareDescending = compareAscending.reversed();

        printPeople("Sorted in ascending order by age: ",
                people.stream().sorted(compareAscending).collect(toList()));

        printPeople("Sorted in descending order by age: ",
                people.stream().sorted(compareDescending).collect(toList()));

        // 按照名字排序
        printPeople("Sorted in ascending order by name: ",
                people.stream().sorted((person1, person2) ->
                        person1.getName().compareTo(person2.getName()))
                        .collect(toList()));

        // 最年轻的
        people.stream().min(Person::ageDifference)
                .ifPresent(youngest -> System.out.println("Youngest: " + youngest));

        // 最年长的
        people.stream().max(Person::ageDifference)
                .ifPresent(eldest -> System.out.println("Eldest: " + eldest));

        // 多重比较和流式比较
        // 按照名字排序①
        people.stream()
                .sorted((person1, person2) -> person1.getName().compareTo(person2.getName()));
        // 按照名字排序②
        final Function<Person, String> byName = person -> person.getName();
        people.stream().sorted(comparing(byName));

        // 按名字和年龄比较
        final Function<Person, Integer> byAge = person -> person.getAge();
        final Function<Person, String> byTheirName = person -> person.getName();
        printPeople("Sorted in ascending order by age and name: ",
                people.stream()
                        .sorted(comparing(byAge).thenComparing(byTheirName))
                        .collect(toList()));

    }
}

```



### 比较器Comparator中lambda的使用

```java
import java.math.BigDecimal;

public class Developer {

    String name;
    BigDecimal salary;
    int age;

    public Developer(String name, BigDecimal salary, int age) {
        this.name = name;
        this.salary = salary;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public BigDecimal getSalary() {
        return salary;
    }

    public void setSalary(BigDecimal salary) {
        this.salary = salary;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Developer [" +
                "name='" + name + '\'' +
                ", salary=" + salary +
                ", age=" + age +
                ']';
    }
}
```



- 典型的比较器示例

```java
Comparator<Developer> byName = new Comparator<Developer>() {
    @Override
    public int compare(Developer o1, Developer o2) {
        return o1.getName().compareTo(o2.getName());
    }
};
```

- 等价的Lambda的方式

```java
Comparator<Developer> byName =
    (Developer o1, Developer o2)->o1.getName().compareTo(o2.getName());
```



**不使用Lambda的排序**

假如我们要通过Developer 对象的年龄进行排序，通常情况下我们使用Collections.sort，new个匿名Comparator 类，类似下面这种：

```java
import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class TestSorting {

    public static void main(String[] args) {

        List<Developer> listDevs = getDevelopers();

        System.out.println("Before Sort");
        for (Developer developer : listDevs) {
            System.out.println(developer);
        }

        //sort by age
        Collections.sort(listDevs, new Comparator<Developer>() {
            @Override
            public int compare(Developer o1, Developer o2) {
                return o1.getAge() - o2.getAge();
            }
        });

        System.out.println("After Sort");
        for (Developer developer : listDevs) {
            System.out.println(developer);
        }

    }

    private static List<Developer> getDevelopers() {

        List<Developer> result = new ArrayList<Developer>();

        result.add(new Developer("ricky", new BigDecimal("70000"), 33));
        result.add(new Developer("alvin", new BigDecimal("80000"), 20));
        result.add(new Developer("jason", new BigDecimal("100000"), 10));
        result.add(new Developer("iris", new BigDecimal("170000"), 55));

        return result;

    }

}
```

输出结果：

```java
Before Sort
Developer [name=ricky, salary=70000, age=33]
Developer [name=alvin, salary=80000, age=20]
Developer [name=jason, salary=100000, age=10]
Developer [name=iris, salary=170000, age=55]

After Sort
Developer [name=jason, salary=100000, age=10]
Developer [name=alvin, salary=80000, age=20]
Developer [name=ricky, salary=70000, age=33]
Developer [name=iris, salary=170000, age=55]
```



当比较规则发生变化时，你需要再次new个匿名Comparator 类：

```java
    //sort by age
    Collections.sort(listDevs, new Comparator<Developer>() {
        @Override
        public int compare(Developer o1, Developer o2) {
            return o1.getAge() - o2.getAge();
        }
    });

    //sort by name
    Collections.sort(listDevs, new Comparator<Developer>() {
        @Override
        public int compare(Developer o1, Developer o2) {
            return o1.getName().compareTo(o2.getName());
        }
    });

    //sort by salary
    Collections.sort(listDevs, new Comparator<Developer>() {
        @Override
        public int compare(Developer o1, Developer o2) {
            return o1.getSalary().compareTo(o2.getSalary());
        }
    });
```



这样也可以，不过你会不会觉得这样有点怪，因为其实不同的只有一行代码而已，但是却需要重复写很多代码？



**通过lambda进行排序**

在java8中，List接口直接提供了排序方法， 所以你不需要使用Collections.sort

```java
    //List.sort() since Java 8
    listDevs.sort(new Comparator<Developer>() {
        @Override
        public int compare(Developer o1, Developer o2) {
            return o2.getAge() - o1.getAge();
        }
    });
```



Lambda 示例

```java
import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.List;

public class TestSorting {

    public static void main(String[] args) {

        List<Developer> listDevs = getDevelopers();

        System.out.println("Before Sort");
        for (Developer developer : listDevs) {
            System.out.println(developer);
        }

        System.out.println("After Sort");

        //lambda here!
        listDevs.sort((Developer o1, Developer o2)->o1.getAge()-o2.getAge());

        //java 8 only, lambda also, to print the List
        listDevs.forEach((developer)->System.out.println(developer));
    }

    private static List<Developer> getDevelopers() {

        List<Developer> result = new ArrayList<Developer>();

        result.add(new Developer("ricky", new BigDecimal("70000"), 33));
        result.add(new Developer("alvin", new BigDecimal("80000"), 20));
        result.add(new Developer("jason", new BigDecimal("100000"), 10));
        result.add(new Developer("iris", new BigDecimal("170000"), 55));

        return result;

    }

}
```



输出结果:

```
Before Sort
Developer [name=ricky, salary=70000, age=33]
Developer [name=alvin, salary=80000, age=20]
Developer [name=jason, salary=100000, age=10]
Developer [name=iris, salary=170000, age=55]

After Sort
Developer [name=jason, salary=100000, age=10]
Developer [name=alvin, salary=80000, age=20]
Developer [name=ricky, salary=70000, age=33]
Developer [name=iris, salary=170000, age=55]
```



更多的Lambda 例子

根据年龄

```java
    //sort by age
    Collections.sort(listDevs, new Comparator<Developer>() {
        @Override
        public int compare(Developer o1, Developer o2) {
            return o1.getAge() - o2.getAge();
        }
    });

    //lambda
    listDevs.sort((Developer o1, Developer o2)->o1.getAge()-o2.getAge());

    //lambda, valid, parameter type is optional
    listDevs.sort((o1, o2)->o1.getAge()-o2.getAge());
```



根据名字

```java
    //sort by name
    Collections.sort(listDevs, new Comparator<Developer>() {
        @Override
        public int compare(Developer o1, Developer o2) {
            return o1.getName().compareTo(o2.getName());
        }
    });

    //lambda
    listDevs.sort((Developer o1, Developer o2)->o1.getName().compareTo(o2.getName()));

    //lambda
    listDevs.sort((o1, o2)->o1.getName().compareTo(o2.getName()));
```



根据薪水

```java
    //sort by salary
    Collections.sort(listDevs, new Comparator<Developer>() {
        @Override
        public int compare(Developer o1, Developer o2) {
            return o1.getSalary().compareTo(o2.getSalary());
        }
    });

    //lambda
    listDevs.sort((Developer o1, Developer o2)->o1.getSalary().compareTo(o2.getSalary()));

    //lambda
    listDevs.sort((o1, o2)->o1.getSalary().compareTo(o2.getSalary()))
```



倒序

- 正常排序

```java
    Comparator<Developer> salaryComparator = (o1, o2)->o1.getSalary().compareTo(o2.getSalary());
    listDevs.sort(salaryComparator);
```



- 倒序

```java
    Comparator<Developer> salaryComparator = (o1, o2)->o1.getSalary().compareTo(o2.getSalary());
    listDevs.sort(salaryComparator.reversed());
```



### 拓展

**匿名内部类**

所谓匿名内部类，是指可以利用内部类创建没有名称的对象，它一步完成了声明内部类和创建该类的一个对象，并利用该对象访问到类里面的成员.

下面看一段代码

```java
public class InnerclassTest {  //外部类
    
    String name="黄林";
  
    public static void main(String []args){
      (
              new InnerclassTest(){  //匿名内部类开始
                  void setName(String n){
                      this.name = n;
                      System.out.println("内部类---姓名："+name);
                  }
              }
              ).setName("王佳");      //匿名内部类结束
  } 
}
```

运行结果：

```
内部类---姓名：王佳
```

这里的内部类是 

1. 直接用外部类InnerclassTest的名字new一个对象，

2. 并紧接着定义类体的，这个内部类没有自己的名字，甚至将创建对象的工作直接交给了其父类InnerclassTest（在这里InnerclassTest既是一个外部类，也是匿名内部类的父类），
3. 在匿名内部类中重新给其父类成员name赋值，所以最后的结果是匿名内部类中所给的值将其父类成员变量的初始值给覆盖了。

匿名内部类不需要class关键字，自然也不需要public，private等修饰，它没有构造方法，一般用来补充内部类中没有定义的内容，或者实现外部接口或其抽象父类中的抽象方法，这样使代码更加简短。比如上面的代码就为其外部类中的成员name属性添加了setName()方法，并利用匿名内部类对象调用该方法并为name赋值。

over。

更详细的可以通过下方参考链接看原文，此处只是简单的描述及理解。



### 原文

《Functional Programming in Java: Harnessing the Power Of Java 8 Lambda Expressions》的第三章节（网上的一些博客网站能找到翻译）

[匿名内部类](https://blog.csdn.net/hst_gogogo/article/details/82798673)

[Java 中 Comparable 和 Comparator 比较](https://www.cnblogs.com/skywang12345/p/3324788.html)

[Comparator and Comparable in Java](https://www.baeldung.com/java-comparator-comparable)

[java8-Lambda中比较器Comparator的使用](https://blog.csdn.net/u014042066/article/details/76248692)

[Java 8 Lambda : Comparator 示例](https://cloud.tencent.com/developer/article/1771454)

[Java 8 Lambda : Comparator example](https://mkyong.com/java8/java-8-lambda-comparator-example/)