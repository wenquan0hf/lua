# 基本语法  

Lua 学起来非常简单。现在，让我们开始创建我们的第一个 Lua 程序吧！  

## 第一个 Lua 程序  

Lua 提供交互式编程模式。在这个模式下，你可以一条一条地输入命令，然后立即就可以得到结果。你可以在 shell 中使用 lua -i 或者 lua 命令启动。输入命令后，按下回车键，就启动了交互模式，显示如下：  

```
$ lua -i 
$ Lua 5.1.4  Copyright (C) 1994-2008 Lua.org, PUC-Rio
quit to end; cd, dir and edit also available
```  

你可以使用如下命令打印输出：  

```
$> print("test")
```  

按下回车键后，你会得到如下输出结果：  

```
'test'
```  

## 默认模式编辑  

使用 Lua 文件做为解释器的参数启动解释器,然后开始执行文件直到文件结束。当脚本执行结束后，解释器就不在活跃了。 
 
让我们写一个简单的 Lua 程序。所有的 Lua　文件都扩展名都是`.lua`。因此，将下面的源代码放到 test.lua 文件中。  

```
print("test")
```  

假如你已经设置好 Lua 程序的环境，用下面的命令运行程序：  

```
$ lua test.lua
```  

我们会得到如下的输出结果：  

```
test
```  

让我们尝试使用另外的方式运行 Lua 程序。下面是修改后的 test.lua 文件：  

```
\#!/usr/local/bin/lua
print("test")
```  

这里，我们假设你的 Lua 解释器程序在 /usr/local/bin/lua 目录下。test.lua 文件中第一行由于以 # 开始而被解释器忽略，运行这个程序可以得到如下的结果：  

```
$ chmod a+rx test.lua
$./test.lua
```  

我们会得到如下的的输出结果：  

```
test
```  

接下来让我们看一下 Lua 程序的基本结构。这样，你可以更容易理解 Lua 编程语言的基本结构单元。  

## Lua 中的符号  

Lua 程序是由大量的符号组成的。这些符号可以分为关键字、标识符、常量、字符串常量几类。例如，下面的 Lua 语句中包含三个符号：  

```
io.write("Hello world, from ",_VERSION,"!\n")
```  

这三个符号分别是:  

```
io.write
(
"Hello world, from ",_VERSION,"!\n"
)
```

### 注释  

注释就是 Lua 程序中的帮助文档，Lua 解释器会自动忽略它们。所有注释都以 --[[ 开始，并以 --]]结束。如下所示：  

```
--[[ my first program in Lua --]]
```  

### 标识符  

Lua 中标识符是识别变量、函数或者其它用户自定义项的名字。标符识总是以字母或者下划线开始，其后可以是零个或多个字母、下划线或数字。  
Lua 标识符中不允许出现任何标点符号，比如，@，$ 或者 %。Lua 是大小写敏感的语言，因此 Manpower 和 manpower 是 Lua 中两个不同的标识符。下面所列的是一些合法标识符的例子。  

```
mohd         zara      abc     move_name    a_123
myname50     _temp     j       a23b9        retVal
```  

### 关键字  

下面列表中所示的是 Lua 中一小部分保留字。这些保留字不能用作常量、变量以及任何标识符的名字。  

<table>
	<tr>
	<td>and</td>
	<td>break</td>
	<td>do</td>
	<td>else</td>
	</tr>
	<tr>
	<td>elseif</td>
	<td>end</td>
	<td>false</td>
	<td>for</td>
	</tr>
	<tr>
	<td>function</td>
	<td>if</td>
	<td>in</td>
	<td>local</td>
	</tr>
	<tr>
	<td>nil</td>
	<td>not</td>
	<td>or</td>
	<td>repeat</td>
	</tr>
	<tr>
	<td>return</td>
	<td>then</td>
	<td>true</td>
	<td>until</td>
	</tr>
	<tr>
	<td>while</td>
	<td></td>
	<td></td>
	<td></td>
	</tr>
</table>

### Lua 中的空白符  

如果 Lua 程序中某一行只包含空格或者注释，那么这样的一行被称之为空行。 Lua 解释器将完全忽略这一行。  
在 Lua 中，空白是用来描述空格、制表符、换行符和注释的术语。空白符用于将语句中的一部分与其它部分区分开，使得解释器可以语句中的一个元素，比如 int，何处结束，以及另一个元素从何处开始。因此，在下面的语句中：  

```
local age
```  

在 local 与 age 之间至少有一个空白符（通常是空格）,这个空白符使得解释器可以将 local 与 age 区分开。另一方面，在下面的语句中：  

```
fruit = apples + oranges   --get the total fruit
```  

fruit 与 = 之间以及 = 与 apples 之间的空白符都是可以没有的。但是为了程序的可读性目的，建议你在它们之间使用空白符。