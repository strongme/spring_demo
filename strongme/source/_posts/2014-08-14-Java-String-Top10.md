title: Top10 - Java中关于String类型的10个问题
date: 2014-08-14 21:15:20
tags:
- Java
- Java-Basics
- Translation
- String
categories:
- Translation
---

## 1. 如何比较两个字符串？用“=”还是equals简单来说，“==”是用来检测俩引用是不是指向内存中的同一个对象，而equals()方法则检测的是两个对象的值是否相等。只要你项检测俩字符串是不是相等的，你就必须得用equals()方法。   

如果你自己到“字符串保留(string intern)”的概念那就更好了。   

## 2. 为什么安全敏感的字符串信息用char[]会比String对象更好？   

String对象是不可变的就意味着直到垃圾回收器过来清扫之前它们都不会发生变化的。用数组的话，就可以很明确的修改它任何位置的字符元素。这样的话，如密码等安全敏感的信息就不会出现在系统的任何地方。  
<!--more-->
## 3. 字符串对象能否用在switch表达式中？   

从JDK7开始的话，我们就可以在switch条件表达式中使用字符串了，也就是说7之前的版本是不可以的。 

``` java
// java 7 only!
switch (str.toLowerCase()) {
      case "a":
           value = 1;
           break;
      case "b":
           value = 2;
           break;
}
``` 



## 4. 如何将字符串转换为整型数值？   

``` java
int n = Integer.parseInt("10");
```  

如此简单，经常使用有偶尔也会被遗忘。   

## 5. 如何用空格去分隔字符串？

我们可以很便捷的使用正则表达式来进行分隔。“\s”就表示空格，还有如"","\t","\r","\n".

``` java
String[] strArray = aString.split("\\s+");
``` 

## 6. substring()方法具体是都干了些啥？   

在JDK6中，这个方法只会在标识现有字符串的字符数组上 给一个窗口来表示结果字符串，但是不会创建一个新的字符串对象。如果需要创建个新字符串对象，可以这样在结果后面+一个空的字符串：   

``` java
str.substring(m, n) + ""
``` 

这么写的话就会创建一个新的字符数组来表示结果字符串。同时，这么写也有一定的几率让你的代码跑的更快，因为垃圾回收器会吧没有在使用的大字符串回收而留下子字符串。   

Oracle JDK7中的substring()方法会创建一个新的字符数组，而不用之前存在的。看看[这张图](http://www.programcreek.com/2013/09/the-substring-method-in-jdk-6-and-jdk-7/)就会明白substring()方法在JDK6和JDK7中的区别。     

## 7. String&StringBuilder&StringBuffer  


String vs StringBuilder:StringBuilder是可变的，这就意味你在创建对象之后还可以去修改它的值。StringBuilder vs StringBuffer:StringBuffer是同步的，意味着它是线程安全的，但是就会比StringBuilder慢些。   

## 8. 如何快速重复构造一段字符串？   

在Python编程中，只需要用字符串去乘以一个数字就可以 搞定了，那在Java编程中，我们可以使用来自Apache Commons Lang包中的StringUtils类的repeat()方法。   

``` java
String str = "abcd";
String repeated = StringUtils.repeat(str,3);
//abcdabcdabcd
``` 

## 9. 如何将时间格式的字符串转换成date对象？   

``` java
String str = "Sep 17, 2013";
Date date = new SimpleDateFormat("MMMM d, yy", Locale.ENGLISH).parse(str);
System.out.println(date);
//Tue Sep 17 00:00:00 EDT 2013
``` 

## 10. 如何计数一个字符在某个字符串中出现的次数？   

使用Apache Commons Lang包中的 StringUtils类就可以完成这个工作。   

``` java
int n = StringUtils.countMatches("11112222", "1");
System.out.println(n);
```  

## 对，还有一个   

[你知道如何检测一个字符串只包含大写字母呢？](http://www.programcreek.com/2011/04/a-method-to-detect-if-string-contains-1-uppercase-letter-in-java/)   

[外文连接：Top 10 questions of Java Strings](http://www.programcreek.com/2013/09/top-10-faqs-of-java-strings/)   






