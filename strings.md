# Lua 字符串  

字符串就是一个由字符或控制字符组成的序列。字符串可以用以下三种方式任意一种进行初始化。
  
<ul>
	<li>单引号字符串</li>
	<li>双引号字符串</li>
	<li>[[和]]之间的字符串</li>
</ul>  

上面三种初始化方式的示例如下：  

```
string1 = "Lua"
print("\"String 1 is\"",string1)
string2 = 'Tutorial'
print("String 2 is",string2)

string3 = [["Lua Tutorial"]]
print("String 3 is",string3)
```  

运行上面的程序，我们可以得到如下的输出结果：  

```
"String 1" is	Lua
String 2 is	Tutorial
String 3 is	"Lua Tutorial"
```  

字符串中转义字符用于改变字符的一般正常的解释。在上面的例子中，输出双引号（""）的时候，我们使用的是 \"。下表列出了转义序列及相应的使用方法：  

<table>
	<tr>
		<th>转义序列</th>
		<th>用法</th>
	</tr>
	<tr>
		<td>\a</td>
		<td>响铃</td>
	</tr>
	<tr>
		<td>\b</td>
		<td>退格</td>
	</tr>
	<tr>
		<td>\f</td>
		<td>换页</td>
	</tr>
	<tr>
		<td>\n</td>
		<td>换行</td>
	</tr>
	<tr>
		<td>\r</td>
		<td>回车</td>
	</tr>
	<tr>
		<td>\t</td>
		<td>制表符</td>
	</tr>
	<tr>
		<td>\v</td>
		<td>垂直制表符</td>
	</tr>
	<tr>
		<td>\\</td>
		<td>反斜线</td>
	</tr>
	<tr>
		<td>\"</td>
		<td>双引号</td>
	</tr>
	<tr>
		<td>\'</td>
		<td>单引号</td>
	</tr>
	<tr>
		<td>\[</td>
		<td>左方括号</td>
	</tr>
	<tr>
		<td>\]</td>
		<td>右方括号</td>
	</tr>
</table>

## 字符串操作  

Lua 支持如下的字符串操作方法：  

<table>
	<tr>
		<th>S.N.</th>
		<th>函数及其功能</th>
	</tr>
	<tr>
		<td>1</td>
		<td>string.upper(argument):将输入参数全部字符转换为大写并返回。</td>
	</tr>
	<tr>
		<td>2</td>
		<td>string.lower(argument):将输入参数全部字符转换为小写并返回。</td>
	</tr>
	<tr>
		<td>3</td>
		<td>string.gsub(maingString,findString,replaceString):将 mainString 中的所有 findString 用 replaceString 替换并返回结果。</td>
	</tr>
	<tr>
		<td>4</td>
		<td>string.strfind(mainString,findString,optionalStartIndex,optionalEndIndex):在主字符串中查找 findString 并返回 findString 在主字符串中的开始和结束位置，若查找失败则返回 nil。</td>
	</tr>
	<tr>
		<td>5</td>
		<td>string.reverse(arg):将输入字符串颠倒并返回。</td>
	</tr>
	<tr>
		<td>6</td>
		<td>string.format(...):返回格式化后的字符串。</td>
	</tr>
	<tr>
		<td>7</td>
		<td>string.char(arg) 和 string.byte(arg):前者返回输出参数的所代表的字符，后者返回输入参数（字符）的数值。</td>
	</tr>
	<tr>
		<td>8</td>
		<td>string.len(arg):返回输入字符串的长度。</td>
	</tr>
	<tr>
		<td>9</td>
		<td>string.rep(string,n): 将输入字符串 string 重复 n　次后的新字符串返回。</td>
	</tr>
	<tr>
		<td>10</td>
		<td>..:连接两个字符串。</td>
	</tr>
</table>

接下来我们用一些例子来讲解如何使用上面这些函数。  

## 大小写操作函数  

下面的代码将字符串中字符全部转换成大写或小写：  

```
string1 = "Lua";
print(string.upper(string1))
print(string.lower(string1))
```  

执行上面的代码可以得到如下的输出结果：  

```
LUA
lua
```  

## 替换子串  

用一个字符串替换字符串的某子串的示例代码如下：  

```
string = "Lua Tutorial"
-- 替换字符串
newstring = string.gsub(string,"Tutorial","Language")
print("The new string is",newstring)
```  

执行上面的代码可以得到如下的输出结果：  

```
The new string is	Lua Language
```  

## 查找与颠倒  

查找一个子串的索引与颠倒一个字符串函数的示例代码如下所示：  

```
string = "Lua Tutorial"
-- 替换字符串
print(string.find(string,"Tutorial"))
reversedString = string.reverse(string)
print("The new string is",reversedString)
```  

执行上面的代码可以得到如下的输出结果：  

```
5	12
The new string is	lairotuT auL
```  

## 格式化字符串  

在编程过程中，我们经常需要将字符串以某种格式输出。此时，你就可以使用 string.format 函数格式化你的输出内容。如下所示：  

```
string1 = "Lua"
string2 = "Tutorial"
number1 = 10
number2 = 20
-- 基本字符串格式
print(string.format("Basic formatting %s %s",string1,string2))
-- 日期格式化
date = 2; month = 1; year = 2014
print(string.format("Date formatting %02d/%02d/%03d", date, month, year))
-- 符点数格式化
print(string.format("%.4f",1/3))
```  

执行上面的代码可以得到如下的输出结果：  

```
Basic formatting Lua Tutorial
Date formatting 02/01/2014
0.3333
```  

## 字符与字节表示  

字节表示函数用于将字符的内部表示转换为字符表示，而字符表示函数正好相反。 示例代码如下：  

```
-- 字节转换
-- 第一个字符
print(string.byte("Lua"))
-- 第三个字符
print(string.byte("Lua",3))
-- 倒数第一个字符
print(string.byte("Lua",-1))
-- 第二个字符
print(string.byte("Lua",2))
-- 倒数第二个字符
print(string.byte("Lua",-2))

-- 内部 ASCII 字值转换为字符
print(string.char(97))
```  

执行上面的代码可以得到如下的输出结果：  

```
76
97
97
117
117
a
```  

## 其它常用函数  

其它常用的字符串处理函数包括字符串连接，字符串长度函数以及重复字符串多次的函数。它们的使用方法示例如下：  

```
string1 = "Lua"
string2 = "Tutorial"
-- 用 .. 连接两个字符串
print("Concatenated string",string1..string2)

-- 字符串的长度
print("Length of string1 is ",string.len(string1))

-- 重复字符串
repeatedString = string.rep(string1,3)
print(repeatedString)
```  

执行上面的代码可以得到如下的输出结果：  

```
Concatenated string	LuaTutorial
Length of string1 is 	3
LuaLuaLua
``` 