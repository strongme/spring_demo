title: 如何编写自己的NPM Package
date: 2014-08-16 09:04:45
tags:
- tutorial
- node.js
- npm
- package
categories:
- Tutor
---
不写自己的文，之后再开始写还是真的很难起笔，都不知道开如何开始的{% emoji worried %} 。
今天鼓捣HEXO Plugin的时候，想起了之前自己也写过npm Package，结果install之后发现是个只有个package.json文件的空壳子，正好学学，那就参考网上的例子开始吧！    

整个的基本流程大概是分两部分：   
* 编写一个简单的NPM Package、发布、然后下载安装测试
* 升级Package，为其添加依赖项   

<!-- more -->
Let's go   

## 首先简单的编写一个NodeJs程序
要编写的这个程序呢，就是将指定文件中的所有字母给它整成小写的。   
首先创建目录test以及一个js文件lowerme.js

	注意：这里得提示下，在创建Package之前最好前往NPM（https://www.npmjs.org）先搜索下，看是否已经存在，不然之后修改名称倒是简单，但是麻烦啊！


``` javascript
//lowerme.js
"use strict"

var fs = require('fs');
var fileName = 'test.txt';

if(fs.existsSync(fileName)) {
	var content = fs.readFileSync(fileName,'utf-8');
	fs.writeFileSync(fileName,content.toLowerCase());
	console.log('Done,all letters are in lowercase now!');
}else {
	console.log('File not found!');
}
```   

上面这段代码就是将给定文件中的所有字母都给它转换成小写的。   
在命令提示符中执行如下命令，test.txt文件需要放在与js文件同级目录下：   

``` bash
node lowerme
```

结果应该是test.txt文件中的所有字母都是小写的了。   

### 修改程序，从命令中输入要转换的文件名   

将文件名写死在代码里面实在是不优雅啊，咱稍微修改下，让它从命令行来读取文件名字，这时我们将使用process.argv这个数组来传递变量：   

```
0: node
1: <在执行的js文件的名称>
2: 执行时另外敲入的参数
```   

修改代码后如下：   

``` javascript
//lowerme.js
"use strict"

var fs = require('fs');
if(process.argv.length>2) {
	//读取传入的文件名称
	var fileName = process.argv[2];
	if(fs.existsSync(fileName)) {
		var content = fs.readFileSync(fileName,'utf-8');
		fs.writeFileSync(fileName,content.toLowerCase());
		console.log('Done,all letters are in lowercase now!');
	}else {
		console.log('File not found!');
	}
}

```   

那现在的话我们就可以从命令行来指定要转换的文件名了，如果未输入文件名称的话便会提示'File not found!'。   

同样执行先前的命令：   
 ``` bash
 node lowerme test.txt
 ```   

 ## 开始编写Node模块   

 现在我们已经有了就一个转换文件内容为小写的小程序了，那么接下来就需要去编写一个可以发布到NPM的包，以让别个开发人员下载使用。   

 具体有如下几个：   

 * 通过NPM来安装这个小程序
 ``` bash
 npm install lowerme
 ```
 * 通过命令提示符来使用这个功能 
 ``` bash
 lowerme <filename>
 ```
 * 在其他NodeJs程序中使用这个功能
 ``` javascript
 require('lowerme');
 ```

 那就开始编写这个包吧（NPM：Node Package Manager）。   

 开始之前，需要修改test目录里面的目录结构，需要建一些文件和目录来构造如下结构的目录：  
 ```
 test
 	src
 		bin
 			-- lowerme
 		lib
 			-- lowerme.js
 		-- package.json
 		-- README.md
 	test.txt

 ```   

 将我们之前创建的lower.js文件移动到lib目录下，test.txt文件位置不变。   

 src目录中的结构就是为NPM包需要而准备的：   
 * package.json：里面的信息就是这个NPM包的配置信息，这是唯一一个必须存在的文件，并且内容有特定要求。至于其他文件就可以按照自己口味来弄了。   
 * README.md：这个文件是可选的，不过要是没有的话NPM命令会警告这个文件不存在。   

 Markdown的格式描述看这里：[markdown format](http://en.wikipedia.org/wiki/Markdown)   

 移动过一些文件之后再测试下之前的功能是不是正常的：   
 ``` bash
 node ./src/lib/lower.js ./test.txt
 ```   

 应该是跟之前一样的，如果不是的话，就检查下目录是不是没写对。   
 接下来在./src/bin目录下创建文件lowerme，注意是没有后缀js的，文件内容如下：    

 ``` bash
 #!/usr/bin/env node

"use strict";
var path = require('path');
var fs = require('fs');
var lib = path.join(path.dirname(fs.realpathSync(__filename)),'../lib');

require(lib+'/lowerme.js').convert();
 ```   
 上面的这个文件就是设置让这个功能可以直接在命令行使用的，之后还需要在package.json文件中进行设置。   

 现在可以先测试下这个文件：   
 ``` bash
 node ./src/bin/lowerme ./test.txt
 ```   
 这家伙，报错了：   
``` bash
module.js:340
    throw err;
          ^
Error: Cannot find module 'D:\NODEJS\uppercaseme\test\src\liblowerme.js'
    at Function.Module._resolveFilename (module.js:338:15)
    at Function.Module._load (module.js:280:25)
    at Module.require (module.js:364:17)
    at require (module.js:380:17)
    at Object.<anonymous> (D:\NODEJS\uppercaseme\test\src\bin\lowerme:8:1)
    at Module._compile (module.js:456:26)
    at Object.Module._extensions..js (module.js:474:10)
    at Module.load (module.js:356:32)
    at Function.Module._load (module.js:312:12)
    at Function.Module.runMain (module.js:497:10)
```   

这是由于我们还没有把之前写好的小程序转换成Node 模块的格式，同样也没有convert方法。先改下：   

``` javascript
//lowerme.js
"use strict"

var fs = require('fs');

function convertStr() {
	if(process.argv.length>2) {

		var fileName = process.argv[2];
		if(fs.existsSync(fileName)) {
			var content = fs.readFileSync(fileName,'utf-8');
			fs.writeFileSync(fileName,content.toLowerCase());
			console.log('Done,all letters are in lowercase now!');
		}else {
			console.log('File not found!');
		}
	}
}

exports.convert = convertStr;

```   

改写之后，就会把convertStr方法从lowerme模块里面暴露出这个接口：convert   

OK，再试试：   

``` bash
D:\NODEJS\uppercaseme\test>node ./src/bin/lowerme ./test.txt
Done,all letters are in lowercase now!
```
那么，现在就可以开始“打包”这个功能啦！


## 创建NPM包   

创建src/package.json文件和src/README.md文件   

package.json文件的[格式](http://en.wikipedia.org/wiki/JSON)如下：   

``` json
{
	"author": "Strong Walter",
	"name": "lowerme",
	"description": "COnverts files to lowercase",
	"version": "0.0.1",
	"repository": {
		"url": ""
	},
	"main": "./lib/lowerme",
	"keywords": [
		"lower",
		"case",
		"file"
	],
	"bin": {
		"lowerme": "./bin/lowerme"
	},
	"dependencies": {},
	"engines": {
		"node": "*"
	}
}
```   

上面json文件中的一些字段，大家可以设置成自己需要的。   
这些字段都是一看就明白什么意思，我们简单介绍几个：   

* main 是程序模块的入口ID。在这个例子中，包名叫lowerme。用户安装之后，调用require('lowerme')将会返回主模块的方法对象。  
* bin 安装这个软件之后，我们可能需要在任何地方打开命令行来调用，因此就需要将其加入到环境变量中。
* dependices 这里会列出模块所需要的其他功能库，不过这个例子不需要依赖其他库。

更多字段说明看[NPM-JS Documentation](https://npmjs.org/doc/json.html)   

顺便闯将README.md文件：   

```
upper-case-me
-------------

This is a cool program to lower-case files
```
OK，一切就绪，现在可以发布到NPM了。   

首先执行命令注册添加NPM账户信息到本地：   
``` bash
npm adduser
username:
password:
email:
```   
输入之后便会注册一个帐号。

进入src目录：   
``` bash
npm publish
```   
打印如下：   
```
D:\NODEJS\uppercaseme\test\src>npm publish
+ lowerme@0.0.1
``` 
这里应该是打印许多信息的，但是我升级NPM之后便没有那么多日志信息了。   

上传成功，具体信息看[这里](https://www.npmjs.org/package/lowerme)。   

测试下：   
``` bash
D:\NODEJS\lowerme\test\src>npm install -g lowerme
C:\Users\*****\AppData\Roaming\npm\lowerme -> C:\Users\walter\AppData\Roaming\npm\node_modules\lowerme\bin\lowerme
lowerme@0.0.2 C:\Users\walter\AppData\Roaming\npm\node_modules\lowerme

D:\NODEJS\uppercaseme\test>lowerme ./myfile.txt
Done,all letters are in lowercase now!
```   

OK，可以安装使用，这个例子先写到这，下个加一些依赖进去试试。   




















