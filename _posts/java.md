---
layout:     post
title:      "Java常见问题（一）"
subtitle:   
date:       2016-09-27
author:     "Terry"
header-img: ""
catalog: Java
tags:
    - Java
---
#### 类型转换
先看下面一段代码：
```java   
        byte a=5;
        byte b=6;
        byte c=7;
        a=b+c;
        System.out.println(b);
```
编译时报错“java: 不兼容的类型: 从int转换到byte可能会有损失”。

Java中，byte类型占1个字节，int类型占4个字节。

整数常量的默认数据类型是int，当其赋值给byte变量时，系统会先检查其大小是否在byte的取值范围（-128-127）内，如果在范围内，则会自动进行类型转换，把常量的前3个字节删除；如果不在范围，编译时就会报错。

再看上面的代码，a=b+c中b和c都是变量，因此编译时系统不能确定b和c的和是否在byte内，可能会丢失精度，所以报错。而如果将变量初始化为int类型则不会有此问题。

#### 引用变量
先看下面一段代码：
```java   
public class Num{

int num =1;

public static void main(String[] args) {
        Num a = new Num();
        System.out.println("a="+a.num);
        Num b = a;
        b.num=2;
        System.out.println("a="+a.num);
    }
}
```
运行结果是：
```java
a=1,a=2
```
要注意的是，新建一个Num对象的时候，a是一个类类型的引用变量，储存了该类对象在堆内存中的地址，而Num b = a表示新建一个类类型的引用变量b，并将a储存的地址复制给b，所以此时a和b都指向同一个对象，a和b对对象的操作是等价的。

#### 不可变字符串与限定字符串
我们知道String并不是基本数据类型，而是一个类，JDK1.5以后的autoboxing特性，使得新建一个String对象变得更方便了：
```java   
//JDK1.5以前
String a = new String();
//JDK1.5以后
String a = "...";      
```
注意，第一种是用new关键字来生成对象，后一种属于直接声明，得到**字符串直接量**。

对于字符串直接量，JVM会在内存中开辟一个“String Pool”，用来存放字符串直接量。

String类的对象是不可变的，不能改变其内容。

看这一段代码：
```java
String a = "abc";
a = "bcd";
System.out.println(a);
```
输出
```java
bcd
```
这说明String对象的内容可以被改动？但实际情况是，a储存的引用被改变了，从“abc”指向了“bcd”所在的地址。

通过获取hashcode可以明显的看出，a储存的地址发生了变化：
```java
String a = "abc";
System.out.println(r.hashCode());
a = "bcd";
System.out.println(a);
System.out.println(r.hashCode());
```
输出
```java
96354
bcd
97347
```
a的指向被改变后，原来的对象“abc”并不会被清除，而是会一直存在于String Pool中，通过intern()方法,可以重新定位到“abc”对象。



```java   
String a = new String("abc");
String b = new String("abc");
String c = "abc";
String d = "abc";
String e = a.intern();
System.out.println(a==b);
System.out.println(c==d);
System.out.println(c==a);
System.out.println(c==b);
System.out.println(c==e);
```
编译结果为:
```java
false
true
false
false
true
```



