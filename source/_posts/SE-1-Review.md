---
title: SE-1 Review
date: 2022-09-06 19:27:34
tags:
---

# Python部分

1. parameter是形参，argument是实参。

2. title() 方法返回一个字符串，其中每个单词的第一个字符均为大写。如果单词包含数字或符号，则其后的第一个字母将转换为大写字母。

3. 换零钱问题（递归、动态规划）：给定半美元、25美分、10美分、5美分、1美分 5种硬币，将 1 美元换成硬币，有多少种硬币组合？
   给定任意数量的现金 和 任意组合的硬币种类，计算换零钱所有方式的种数。

   10 美分能够用到的硬币种类只有`[10, 5, 1]`三种。我们用 `10$[10, 5, 1]`这种记法来标记问题。先按币值为10的硬币来分类，将问题化为：

   1. 第一组，不使用 10 美分的硬币来表示，只用5美分和1美分来表示 即 `10$[5, 1]`

   2. 第二组，使用了 10 美分的硬币，剩下的金额 0 使用全部的硬币种类表示，即 `0$[10, 5, 1]`

      `10$[10, 5, 1] = 10$[5, 1] + 0$[10, 5, 1]
                    = 10$[1] + 5$[5, 1] + 0$[10, 5, 1]
                    = 10$[1] + 5$[1] + 0$[5, 1] + 0$[10, 5, 1]`

   分成两组后，这两个问题新问题相对于原问题的范围缩小了，第一组现金没有变，但可选的硬币种类少了一种；第二组硬币种类没有变，但是现金减少了第一种硬币的币值。

   边界问题如下：

   - 当现金数 `a` 为 0 时，应该算作是有 1 种换零钱的方法
   - 当现金数 `a` 小于 0 时，应该算作是有 0 种换零钱的方法
   - 当换零钱可选的硬币种类为 0 时，应该算作是有 0 种换零钱的方法

   按边界规则，可以直观看出，当金额为 0 或可选币种只有 1 种时，组合数都为 1。

   一定要**小心边界出口**，仔细考虑设计边界条件

   ```python
   # -- coding = utf-8 --
   def count_change(amount, coins_list):
       if amount == 0:
           return 1
       elif amount < 0 or not coins_list: 
           return 0
       else:
           part1 = count_change(amount, coins_list[1:])
           part2 = count_change(amount-coins_list[0], coins_list[:])
       return part1 + part2
   
   us_coins = [1, 5, 10, 25, 50]
   print(count_change(100, us_coins)
   ```

   没有硬币种类换就返回0，amount<0防止

   50$[20,10,5,1]->30$[20,10,5,1] - >10$[20,10,5,1]->10$[20,10,5,1]

4. 思考递归问题时，先将其分解为几个子问题，然后分别判断这几个子问题在哪些方面缩减了问题的规模，以此来考虑边界情况。

5. 在 [Python](http://c.biancheng.net/python/) 中，有一个特殊的常量 None（N 必须大写）。和 False 不同，它不表示 0，也不表示空字符串，而表示没有值，也就是空值。这里的空值并不代表空对象，即 None 和 []、“” 不同。

   <!--more-->

6. **sort()** 函数用于对原列表进行排序，会改变原列表中数据的顺序，sorted 可以对所有可迭代的对象进行排序操作， sorted 方法返回的是一个新的 list，而不是在原来的基础上进行的操作。

7. 对字符串的操作方法都不会改变原来字符串的值，下面是一些常用的字符串方法：

   1.去掉空格和特殊符号
   name.strip() 去掉空格和换行符
   name.strip(‘xx’) 去掉某个字符串
   name.lstrip() 去掉左边的空格和换行符
   name.rstrip() 去掉右边的空格和换行符
   2.字符串的搜索和替换
   name.count(‘x’) 查找某个字符在字符串里面出现的次数
   name.capitalize() 首字母大写
   name.center(n,’-’) 把字符串放中间，两边用- 补齐
   name.find(‘x’) 找到这个字符返回下标，多个时返回第一个；不存在的字符返回-1
   name.index(‘x’) 找到这个字符返回下标，多个时返回第一个；不存在的字符报错
   name.replace(oldstr, newstr) 字符串替换
   name.format（） 字符串格式化
   name.format_map(d) 字符串格式化，传进去的是一个字典
   3.字符串的分割
   name.split() 默认是按照空格分割
   name.split(’,’) 按照逗号分割

   str.upper()用法：将字符串全部变成大写
   str.lower()用法：将字符串全部变成小写
   string.split(text)#字符串分割?
   string.join(string.split(text), “+”)#字符串连接?
   string.replace(text, “Python”, “Java”)#字符串替换?
   string.count(text, “n”)#字符串计数?
   string.find(text, “Python”), string.find(text,“Java”)#字符串查找

8. 要学会从题目中抽象出一些东西来，而不是按照题目给的要求来模拟，不要被题目说的那些方法步骤所局限，要跳出题目给的方法和步骤，想一想能不能从另一个角度来思考这个问题，或者说用另一种方式来模拟这个题目，有时候可以直接根据题目文字来模拟，有时候要抽象一下题目给的条件，用另一种方式来模拟或者是计算，解题两种路径**模拟**和**计算**

9. 比较两个列表：

   1. “==”只有成员、成员位置都相同时才返回True，但有时候我们希望只要成员相同、即使成员位置不同也能返回True，可以使用列表sort()方法进行排序后比较，注意sort()会改变原列表。sorted()不改变列表原本顺序而是新生成一个排序后的列表并返回。

      ```python
      list1 = ["one","two","three"]
      
      list2= ["one","three","two"]
      
      sorted(list1)==sorted(list2)
      ```

   2. 包含比较

      直接用列表本身进行包含类比较，只能用遍历的方法这是比较麻烦的，使用set()转成集合进行包含比较就简单多了。

      - 判断列表是否包含另一列表

      ```py
      list1 = ["one","two","three"]
      
      list2= ["one","three","two","four"]
      
      set(list1).issubset(set(list2))
      
      set(list2).issuperset(set(list1))
      ```

      - 求交集

      ```python
      list1 = ["one","two","three","five"]
      
      list2= ["one","three","two","four"]
      
      set(list1).intersection(set(list2))
      ```

      - 获取两个列表不同成员

      ```python
      list1 = ["one","two","three","five"]
      
      list2= ["one","three","two","four"]
      
      set(list1).symmetric_difference(set(list2))
      ```

      - 获取一个列表中不是另一个列表成员的成员（差集）

      ```python
      list1 = ["one","two","three","five"]
      
      list2= ["one","three","two","four"]
      
      set(list1).difference(set(list2))
      
      set(list2).difference(set(list1))
      ```

      - 求并集

      ```python
      list1 = ["one","two","three","five"]
      
      list2= ["one","three","two","four"]
      
      set(list1).union(set(list2))
      ```

10. string.format方法，`print("{:.2f}".format(3.1415926))`

   {:.2f} 保留小数点后两位

   {:+.2f} 带符号保留小数点后两位

   {:0>2d} 数字补零 (填充左边, 宽度为2)

   {:x<4d} 数字补x (填充右边, 宽度为4)

# Java部分

1. 一道我没做起的Java简单题

##### 	题目要求

​	写一个程序来检测一个整数是不是丑数。

​	丑数的定义是，只包含质因子 2, 3, 5 的正整数。比如 6, 8 就是丑数，但是 14 不是丑数因为他包含了质因子 7。

##### 	注意事项

​	可以认为 1 是一个特殊的丑数。

##### 	示例

​	给出 num = 8，返回 true。 给出 num = 14，返回 false。

​	**思路**：只包含质因子2 3 5意味着该数可以被分解为2<sup>x</sup> * 3<sup> y</sup> * 5<sup>z</sup>,使 sum 依次对 2, 3, 5 相除，直到与 2, 3, 5 的余数不为 0，最终 sum 为 1，则代表该数只能被 2, 3, 5整除，返回 `true`，反之返回 `false`。

```java
public static boolean isUgly(int num) {
    //0也需要特殊判断，但题目说了是正整数
    if (num == 1) {
        return true;
    }
    while (num%2==0){
        num=num/2;
    }
    while (num%3==0){
        num=num/3;
    }
    while (num%5==0){
        num=num/5;
    }
    return num == 1;
}
```

2. 手动实现Java的trim函数

   ```java
   public String trim(String str) {
       //String abc = "   a b c ddsa    ";
       //在遍历中其实我们只需要获得第一个和最后一个不为空格的字符的下标
       char[] chars = str.toCharArray();
       int start = 0;
       int end = 0;
       for (int i = 0; i < chars.length; i++) {
           if (chars[i] != ' ') {
               start = i;
               break;
           }
       }
       for (int i = chars.length - 1; i >= 0; i--) {
           if (chars[i] != ' ') {
               end = i;
               break;
           }
       }
       return str.substring(start,end+1);//substring不包含后一个参数，所以需要+1
   }
   ```

3. 正则表达式：*表示匹配前面的字符0次或多次，+表示1次或多次，？表示0次或1次，{n}匹配确定的n次，{n，}匹配至少n次，{n，m}匹配n到m次两边都包含

# 转专业考试复习

1. 高阶函数：

   ```java
   public class HighOrderFunction {
       public static int cube(int x){
           return x*x*x;
       }
   
       /**
        * 计算a到b的立方和（包含b）
        * @param a
        * @param b
        * @return
        */
       public static int fCube(int a,int b){
           if (a>b){
               return 0;
           }
           return cube(a)+fCube(a+1,b);
       }
   
   
       public static double piTerm(int x){
           return 1.0/(x*(x+2));
       }
       public static int piNext(int x){
           return x+4;
       }
   
       /**
        * 计算1/(1*3)+1/(5*7)+1/(9*11)……序列和，从a到b（包含b）
        * @param a
        * @param b
        * @return
        */
       public static double sumPi(int a,int b){
           if (a>b){
               return 0;
           }
           return piTerm(a)+sumPi(piNext(a),b);
       }
       public static void main(String[] args) {
           System.out.println(fCube(1,2));
           System.out.println(sumPi(1,5));
       }
   }
   ```


# VJAM实验报告



