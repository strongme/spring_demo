title: 判断一个字符串中只包含大写字母的方法
date: 2014-08-16 03:20:05
tags:
- Java
- Java-Basics
- Translation
- String
categories:
- Translation
---

## 我的方法

下面这个办法是循环查看字符串中的每一个字符是否是大写字母。大写字母的值介于97~122。   

``` java
public static boolean testAllUpperCase(String str){
	for(int i=0; i<str.length(); i++){
		char c = str.charAt(i);
		if(c >= 97 && c <= 122) {
			return false;
		}
	}
	//str.charAt(index)
	return true;
}
```   

大家有没有考虑到性能方面的办法？   

[外文链接：A method to detect if string contains only uppercase letter in Java](http://www.programcreek.com/2011/04/a-method-to-detect-if-string-contains-1-uppercase-letter-in-java/)  


