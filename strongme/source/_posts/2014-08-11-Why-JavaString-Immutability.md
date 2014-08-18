title: Java中的String对象为什么是不可变的
date: 2014-08-11 07:00:40
tags:
- Java
- Translation
- String
- Java-Basics
categories:
- Translation
---

String类型在Java中是不可变类型。不可变类型简单的理解就是它实例化之后的对象是不可修改的。所有的信息在这个对象创建的时候就被初始化好了，并且是不可以修改的。类的不可变性还是有许多的优点的。这篇文章咱就来唠嗑唠嗑为什么Java中的String类型被设计成了不可变的。对于这个问题完美的解释还得靠对存、同步、数据结构等的深刻认识。   

## 1. 对字符串池的需求   

字符串池（字符串保留池）是方法区中的一个特殊存储区域。当一个字符串被创建并且这个字符串在字符串池中存在时，当前存在的字符串引用就会被返回并指向它，否则的话，就创建一个新的String对象并且返回它的引用。   

下面的这段示例代码只会在堆内存中创建一个String对象。   

``` java
String string1 = "abcd";
String string2 = "abcd";
```   

在内存中是这个样子的：   

<img  src="http://www.programcreek.com/wp-content/uploads/2013/07/java-string-pool.jpeg">    

那咱假如String类型是可变的，那么通过其中一个引用来改变它所指向的String对象，将会导致另一个指向这个String对象的引用指向错误的字符串对象。   

<!--more-->
## 2. 缓存哈希码   

在Java中，字符串的哈希值是经常使用的。例如，在HashMap中就用到了字符串的哈希值。String类型的不可变性，保证了每次对于同样的字符串获取到的哈希值都是相同的，所以咱就可以对哈希值进行缓存而不必去担心下次得到另外不一样的值。同时也意味着不需要每次需要使用哈希值的时候再去计算它，使用缓存起来的更加高效。   

在String的源码中，有这么一小片代码：   

``` java
private int hash;//this is used to cache hash code .
```   

## 3. 方便了其他对象的使用   

为了容易理解一点，咱看下面这个程序：   

``` java
HashSet<String> set = new HashSet<String>();
set.add(new String("a"));
set.add(new String("b"));
set.add(new String("c"));

for(String a : set) {
	a.value = "a";
}
```   

在上面这个例子中，如果Stirng对象是可变的，那么HashSet中的值是可变的这个现象就是违背了Set类型设计的原则（Set存储非重复值）。这个例子只是为了演示，在真正的使用中，Set中的元素是没有value这个属性的。   

## 4. 安全性   

String对象在Java的许多类型中被当作参数来传递使用，例如：网络连接、文件操作等等。如果String是可变的，连接或者文件就 会改变，最后可能 导致非常严重的威胁。尽管这其中的方法不只在一台机器上调用。可变的字符串类型同意会在反射中产生安全问题，因为它使用的参数也是字符串类型的。   

下面是一段示例代码：   

``` java
boolean connect(string s){
    if (!isSecure(s)) { 
	throw new SecurityException(); 
    }
//here will cause problem, if s is changed before this by using other references.    
    causeProblem(s);
}
```   

## 5. 对象的不可变性造就了天然的线程安全性   

由于对象的不可变性，使得它可以在不同的线程之间自由的进行共享操作，这就终结了必须使用同步锁的必要性。   



##   总结
咳咳，该总结总结了，将String设计成不可变的初衷是为了效率和安全。这就解释了为什么在一般使用过程中不可变对象更加受欢迎。   


[外文连接：Why String is immutable in Java ?](http://www.programcreek.com/2013/04/why-string-is-immutable-in-java/)

