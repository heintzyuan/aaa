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
class ByteDemo
{
public static void main(String[] args) {
        byte a=5;
        byte b=6;
        byte c=7;
        a=b+c;
        System.out.println(b);
    }
}
```
编译时报错



