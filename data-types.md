# 数据类型  

Lua 是动态类型编程语言，变量没有类型，只有值才有类型。值可以存储在变量中，作为参数传递或者作为返回值。  
尽管在 Lua 中没有变量数据类型，但是值是有类型的。下面的列表中列出了数据类型： 
 
<table>
	<tr>
		<th>值类型</th>
		<th>描述</th>
	</tr>
	<tr>
		<td>nil</td>
		<td>用于区分值是否有数据，nil 表示没有数据。</td>
	</tr>
	<tr>
		<td>boolean</td>
		<td>布尔值，有真假两个值，一般用于条件检查。</td>
	</tr>
	<tr>
		<td>number</td>
		<td>数值，表示实数(双精度浮点数)。</td>
	</tr>
	<tr>
		<td>string</td>
		<td>字符串。</td>
	</tr>
	<tr>
		<td>function</td>
		<td>函数，表示由 C 或者 Lua 写的方法。</td>
	</tr>
	<tr>
		<td>userdata</td>
		<td>表示任意 C 数据。</td>
	</tr>
	<tr>
		<td>thread</td>
		<td>线程，表示独立执行的线程，它被用来实现协程。</td>
	</tr>
	<tr>
		<td>table</td>
		<td>表，表示一般的数组，符号表，集合，记录，图，树等等，它还可以实现关联数组。它可以存储除了 nil 外的任何值。</td>
	</tr>
</table> 
 
## type 函数  

Lua　中有一个 type　函数，它可以让我们知道变量的类型。下面的代码中给出了一些例子：　　

```
print(type("What is my type"))   --> string
t=10
print(type(5.8*t))               --> number
print(type(true))                --> boolean
print(type(print))               --> function
print(type(type))                --> function
print(type(nil))                 --> nil
print(type(type(ABC)))           --> string
```  

在 Linux 系统中运行上面的代码可以得到如下的结果：  

```
string
number
function
function
boolean
nil
string
```  

默认情况下，在被初始化或赋值前，所有变量都指向 nil。 Lua 中空字符串和零在条件检查时，都被当作真。所以你在使用布尔运算的时候要特别注意。在下一章中，我们会了解到更多关于这些类型的知识。
