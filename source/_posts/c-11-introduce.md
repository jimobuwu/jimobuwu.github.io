---
title: c++11特性简介
date: 2016-12-04 01:10:03
tags:
categories: C++11
---
#### 右值引用&& 移动语义（std::move）
C++在用临时对象或函数返回值给左值对象赋值时的深度拷贝，会影响执行效率。
考虑到临时对象的生命期仅在表达式中持续，如果把临时对象的内容直接移动（move）给被赋值的左值对象，效率改善将是显著的。

总结：消除两个对象交互时不必要的对象拷贝，节省运算存储资源，提高效率。
<!--more-->
#### 初始化列表initializer_list
一个initializer_list当出现在以下两种情况的被自动构造：

- 当初始化的时候使用的是大括号初始化，被自动构造。包括函数调用时和赋值
- 当涉及到for（initializer： list）,list被自动构造成initializer_list对象

也就是说initializer_list对象只能用大括号{}初始化。
拷贝一个initializer_list对象并不会拷贝里面的元素。其实只是引用而已。而且里面的元素全部都是const的。
**方便了STL中容器的初始化。**

#### emplace_back减少内存拷贝和移动
emplace_back能就地通过参数构造对象，不需要拷贝或者移动内存，相比push_back能更好地避免内存的拷贝与移动，使容器插入元素的性能得到进一步提升。

#### lambda表达式
lambda 表达式可以方便地构造匿名函数
C++11 的 lambda 表达式规范如下：
        \[ capture ] ( params ) mutable exception attribute -> ret { body }(1) 
        \[ capture ] ( params ) -> ret { body }(2) 
        \[ capture ] ( params ) { body }(3) 
        \[ capture ] { body }(4) 
其中,
	* (1) 是完整的 lambda 表达式形式，
	* (2) const 类型的 lambda 表达式，该类型的表达式不能改捕获("capture")列表中的值。
	* (3)省略了返回值类型的 lambda 表达式，但是该 lambda 表达式的返回类型可以按照下列规则推演出来：
	* 
		* 如果 lambda 代码块中包含了 return 语句，则该 lambda 表达式的返回类型由 return 语句的返回类型确定。
		* 如果没有 return 语句，则类似 void f(...) 函数。

	* 省略了参数列表，类似于无参函数 f()。


**capture指定了在可见域范围内 lambda 表达式的代码内可见得外部变量的列表，具体解释如下**
	* [] 什么也没有捕获
	* [a, &b] 按值捕获a，按引用捕获b
	* [&] 按引用捕获任何用到的外部变量
	* [=] 按值捕获任何用到的外部变量
	* [a, &] 按值捕获a, 其它的变量按引用捕获
	* [&b, =] 按引用捕获b,其它的变量按值捕获
	* [this] 按值捕获this指针

#### 强类型枚举（枚举类）
普通枚举的缺点：
- 枚举类型不是类型安全的。两种不同的枚举类型之间可以进行比较。
- 两个不同的枚举，不可以有相同的枚举名。

		enum class Enumeration
		{
		Val1,
		Val2,
		Val3 = 100,
		Val4 /* = 101 */,
		};

此种枚举为类型安全的。枚举类型不能隐式地转换为整数；也无法与整数数值做比较。
枚举类型的语汇范围(scoping)定义于枚举类型的名称范围中。为了兼容性，也可以在一般范围内使用。

#### 线程



#### 智能指针
shared_ptr
unique_ptr