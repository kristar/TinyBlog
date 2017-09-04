title: Java格式化输出
date: 2017/5/16 21:35
comments: true
categories:
  - Java
tags:
  - Java
---
有一次在写Java后台时需要将一个整形例如1变成01的字符串

在String类中提供String.format
```java
public static String format(String format, Object... args)
```
使用指定的格式字符串和参数返回一个格式化字符串。
```java
int number = 1;
String str = String.format("%02d", number);
System.out.println(str); // 输出01
```
也可以使用Java.util.Formatter所提供的方法
