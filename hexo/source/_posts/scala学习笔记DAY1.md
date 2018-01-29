---
title: Scala学习笔记DAY1
date: 2017-06-01 15:40:25
tags: Scala
categories: Scala
---

Scala实际使用的是java虚拟机进行操作。他将输入迅速转为字节码，交给JVM执行，然后将执行结果显示在命令行界面，然后继续等待输入。

## Scala声明值和变量
使用val定义一个常量，无法改变这个常量的值，无法通过再次赋值改变；需要注意的是res变量都是val的。

![](/images/Day1/1.png)

使用var定义一个变量，但是不能改变其类型。需要注意的是，虽然在初始化的时候不需要给出值或者常量的类型，因为这个信息可以从表达式中推测出来，但是必须初始化，不初始化会报错。

![](/images/Day1/2.png)

在需要的时候，可以指定类型。

![](/images/Day1/3.png)

在Scala中，可以像java那样同时申明多个变量；同行中出现多条语句时，才需要使用分号。

![](/images/Day1/3.png)

## Scala的基本类型和操作

Scala中没有基本类型和引用类。Scala中有java拥有的八种类型Byte,Short, Char,Int, Long,Float,Double和Boolean。
Scala使用StringOps类给字符串String追加一些方法，如“Hello”.intersect("World")方法中，首先将“Hello”转换为StringOps类，然后才能使用intersect()方法得到两个字符串的交集；
Scala使用RichInt，RichDouble,RichChar丰富java中对应类的更多方法，都可以隐式转换。还可以使用BigInt和BigDecimal用于任意大小的数字。用户可以使用类似基本类型的数学操作符来操作这些“大数”，而在java中，这是不行的。

![](/images/Day1/4.png)

![](/images/Day1/5.png)

需要注意的是:

1. 在Scala中，不适用强制类型转换，而是使用方法来转换类型，如x1.toDouble(学习感觉：Scala有点奇怪啊，说是方法，但是这个转换后面又不能加java方法必须带的括号。加了就错，好不规范；补充：书籍后面说，不带参数且不改变当前对象的Scala方法通常不适用圆括号) ![](/images/Day1/6.png)

2. Scala中没有++和--操作；必须使用+=1或者是-=1
Scala神奇的理解能力
对于方法调用，可以使用  a.method(b)或者a method b两种表达

![](/images/Day1/7.png)

![](/images/Day1/8.png)

## 调用函数和方法
Scala提供数学函数，java需要使用math的静态方法实现。但是需要导入scala.math._包，其中_是通配符，类似java中的"*"。

![](/images/Day1/9.png)

![](/images/Day1/10.png)

Scala中没有静态方法，但是有一个类似的特性叫单例对象(singleton object)。通常一个类有一个伴生对象(companion object),其方法和java中的静态方法一样。使用起来和java的静态方法一样一样的.
比较：单例对使用object ASpecialClass{...}创建，这个对象类似于java的静态类，他的所有成员和方法都是静态的。
Scala伴生对象有一个伴生类，即class ASpecialClass{....}， 这个类可以通过对象名访问到所有的成员
object对象为静态常量、静态变量区域，可以直接调用，共享全局变量很有意义，伴生对象方便类的构建，可做为当前类的静态方法、成员的集合。 
[http://www.cnblogs.com/nethk/p/5609320.html](http://www.cnblogs.com/nethk/p/5609320.html)

![](/images/Day1/11.png)

Scala中使用伴生对象的apply方法构建对象是一种常用的手段。apply方法也可以省略！

![](/images/Day1/12.png)

BigInt的apply方法可以不需要使用new来生成一个新的BigInt对象

![](/images/Day1/13.png)

使用[http://www.scala-lang.org/api/current/](http://www.scala-lang.org/api/current/)这个在线的Scaladoc来帮助自己理解Scala的使用细节；
课后题答案：

1. 没有结果啊！！
2. ![](/images/Day1/14.png)
3. 是val，不能改变它的值。
4. “crazy”*3得到的“crazycrazycrazy”
5. 10 max 2是比较两个数的大小，返回较大的数值
6. ![](/images/Day1/15.png)
7. 需要引入scala.util._  sacla.math.BigInt._两个包
8. ![](/images/Day1/16.png)
9. 获得字符串的首字母和最后一个字母,"hello".take(1)或者是“hello”(0),"hello".takeRight(1)或者是"hello"reverse(0)或者是res45(res.length-1) ![](/images/Day1/17.png)
10. take(index)是（第一个字符下标为0）得到第index个字符；drop(index)丢弃第index个字符后得到新的字符串结果，原始字符串不改变