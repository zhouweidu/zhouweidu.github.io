---
title: review on java
date: 2022-09-06 19:25:58
tags:
typora-root-url: review-on-java
---

## 对象和类

1. 静态方法不能访问某一对象的实例域，因为它不能操作对象，但静态的方法可以访问自身类中的静态域

2. 在下面两种情况下使用静态方法：一个方法不需要访问对象状态，其所需参数都是通过显式参数提供（例如：Math.pow（)）；一个方法只需要访问类的静态域

3. **Scanner类的一些知识点**：next()方法如果没有扫描读入非空格或非回车字符是不会创建String对象并返回的，并且next()遇到空格或者说\t或者是回车都会结束扫描；nextLine():读取输入，包括单词之间的空格和除回车以外的所有符号(即。它读到行尾)。读取输入后，nextLine()将光标定位在下一行（会忽略掉末尾的回车进入下一行开始扫描）。所以它和next()的区别就是它没有分隔符，直接扫描全部的键盘输入内容，并创建对象进行将其引用返回。

   Scanner类还可以用来**读取文件**

   ```java
   try {
       scanner = new Scanner(new File(fileName));//注意传入的是File
   } catch (FileNotFoundException e) {
       e.printStackTrace();
   }
   ```

   使用Scanner对象中hasNext()方法来判断文件是否读取完毕，另外一个就是用来获取控制台输入的nextLine(),nextInt()等方法来获取文本的信息，**非常类似于自己在控制台输入的数据变成了文本内容，给Scanner对象获取**。

   nextInt:**Scanner中的nextInt()只会读取数值，剩下"\n"还没有读取**

   next()与nextLine()区别

   next():

   - 1、一定要读取到有效字符后才可以结束输入。
   - 2、对输入有效字符之前遇到的空白，next() 方法会自动将其去掉。
   - 3、只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符。
   - 4、next() 不能得到带有空格的字符串。

   nextLine()：

   - 1、以Enter为结束符,也就是说 nextLine()方法返回的是输入回车之前的所有字符，并且将光标定位在下一行（会忽略掉末尾的回车进入下一行开始扫描）。
   - 2、可以获得空白。

4. Java控制台输入输出

   - 使用Scanner

   `Scanner scanner=new Scanner(System.in)`,然后使用Scanner相关的方法，比如scanner.nextInt() scanner.nextLine() scanner.next()等

   - 使用BufferReader

   ```java
   BufferedReader bufferedReader=new BufferedReader(new InputStreamReader(System.in));
   try {
       String str1=bufferedReader.readLine();
       String str2=bufferedReader.readLine();
       System.out.print(str1);
       System.out.print(str2);
   } catch (IOException e) {
       e.printStackTrace();
   }
   ```

<!--more-->

## 多态

1. 通过**后期绑定**（也叫动态绑定）来确定一个引用所调用的方法是哪一个， private 方法、 static 方法或 final 方法不会动态绑定，在运行时， 调用 e.getSalary（） 的解析过程为（e是Employee类的一个对象）：

   在 Employee 的方法表中， 列出了这个类定义的所有方法（实际上没列出超类Object的方法）：

    ![Java-method-table1](Java-method-table1.png)

   在Manager的方法表中，有三个方法是继承而来的，一个方法是重新定义的， 还有一个方法是新增加的：

   ![Java-method-table2](Java-method-table2.png)

   1 ) 首先，虚拟机提取 e 的实际类型的方法表。既可能是 Employee、 Manager 的方法表， 也可能是 Employee 类的其他子类的方法表（有可能e的实际类型是Employee的子类但赋值给employee引用）。 

   2 ) 接下来， 虚拟机搜索定义 getSalary 签名的类（方法的名字和参数列表称为方法的签名）。此时，虚拟机已经知道应该调用哪个 方法。 

   3 ) 最后，虚拟机调用方法。动态绑定有一个非常重要的特性： 无需对现存的代码进行修改，就可以对程序进行扩展。 假设增加一个新类 Executive, 并且变量 e 有可能引用这个类的对象， 我们不需要对包含调用 e.getSalary() 的代码进行重新编译。如果 e 恰好引用一个 Executive 类的对象，就会自动地调用 Executive.getSalary（）方法。

## 静态方法和静态变量

1. 在同一个类中：
   对于静态方法，其他的静态或非静态方法都可以直接调用它。
   而对于非静态方法，其他的非静态方法是可以直接调用它的。但是其他静态方法只有通过对象才能调用它。

   静态方法不能被非静态方法覆盖。

2. 不同的类之间，无论调用方法是非静态还是静态，如果被调用的方法是：
   静态方法，则通过类名与对象都可以调（但通过对象的方式不建议使用，通过对象来调用静态的方法静态方法还是不知道它是被哪个对象所调用的，这会产生误解）。
   非静态方法，则只能通过对象才可以调用它。

## 接口

1. 接口中的所有方法自动地属于 public，接口中所有的域被自动的设为public static final，但实现接口时要显式的指出方法是public的，接口中可能包含多个方法，在接口中可以定义常量，但接口绝对不能含有实例域，接口最好没有实现方法，虽然Java8可以在接口中提供简单方法
2. 不能构造接口的对象，却能声明接口的变量，接口变量必须引用实现了接口的类对象
3. 在方法中定义的局部类不能用public或private访问说明符进行说明，它的作用域被限定在声明这个局部类的块中
4. 内部类方法可以访问该类定义所在的作用域中的数据， 包括私有的数据
5. 内部类没太看懂，不知道重不重要

## 异常

1. 如果出现RuntimeException（非受查异常）异常，那么就一定是你的问题，有可能是因为程序的逻辑错误导致的，这些运行时错误完全在我们的控制之下，但已检查异常需要提供异常处理器

2. 方法应该在其首部声明所有可能抛出的异常。这样可以从首部反映出这个方法可能抛出哪类受査异常。

3. 运行时异常：编译时不会报错，但程序运行起来如果有错误就会报异常。下面是几种常见的运行时异常

   ArithmeticException  算数运算异常，由于除数为0引起的异常； 
   ClassCastException  类型转换异常，当把一个对象归为某个类，但实际上此对象并不是由这个类创建的，也不是其子类创建的，则会引起异常；
   ArrayStoreException  由于数组存储空间不够引起的异常； 
   NullPointerException  空指针异常，程序试图访问一个空的数组中的元素或访问空的对象中的方法或变量时产生异常；
   IndexOutOfBoundsExcention  索引越界异常，由于数组下标越界或字符串访问越界引起异常； 
   ConcurrentModificationException  并发修改异常；
   NoSuchElementException  找不到元素异常； 
   UnsupportedOperationException  不支持请求异常；（使用Arrays工具类的asList将数组转成集合增加元素时，会报此异常）

## 泛型程序设计

1. 一个泛型类（ generic class) 就是具有一个或多个类型变量的类。

2. 下面的这个Pair就是一个泛型类，类定义中的类型变量指定方法的返回类型以及域和局部变量的类型

```java
public class Pair<T>
{
    private T first;
    private T second;
    public Pair() { first = null ; second = null ; }
    public Pair(T first, T second) 
    { this.first = first; this.second = second; }
    public T getFirst() { return first; }
    public T getSecond() { return second; }
    public void setFirst(T newValue) { first = newValue; }
    public void setSecond(T newValue) { second = newValue; }
}
```

3. 对类型变量 T 设置**限定**：`public static <T extends Comparable> T min(T[] a) `,一个类型变量或通配符可以有多个限定， 例如： `T extends Comparable & Serializable` 限定类型用“ &” 分隔，而逗号用来分隔类型变量。
4. **泛型的一些东西没搞清楚**，但基本上应该使用泛型没什么问题

## 集合

1. Java 迭代器指向两个元素之间的位置

2. 树集是一个有序集合 ( sorted collection) o，可以以任意顺序将元素插入到集合中。在对集合进行遍历时，每个值将自动地按照排序后的顺序呈现。

3. Java 集合类型分为 Collection 和 Map，它们是 Java 集合的根接口，这两个接口又包含了一些子接口或实现类

   ![Collection接口结构](5-1912051036333V.png)

   ![Map接口结构](5-191205103G5960.png)

4. Java集合接口的作用

   | 接口名称        | 作  用                                                       |
   | --------------- | ------------------------------------------------------------ |
   | Iterator 接口   | 集合的输出接口，主要用于遍历输出（即迭代访问）Collection 集合中的元素，Iterator 对象被称之为迭代器。迭代器接口是集合接口的父接口，实现类实现 Collection 时就必须实现 Iterator 接口。 |
   | Collection 接口 | 是 List、Set 和 Queue 的父接口，是存放一组单值的最大接口。所谓的单值是指集合中的每个元素都是一个对象。一般很少直接使用此接口直接操作。 |
   | Queue 接口      | Queue 是 Java 提供的队列实现，有点类似于 List。              |
   | Dueue 接口      | 是 Queue 的一个子接口，为双向队列。                          |
   | List 接口       | 是最常用的接口。是有序集合，允许有相同的元素。使用 List 能够精确地控制每个元素插入的位置，用户能够使用索引（元素在 List 中的位置，类似于数组下标）来访问 List 中的元素，与数组类似。 |
   | Set 接口        | 不能包含重复的元素。                                         |
   | Map 接口        | 是存放一对值的最大接口，即接口中的每个元素都是一对，以 key→value 的形式保存。 |

5. Java集合实现类的作用

   | 类名称     | 作用                                                         |
   | ---------- | ------------------------------------------------------------ |
   | HashSet    | 为优化査询速度而设计的 Set。它是基于 HashMap 实现的，HashSet 底层使用 HashMap 来保存所有元素，实现比较简单 |
   | TreeSet    | 实现了 Set 接口，是一个有序的 Set，这样就能从 Set 里面提取一个有序序列 |
   | ArrayList  | 一个用数组实现的 List，能进行快速的随机访问，效率高而且实现了可变大小的数组 |
   | ArrayDueue | 是一个基于数组实现的双端队列，按“先进先出”的方式操作集合元素 |
   | LinkedList | 对顺序访问进行了优化，但随机访问的速度相对较慢。此外它还有 addFirst()、addLast()、getFirst()、getLast()、removeFirst() 和 removeLast() 等方法，能把它当成栈（Stack）或队列（Queue）来用 |
   | HsahMap    | 按哈希算法来存取键对象                                       |
   | TreeMap    | 可以对键对象进行排序                                         |

6. comparable和comparator：

   `Comparable & Comparator`都是用来实现集合中元素的比较、排序的，只是 Comparable 是在集合内部定义的方法实现的排序，`Comparator` 是在集合外部实现的排序，所以，如想实现排序，就需要在集合外定义`Comparator`接口的方法或在集合内实现 `Comparable`接口的方法

   Java中有两种方式来提供比较功能。第一种是实现java.lang.Comparable接口，使你的类天生具有比较的能力，此接口很简单，只有一个`compareTo`一个方法。此方法接收另一个Object为参数，如果当前对象小于参数则返回负值，如果相等则返回零，否则返回正值，也就是：
    **x.compareTo(y) 来“比较x和y的大小”。若返回“负数”，意味着“x比y小”；返回“零”，意味着“x等于y”；返回“正数”，意味着“x大于y”。**

   使用Comparable比较的例子：

   ```java
   class Person implements Comparable<Person>{
       @Override
       public int compareTo(Person person) {
           return name.compareTo(person.name);
           //return this.name - person.name;
       }
   }
   ArrayList<Person> list = new ArrayList<Person>();
   // 添加对象到ArrayList中
   list.add(new Person("aaa", 10));
   list.add(new Person("bbb", 20));
   list.add(new Person("ccc", 30));
   list.add(new Person("ddd", 40));
   Collections.sort(list); //这里会自动调用Person中重写的compareTo方法。
   ```

   使用Comparator比较的例子:

   ```java
   package com.zhouweidu.se1codetest;
   
   import java.util.*;
   
   public class Compare {
       public static void main(String[] args) {
           ArrayList<Person2> person2s = new ArrayList<>();
           person2s.add(new Person2(1 ,2));
           person2s.add(new Person2(2 ,1));
           person2s.add(new Person2(4 ,6));
           person2s.add(new Person2(6 ,4));
           person2s.sort(new Comparator<Person2>() {
               @Override
               public int compare(Person2 o1, Person2 o2) {
                   //return Integer.compare(o1.getAge(),o2.getAge());
                   return o2.getAge() - o1.getAge();
               }
           });
           System.out.println(person2s);
       }
   }
   class Person2{
       private int age;
       private int id;
   
       public Person2(int age, int id) {
           this.age = age;
           this.id = id;
       }
   
       public int getAge() {
           return age;
       }
   
       public int getId() {
           return id;
       }
   
       @Override
       public String toString() {
           return this.age+" "+this.id;
       }
   }
   ```


## 反射

1. 反射的特征：动态性（编译时无法确定，在运行时确定下来）
2. 在一个类中最好提供一个public的空参构造器，便于通过反射去创建运行时类的对象，便于子类继承此运行时类时，默认调用super()时保证父类有此构造器
3. 框架=注解+反射+设计模式

## 内部类

1. 内部类的分类：成员内部类，局部内部类

## 多线程

1. 同步监视器，俗称锁。任何一个类的对象都可以充当锁。要求：多个线程必须要共用同一把锁，即充当锁的对象只有一个
2. 同步方法synchronized默认的锁，对于非静态方法是this，静态方法是`当前类.class`
3. 线程通信涉及到三个方法：
   - wait()：一旦执行此方法，当前线程就进入阻塞状态，并释放同步监视器
   - notify()：一旦执行此方法，就会唤醒被wait的一个线程，如果有多个线程被wait，我们就唤醒优先级高的那个
   - notifyAll()：一旦执行此方法，就会唤醒所有被wait的线程
4. 线程通信的注意点：
   - wait()、notify()、notifyAll()这三个方法必须使用在同步代码块或同步方法中。
   - wait()、notify()、notifyAll()这三个方法的调用者必须是同步代码块或同步方法中的同步监视器，否则会出现异常。

## 一些零散知识点

1. 块（即复合语句）是指由一对大括号括起来的若干条简单的 Java 语句

2. 不能在嵌套的两个块中声明同名的变量

3. instanceof（）方法左边是对象，右边是类，当对象是右边类或子类所创建的对象时，返回true，否则返回false

4. 设计方法的第一步是**先对数据的合理性进行校验**

5. 类之间的关系：依赖（“uses-a”）：一个类的方法操纵另一个类的对象；聚合（”has-a“）：类A对象包含类B对象；继承（”is-a“）：特殊与一般的关系

6. 单引号是用来写字符型的，例如```char str='a'```，而双引号是用来写字符串的

7. 开发类时的经验：

   （1）找出类应该做的事情

   （2）列出实例变量和方法

   （3）编写方法的伪码

   （4）编写方法的测试用程序

   （5）实现类

   （6）测试方法

   （7）除错或重新设计

8. logo用面向对象的思想重写一遍：有两个类一个是Turtle一个是Chessboard，当乌龟移动时，就在Chessboard上留下痕迹，这部分是在main方法里面实现的，感觉这部分做的不是很好，乌龟和棋盘的关系是棋盘上有只乌龟，或许应该在棋盘类中创建乌龟的实例？（“has-a”关系？），继续学习用**面向对象的观点和方法**来思考

9. 方法签名是指方法的名称（即函数名）和方法的参数

10. 在定义方法时，在最后一个形参后加上三点 **…**，就表示该形参可以接受多个参数值，多个参数值被当成数组传入。上述定义有几个要点需要注意：

- 可变参数只能作为函数的最后一个参数，但其前面可以有也可以没有任何其他参数
- 由于可变参数必须是最后一个参数，所以一个函数最多只能有一个可变参数
- Java的可变参数，会被编译器转型为一个数组
- 变长参数在编译为字节码后，在方法签名中就是以数组形态出现的。这两个方法的签名是一致的，不能作为方法的重载。如果同时出现，是不能编译通过的。可变参数可以兼容数组，反之则不成立

```java
public void foo(String...varargs){}

foo("arg1", "arg2", "arg3");

//上述过程和下面的调用是等价的
foo(new String[]{"arg1", "arg2", "arg3"});
```

11. Equals 与 hashCode 的定义必须一致：如果 x.equals(y) 返回 true, 那么 x.hashCode( ) 就必 须与 y.hashCode( ) 具有相同的值。

12. 正则表达式：

    | 代码 | 功能                                        |
    | :--- | :------------------------------------------ |
    | .    | 匹配任意1个字符（除了\n）                   |
    | [ ]  | 匹配[ ]中列举的字符                         |
    | \d   | 匹配数字，即0-9                             |
    | \D   | 匹配非数字，即不是数字                      |
    | \s   | 匹配空白，即 空格，tab键                    |
    | \S   | 匹配非空白                                  |
    | \w   | 匹配非特殊字符，即a-z、A-Z、0-9、_、汉字    |
    | \W   | 匹配特殊字符，即非字母、非数字、非汉字、非_ |

13. 对**递归**的理解：倒着想问题，正着写代码，先想问题的前一步，也就是再走一步就可以解决问题的这个点，然后看从这一点到解决问题有多少种方法，一般而言这几种方法就是从起点走到解决问题的这些方法，但也有可能不是，可能起点开始的时候方法选择会更多，所以要综合第一步来考虑，综合后这就是递归的路径，然后根据不同的路径判断**递归的边界条件**（边界条件非常重要），首先看问题规模是朝着什么方向缩减的，这可以写成if，然后再看看会不会有走向死路（死循环）的情况，这个边界条件也要加上去，这个地方的边界条件可能有点难找，多考虑几个样例来试试，切记分析问题时可以从正向和反向两个方面来分析，反向：假设这样可以会怎么怎么样。

## Java高级特性反射、代理（等到今后需要用到时再了解吧）

