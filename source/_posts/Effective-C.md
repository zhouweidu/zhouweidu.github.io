---
title: Effective C++
date: 2022-09-18 15:54:01
tags:
- C++
categories:
- 个人技术栈
typora-root-url: Effective-C
---

## 条款01:  视C++ 为一个语言联邦

![image-20220918165735085](image-20220918165735085.png)

## 条款02: 尽量以 const, enum, inline 替换 #define

<!--more-->

## 条款03: 尽可能使用 const

- 将某些东西声明为 const 可帮助编译器侦测出错误用法。 const可被施加于任何作用域内的对象、函数参数、函数返回类型、成员函数本体。 

- 编译器强制实施 bitwise constness ，但你编写程序时应该使用"概念上的常量性" (conceptual constness) 
- 当 const  non-const 成员函数有着实质等价的实现时，令 non-const 版本调 const 版本可避免代码重复。

## 条款 04: 确定对象被使用前已先被初始化

- 为内置型对象进行手工初始化，因为 C++不保证初始化它们。 
- 构造函数最好使用成员初值列 (member initialization list) ，而不要在构造函数 本体内使用赋值操作(assignment) 。初值列列出的成员变量，其排列次序应该 和它们在 class 中的声明次序相同。 
- 为免除"跨编译单元之初始化次序"问题，请以local static 对象（函数内的static对象）替换 non-local static 对象

## 条款 05: 了解 C++ 默默编写并调用哪些函数

编译器可以暗自为 class 创建 default 构造函数、 copy 构造函数、 copy assignment 操作符，以及析构函数。

## 条款 06: 若不想使用编译器自动生成的函数，就该明确拒绝

## 条款 07: 为多态基类声明 virtual 析构函数

- polymorphic (带多态性质的) base classes 应该声明一个 virtual 析构函数。如果 class 带有任何 virtual 函数，它就应该拥有一个 virtual 析构函数。 
- Classes 的设计目的如果不是作为 base classes 使用，或不是为了具备多态性 (polymorphically) ，就不该声明 virtual 析构函数。（会多出虚函数指针的空间）

## 条款 08: 别让异常逃离析构函数

- 析构函数绝对不要吐出异常。如果一个被析构函数调用的函数可能抛出异常，析构函数应该捕捉任何异常，然后吞下它们(不传播)或结束程序。
- 如果客户需要对某个操作函数运行期间抛出的异常做出反应，那么 class 应该提 供一个普通函数(而非在析构函数中)执行该操作。

## 条款 09: 绝不在构造和析构过程中调用 virtual 函数

在构造和析构期间不要调用 virtual 函数，因为这类调用从不下降至 derived class (比起当前执行构造函数和析构函数的那层)。

## 条款 10: 令operator= 返回一个 reference to *this

## 条款 11: 在operator= 中处理"自我赋值"

**copy and swap 技术**

![image-20220918195311593](image-20220918195311593.png)

## 条款 12: 复制对象时勿忘其每一个成分

- Copying 函数应该确保复制"对象内的所有成员变量"及"所有 base class 成分"。
- 不要尝试以某个 copying 函数实现另一个 copying 函数。应该将共同机能放进第三个函数中，并由两个 coping 函数共同调用。

## 条款 13: 以对象管理资源

## 条款 14: 在资源管理类中小心coping 行为

## 条款 15: 在资源管理类中提供对原始资源的访问

## 条款 16: 成对使用 new delete 时要采取相同形式

## 条款 17: 以独立语句将 new 创建的对象置入智能指针

以独立语句将 new 创建的对象存储于(置入)智能指针内。如果不这样做，一旦异常被抛出，有可能导致难以察觉的资源泄漏

## 条款 18: 让接口容易被正确使用，不易被误用

## 条款 19: 设计 class 犹如设计 type

## 条款 20: 宁以 pass-by -reference-to-const 替换 pass-by-value

- 尽量以 pass-by-reference-to-const 替换 pass-by-value 前者通常比较高效，并可避免切割问题 (slicing problem) 
- 以上规则并不适用于内置类型，以及 STL 的迭代器和函数对象。对它们而言， pass-by-value 往往比较适当。

## 条款 21: 必须返回对象时，别妄想返回其 reference

## 条款 22: 将成员变量声明为private（封装）

## 条款 23: 宁以 non-member non- friend 替换 member 函数

宁可拿 non-member non- friend 函数替换 member 函数。这样做可以增加封装性、包裹弹性 (packaging flexibility )和机能扩充性。

## 条款 24 :若所育参数皆需类型转换，请为此采用 non-member 函数

## 条款 25: 考虑写出一个不抛异常的 swap 函数

普通class写法如下：

![image-20220918213759699](image-20220918213759699.png)

template class写法如下：

![image-20220918214019413](image-20220918214019413.png)

- std: :swap 对你的类型效率不高时，提供一个swap 成员函数，并确定这个函数 不抛出异常。
- 如果你提供一个member swap，也该提供一个non-member swap 用来调用前者。对 classes (而非 templates) ，也请特化 std: :swap。
- 调用 swap 时应针对 std::swap 使用 using 声明式，然后调用 swap 并且不带任何"命 名空间资格修饰"。
- 为"用户定义类型"进行std templates 全特化是好的，但千万不要尝试在std 内加 入某些对 std 而言全新的东西。

## 条款 26: 尽可能延后变量定义式的出现时间

## 条款 27: 尽量少做转型动作

## 条款 28: 避免返回 handles 指向对象内部成分

避免返回 handles (包括 references 、指针、迭代器)指向对象内部。遵守这个条款可增加封装性，帮助 const 成员函数的行为像个 const ，并将发生"虚吊号码牌" (dangling handles) 的可能性降至最低。

## 条款 29: 为"异常安全"而努力是值得的

- 异常安全函数 (Exception-safe functions) 即使发生异常也不会泄漏资源或允许任何数据结构败坏。这样的函数区分为三种可能的保证:基本型、强烈型、不抛异常型。
- "强烈保证"往往能够以 copy-and-swap 实现出来，但"强烈保证"并非对所有函数都可实现或具备现实意义。
- 函数提供的"异常安全保证"通常最高只等于其所调用之各个函数的"异常安全保证"中的最弱者

## 条款 30: 透彻了解 inlining 的里里外外

- 将大多数 inlining 限制在小型、被频繁调用的函数身上。这可使日后的调试过程和二进制升级 (binary upgradability) 更容易，也可使潜在的代码膨胀问题最小化，使程序的速度提升机会最大化。
- 不要只因为 function templates 出现在头文件，就将它们声明为 inline。

## 条款 31: 将文件间的编译依存关系降至最低（解耦合）

## 条款 32: 确定你的 public 继承塑模出 is-a 关系

## 条款 33: 避免遮掩继承而来的名称

- derived classes 内的名称会遮掩 base classes 内的名称。在 public 继承下从来没有 人希望如此。
- 为了让被遮掩的名称再见天日，可使用 using 声明式或转变函数(forwarding functions)

## 条款 34: 区分接口继承和实现继承

- 接口继承和实现继承不同。在 public 继承之下，derived classes 总是继承 base class 的接口。
- virtual 函数只具体指定接口继承。
- 简朴的(非纯) impure virtual 函数具体指定接口继承及缺省实现继承。
- non-virtual 函数具体指定接口继承以及强制性实现继承。

## 条款 35: 考虑 virtual 函数以外的其他选择（涉及到设计模式）

- virtual 函数的替代方案包括 NYl 手法及 Strategy 设计模式的多种形式。 NYI 手法（non-virtual interface) ）自身是一个特殊形式的 Template Method 设计模式。
- 将机能从成员函数移到 class 外部函数，带来的一个缺点是，非成员函数无法访 class non-public 成员。 
- trl::function 对象的行为就像一般函数指针。这样的对象可接纳"与给定之目标签名式 (target signature) 兼容"的所有可调用物 (callable entities)

## 条款 36: 绝不重新定义继承而来的 non-virtual 函数

## 条款 37: 绝不重新定义继承而来的缺省参数值

绝对不要重新定义一个继承而来的缺省参数值，因为缺省参数值都是静态绑定，而virtual函数一一你唯一应该覆写的东西一一却是动态绑定。

## 关于私有虚函数的相关问题

1. 编译器不检查虚函数的各类属性。
2. 编译器在编译子类成员函数时，会先查询父类，如果存在非虚函数，则隐藏父类函数。如果存在虚函数，则重写该位置的虚函数。此时你又可以赋予该函数在该类中的各种属性（例如public）。
3. 私有只是让子类不能访问父类，仅此而已，对其他规则没限制，也就是说可以重写（覆盖）。

## 条款 38: 通过复合塑模出 has-a 或"根据某物实现出"

- 复合(composition，也称为聚合) 的意义和 public 继承完全不同。
- 在应用域 (application domain) ，复含意味 has-a (有一个)。在实现域 (implementation domain) ，复合意味is-implemented-in-terms-of(根据某物实现出)。

## 条款 39: 明智而审慎地使用 private 继承

- 尽可能使用public继承加复合，必要时才使用 private 继承
- Private 继承意味 is-implemented-in-terms of (根据某物实现出)。它通常比复合 (composition) 的级别低。但是当 derived class 需要访问 prot ted base class 成员，或需要重新定义继承而来的virtual 函数时，这么设计是合理的。
- 和复合(composition) 不同， private 继承可以造成 empty base 最优化。这对致 力于"对象只寸最小化"的程序库开发者而言，可能很重要。

## 条款 40: 明智而审慎地使用多重继承

- **C++ 用来解析 (resolving) 重载函数调用的规则**：看到是否有个函数可取用之前，C++ 首先确认这个函数对此调用之言是最佳匹配。 找出最佳匹配函数后才检验其可取用性。
- 多重继承比单一继承复杂。它可能导致新的歧义性，以及对 virtual 继承的需要。
-  virtual 继承会增加大小、速度、初始化(及赋值)复杂度等等成本。如果 virtual base classes 不带任何数据，将是最具实用价值的情况。
- 多重继承的确有正当用途。其中一个情节涉及"public 继承某个 Interface class"和 "private 继承某个协助实现的class" 的两相组合。

## 条款 41: 了解隐式接口和编译期多态

- classes templates 都支持接口(interfaces) 和多态 (polymorphism)
- 对 classes 而言接口是显式的 (explicit) .以函数签名为中心。多态则是通过 virtual 函数发生于运行期。
- 对 template 参数而言，接口是隐式的(implicit) .奠基于有效表达式。多态则是通过 template 具现化和函数重载解析( function overloading resolution) 发生于编译期

## 条款 42: 了解 typename 的双重意义

- 声明 template 参数时，前缀关键字class typenarne可互换。
- 请使用关键字typenarne标识嵌套从属类型名称:但不得在base class lists (基类列，继承时出现在`:`后面的基类列表）或 member initialization list (成员初始化列表）内以它作为 base class 修饰符。

## 条款 43: 学习处理模板化基类内的名称

可在 derived class templates 内通过" this->" 指涉 base class templates 内的成员 名称，或藉由一个明白写出的 "base class 资格修饰符"完成，前一种方法更好，如果调用的是虚函数后一种方法丢失了多态性

## 条款 44: 将与参数无关的代码抽离templates

## 条款 45: 运用成员函数模板接受所有兼容类型

- 请使用 member function templates (成员函数模板）生成"可接受所有兼容类型"的函数。
- 如果你声明 member templates 用于"泛化 copy 构造"或"泛化 assignment操作" 你还是需要声明正常的copy构造函数和 copy assignment 操作符。

## 条款 46: 需要类型转换时请为模板定义非成员函数

当我们编写一个 class template，而它所提供之"与此 template 相关的"函数支持"所有参数之隐式类型转换"时，请将那些函数定义为" class template 内部的friend 函数"。

## 条款 47: 请使用 traits classes 表现类型信息（个人感觉有点像反射）

- Traits classes 使得"类型相关信息"在编译期可用。它们以templates 和 "templates 特化"完成实现。
- 整合重载技术(overloading) 后， traits classes 有可能在编译期对类型执行 if...else 测试。

## 条款 48: 认识 template 元编程

- Template metaprogramming (TMP，模板元编程)可将工作由运行期移往编译期， 因而得以实现早期错误侦测和更高的执行效率。
- TMP 可被用来生成"基于政策选择组合" (based on combinations of policy choices) 的客户定制代码，也可用来避免生成对某些特殊类型并不适合的代码。

## 条款 49:了解new-handler的行为

## 条款 50: 了解 new 和 delete 的合理替换时机

## 条款 51: 编写 new delete 时需固守常规

## 条款 52: 写了 placement new 也要写 placement delete
