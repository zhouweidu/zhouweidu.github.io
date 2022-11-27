---
title: c++高级程序设计
date: 2022-09-13 11:24:40
tags:
- C++
categories:
- 南大课程
typora-root-url: c-高级程序设计
---

![image-20220913112451688](image-20220913112451688.png)

# 结构化程序设计部分

## 1. 数据类型

`int x=8;` x的取值范围被称为值集

![image-20220915121433553](image-20220915121433553.png)

<!--more-->

# 作业部分

## 1. 判断int是否溢出

### 1.1 用INT_MAX

INT_MAX这个宏定义了int的最大值，用long long来定义结果变量，如果得到的结果大于INT_MAX则发生了溢出，不能用int定义结果变量，因为是int类型，就算发生了溢出也会截断一些位来使变量保持在int范围内（溢出会发生未定义的行为），所以int类型的变量永远不可能超过INT_MAX。

### 1.2 用数学方法

```c++
void test(){	
	int a = 964632435;
	a = a * 10;
	cout<<a<<endl;//1056389758
}
```

我们不能通过这样的简单的判断逻辑来进行，发生了溢出，数据已经改变了，不能用改变后的数据（改变后的数据一定是通过了一些丢位操作，使得数值在Int类型的范围内，其实这是未定义的行为）去判断该数值是否溢出。

思想：相加的结果与其中一个数相减，与另一个数比较看是否相等；相乘除的结果和乘除的因子相反操作，和原来的数值进行比较看是否相等。

# 2. 最小公倍数和最大公约数

**两个数的乘积等于这两个数的最大公约数与最小公倍数的积**，可以用欧几里得算法求出最大公约数进而得出最小公倍数。

```c++
int gcd(int a, int b){
    if (a%b == 0) {
        return b;
    }
    return gcd(b, a%b);
}
```

```c++
int gcd(int a, int b){
    int temp = a;
    while(a%b != 0){
        a = b;
        b = temp%b;
        temp = a;
    }
    return b;
}
```

# 3. C++的类

## 3.1 拷贝控制

![image-20221029162402898](image-20221029162402898.png)

# 机考复习

## 1. 输入

split函数的实现

```c++
vector<string> split(string str, char delimiter) {
    vector<string> strVec;
    //show,me,
    for (int i = 0; i < str.size(); ++i) {
        if (str[i] != delimiter) {
            int j;
            for (j = i; j < str.size() && str[j] != delimiter; ++j) {

            }
            string temp=str.substr(i,j-i);
            strVec.push_back(temp);
            i=j;
        }
    }
    return strVec;
}
```

# 2. STL的使用

std::priority_queue 优先队列的使用

```c++
#include <iostream>
#include <queue>
using namespace std;

/******** 定义类时同时定义操作符重载函数 ********/
struct Node1
{
    // 要比较的元素
    int x;
    // 构造函数
    Node1(int x) { this->x = x; }
    // 操作符重载函数，必须是具体的操作符<之类的，写()报错
    bool operator<(const Node1 &b) const
    {
        // 实现less中需要的<,大顶堆
        return x < b.x;
    }
};

/******** 自定义类，自定义比较函数 ********/
struct Node2
{
    // 要比较的元素
    int x;
    // 构造函数
    Node2(int x) { this->x = x; }
};

// 操作符重载函数，必须是具体的操作符<之类的，写()报错
bool operator<(const Node2 &a, const Node2 &b)
{
    // less,大顶堆
    return a.x < b.x;
}

/******** 自定义类，自定义包含比较函数的结构体 ********/
struct Node3
{
    // 要比较的元素
    int x;
    // 构造函数
    Node3(int x) { this->x = x; }
};

struct cmpClass
{
    // 操作符重载函数，必须是写()
    bool operator()(const Node3 &a, const Node3 &b)
    {
        // less,大顶堆
        return a.x < b.x;
    }
};

int main()
{
    /******** 初始化优先级队列的对象p ********/
    // Node1类型，默认使用vector，小顶堆，同 priority_queue<Node1, vector<Node1>, less<Node1> > p;
    priority_queue<Node1> p;

    // 乱序入队
    p.emplace(1);
    p.emplace(3);
    p.emplace(2);

    // 弹出队首
    while (!p.empty())
    {
        cout << p.top().x << " ";
        p.pop();
    }
    cout << endl;
    // 3 2 1

    /******** 初始化优先级队列的对象q ********/
    // 同 priority_queue<Node2> q;
    priority_queue<Node2, vector<Node2>, less<Node2>> q;

    // 乱序入队
    q.emplace(1);
    q.emplace(3);
    q.emplace(2);

    // 弹出队首
    while (!q.empty())
    {
        cout << q.top().x << " ";
        q.pop();
    }
    cout << endl;
    // 3 2 1

    /******** 初始化优先级队列的对象r ********/
    priority_queue<Node3, vector<Node3>, cmpClass> r;

    // 乱序入队
    r.emplace(1);
    r.emplace(3);
    r.emplace(2);

    // 弹出队首
    while (!r.empty())
    {
        cout << r.top().x << " ";
        r.pop();
    }
    cout << endl;
    // 3 2 1
    return 0;
}

```

![image-20221109145041019](/image-20221109145041019.png)

![image-20221109145051045](/image-20221109145051045.png)
