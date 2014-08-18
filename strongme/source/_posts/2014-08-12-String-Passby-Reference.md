title: Java中的String对象是“引用”传递的
date: 2014-08-12 22:30:40
tags:
- Java
- Java-Basics
- Translation
- String
categories:
- Translation
---

这个问题在Java的开发过程中是个很经典的问题。在Stackoverflow上也出现过许多相似的问题，同时里面也充斥这许多不正确或者是不完整的答案。如果你不去想太深入的话，其实这个问题还是挺简单的。但是你稍微的深深一琢磨，它就会把你给搞糊涂了。   

## 1. 一段有趣&迷糊人的代码   

``` java
public static void main(String[] args) {
	String x = new String("ab");
	change(x);
	System.out.println(x);
}
public static void change(String x) {
	x = "cd";
}
``` 

打印结果是“ab”   
<!--more-->
在C++编程中，是这样的：   
``` java
void change(string &x) {
    x = "cd";
}
 
int main(){
    string x = "ab";
    change(x);
    cout << x << endl;
}
```   

打印结果是“cd”   

<!--more-->

## 2. 常见的迷惑问题   

x变量存储的是指向内存堆中“ab”字符串对象的引用，当x作为变量传递给change()方法时，它仍然是指向内存中“ab”这个字符串的：   

<img  src="http://www.programcreek.com/wp-content/uploads/2013/09/string-pass-by-reference--650x247.jpeg">   

因为Java是值传递的，x的值就是一个指向“ab”字符串的引用。当触发change()方法的时候，它就会新创建一个“cd”字符串，然后x引用就会如下图一样指向新的String对象：   

<img  src="http://www.programcreek.com/wp-content/uploads/2013/09/string-pass-by-reference-2-650x247.jpeg">     

看图图说话，貌似是个很有说服性的图解，我们非常清楚Java的时间是值传递的，那这里到底发生什么了呢？   

## 3. 一开始的那段代码究竟做了什么？   

上面的图解实际上有几个错误。要理解起来也是相当容易的，完整的走一边处理过程是个好主意。   

当字符串对象“ab”被创建的时候，Java便会请求分配相应大小内存来存储这个字符串对象。然后这个对象就被赋值给了x变量，实际上x得到的值只是一个指向“ab”字符串的引用，这个引用便是字符串在内存中的地址。   

x变量存储着一个 指向字符串对象的引用，x变量本身却不是一个引用，它仅仅是个存储着引用（内存地址）的变量。   

铭记，Java永远是值传递的。当x变量被传递给change()方法时候，实际传递的是x变量的一个拷贝，change()方法里面创建的“cd”字符串有另外的引用地址，** 这里仅仅是将x变量的引用指向了另外一个字符串的引用，而不是把之前的那个引用指向了另外一个对象！**   


下面这张图解密到底发生了什么：   

<img  src="http://www.programcreek.com/wp-content/uploads/2013/09/string-pass-by-reference-3-650x244.jpeg">   

## 4.  错误的解释   

文头第一段代码所引发的问题跟String类型的不可变性没有任何关系。即时是把String对象替换成StringBuilder也是一样的结果。这里问题的关键是：变量不是引用，它仅仅是存储着引用。（The key point is that variable stores the reference, but is not the reference itself!这句原文有点绕，也不知道理解对没，请大神们指教）   

## 5. 问题解决办法   

如果你必须得改变一个对象的值，首先这个对象得是可变的，像StringBuilder。其次，得确保没有新new的对象出现赋值给参数变量，记住Java是值传递的。   

``` java
public static void main(String[] args) {
	StringBuilder x = new StringBuilder("ab");
	change(x);
	System.out.println(x);
}
 
public static void change(StringBuilder x) {
	x.delete(0, 2).append("cd");
}
```  

[外文连接：String is passed by “reference” in Java](http://www.programcreek.com/2013/09/string-is-passed-by-reference-in-java/)




