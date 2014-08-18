title: 为NPM包添加依赖
date: 2014-08-17 15:16:57
tags:
- tutorial
- node.js
- npm
- package
- dependency
categories:
- Tutor
---

接着[上一篇](/2014-08-16-How-NPM-Package.html){% emoji bowtie %}.   

** 这次给自己的包加如依赖包。**   

需要给命令行lowerme添加自动创建file.txt文件的命令，通过--new选项即可完成。   

修改命令行选项设置如下：   
需要转换一个文件中的字母，则通过--file选项来完成。   

``` bash
lowerme --file file.txt
```   

在工作目录创建file.txt文件。   

``` bash
lowerme --new
```   
<!-- more -->

需要归档这些文件，就需要加入commander模块。   
``` bash
npm install commander -save
```
-save选项可以自动为我们的package.json文件添加依赖。   

注意没有-g，仅仅是安装到了当前的工作目录。   

## 修改NPM包设置来加入commander包   

这个不是介绍commander的例子，所以我们只是简单的解释一下我们的例子中需要的几个功能。   

首先包含依赖包require("commander")，然后设置版本，命令参数名称。这样我们就可以解析传入命令行的参数了。   

``` javascript
"use strict"
var fs = require('fs');
var path = require('path');

var program = require('commander');

program.version('0.1.1')
    .option('-n, --new', 'Create a test file')
    .option('--file [filename]', 'Filename to convert')
    .parse(process.argv);
```
然后我们自己的代码里面就可以通过program变量来获取输入的选项。   

``` bash
if(program.new) {
	...
	var myfile = program.file;
}
```   
Commander会自动的为我们的程序生成命令行帮助内容，需要查看的时候用--help选项即可。   

## 更新我们的代码   
修改src/lib/lowerme.js文件如下：   

``` javascript
//lowerme.js
"use strict"

var fs = require('fs');
var path = require('path');

var program = require('commander');

//定义config program
program.version('0.0.3')
	.option('-n, --new','Create a test file')
	.option('--file [filename]','Filename to convert')
	.parse(process.argv);

function copyFile(from,to) {
	return fs.createReadStream(from).pipe(fs.createWriteStream(to));
}

function convertStr() {

	if(program.new) {
		var newfile = path.join(path.dirname(fs.realpathSync(__filename)),'../lib/example/file.txt');
		copyFile(newfile,path.join(process.cwd(),'file.txt'));
		console.log('Check out the new file name\'s file.txt');
	}else if(program.file){
		var myfile = program.file;
		if(fs.existsSync(myfile)) {
			var content = fs.readFileSync(myfile,'utf8');
			fs.writeFileSync(myfile,content.toLowerCase());
			console.log('Done,all letters are in lowercase now!');
		}else {
			console.log('File does not exist - '+myfile);
			console.log('Or Create a new file with the --new option');
		}
	}else {
		console.log('Pass a file name/path');
	}
}

exports.convert = convertStr;
```   

测试一下   


``` bash
node ./src/bin/lower --new
```   

会在当前目录下生成示例文件   

``` bash
node ./src/bin/lowerme --file file.txt
```   

最后检查下package.json文件修改版本号，就可以发布更新啦。   

``` bash
npm publish
```   

发布成功{% emoji smirk %}.   

TESTEST{% emoji heart_eyes %}.   

``` bash
D:\NODEJS\uppercaseme\test2>npm install lowerme -g
C:\Users\walter\AppData\Roaming\npm\lowerme -> C:\Users\walter\AppData\Roaming\npm\node_modules\lowerme\bin\lowerme
lowerme@0.0.3 C:\Users\walter\AppData\Roaming\npm\node_modules\lowerme
└── commander@2.3.0
D:\NODEJS\uppercaseme\test2>lowerme
Pass a file name/path

D:\NODEJS\uppercaseme\test2>lowerme --new
Check out the new file name's file.txt

D:\NODEJS\uppercaseme\test2>lowerme --file file.txt
Done,all letters are in lowercase now!

D:\NODEJS\uppercaseme\test2>lowerme --help

  Usage: lowerme [options]

  Options:

    -h, --help         output usage information
    -V, --version      output the version number
    -n, --new          Create a test file
    --file [filename]  Filename to convert
```   

## NOTE：为了我们的DEMO不会污染NPM   

既然NPM为我们发布包提供了这么便捷的方式，我强烈建议大家在写DEMO例子之后卸载发布，不过也看自己啦{% emoji blush %}。   

 ``` bash
npm unpublish --force
 ```      

NPM同时也会在本地缓存之前下载过的一些包，可以通过以下命令来清空：   


 ``` bash
 npm cache clean
 ```   
OK，相当的有趣，完全可以用来自定义自己的工具嘛！

 Hooah{% emoji grinning %}   Day {% emoji one %} 5:00    




