title: 图表展示Java中String类型的不可变性
date: 2014-08-11 06:15:20
tags:
- Java
- Translation
- String
- Java-Basics
categories:
- Translation
---

来先看下面的一组图演示Java中String类型的不可变性。   


## 1. 声明一个字符串对象   

``` java
String s = "abcd";
```

这个变量s存储的是这个String对象的引用。下面图中的箭头应该被解释成“存着谁谁谁的引用”。   

<img src="http://www.programcreek.com/wp-content/uploads/2009/02/String-Immutability-1.jpeg">   

<!--more-->
## 2. 将先前创建的那个s String对象赋值给另一个变量s2   

``` java
String s2 = s;
```

s2存着跟s一样的引用，因此是指向一样的String对象。   

<img src="http://www.programcreek.com/wp-content/uploads/2009/02/String-Immutability-2.jpeg" >     




## 3. 字符串连接   

``` java
s = s.concat("ref");
``` 

引用s现在存储着新创建的String对象的引用。   

<img src="http://www.programcreek.com/wp-content/uploads/2009/02/string-immutability-650x279.jpeg" >    


## 总结   

一旦一个Java 的String对象在内存（堆）中创建，就不可能再发生变化了。大家应该注意到String的所有方法都不会改变这个String对象本身，而是返回一个新的String对象。   

如果需要一个可以修改的字符串，可以用StringBuilder或者是StringBuffer来实现。否则每次都创建一个String对象的话，程序运行的时候就会有大把大把的时间浪费在垃圾回收上面。[这](http://www.programcreek.com/2011/11/java-convert-a-file-into-a-string/)有 个例子示范如何使用StringBuilder的，有需要的可以去看看。

[外文链接:Diagram to show Java String’s Immutability](http://www.programcreek.com/2009/02/diagram-to-show-java-strings-immutability/)
