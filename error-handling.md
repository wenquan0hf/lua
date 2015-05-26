# Lua 错误处理  

## 为什么需要错误处理机制

在真实的系统中程序往往非常复杂，它们经常涉及到文件操作、数据库事务操作或网络服务调用等，这个时候错误处理就显得非常重要。不关注错误处理可能在处理诸如涉密或金融交易这些业务时造成重大的损失。  
无论什么时候，程序开发都要求小心地做好错误处理工作。在 Lua 中错误可以被分为两类：  
<ul>
	<li>语法错误</li>
	<li>运行时错误</li>
</ul>

## 语法错误　　

语法错误是由于不正确的使用各种程序语法造成的，比如错误的使用操作符或表达式。下面即是一个语法错误的例子：  

```
a == 2
```  

正如你知道的那样，单个等号与双等号是完全不一样的。二者之间随意的替换就导致语法错误。一个等号表示的是赋值，而双等号表示比较。类似地，下面这一小段代码中也存在语法错误：  

```
for a= 1,10
   print(a)
end
```  

执行上面的这段程序，我们会得到如下的输出结果：  

```
lua: test2.lua:2: 'do' expected near 'print'
```  

语法错误相比于运行时错误更容易处理，因为 Lua 解释器可以更精确的定位到语法错误的位置。由上面的错误，我们可以容易就知道，在 print 语句前添加 do 语句就可以了，这是 Lua 语法结构所要求的。  

## 运行时错误  

对于运行时错误，虽然程序也能成功运行，但是程序运行过程中可能因为错误的输入或者错误的使用函数而导致运行过程中产生错误。下面的例子显示了运行时错误如何产生的：  

```
function add(a,b)
   return a+b
end

add(10)
```  

当我们尝试生成(build)上面的程序，程序可以正常的生成和运行。但是一旦运行后，立马出现下面的运行时错误。  

```
lua: test2.lua:2: attempt to perform arithmetic on local 'b' (a nil value)
stack traceback:
	test2.lua:2: in function 'add'
	test2.lua:5: in main chunk
	[C]: ?
```  

这个运行时错误是由于没有正确的为 add 函数传入参数导致的，由于没有为 b 传入值，所有 b 的值为 nil 从而导致在进行加法运算时出错。  

## Assert and Error 函数  

我们经常用到 assert 和 error 两个函数处理错误。下面是一个简单的例子。  

```
local function add(a,b)
   assert(type(a) == "number", "a is not a number")
   assert(type(b) == "number", "b is not a number")
   return a+b
end
add(10)
```  

执行上面的程序，我们会得到如下的输出结果：  

```
lua: test2.lua:3: b is not a number
stack traceback:
	[C]: in function 'assert'
	test2.lua:3: in function 'add'
	test2.lua:6: in main chunk
	[C]: ?
```  

error(message [,level]) 函数会结束调用自己的函数，并将 message 作为错误信息返回调用者(译注:保护模式下才会返回调用者，一般情况会结束程序运行并在控制终端输出错误信息)。error 函数本身从不返回。一般地，error 函数会在消息前附上错误位置信息。级别(level) 参数指定错误发生的位置。若其值为 1(默认值)，返回的错误的位置是 error 函数被调用的位置。若为 2, 返回的错误位置为调用 error 函数的函数被调用的位置，依次类推。将 level 参数的值设为 0 就不再需要在消息前增加额外的位置信息了。  

## pcall 与 xpcall　　

在 Lua 中，为了避免使用抛出错误和处理错误，我们需要用到 pcall 和 xpcall 函数来处理异常。  
使用 pcall(f,arg1,...) 函数可以使用保护模式调用一个函数。如果函数 f 中发生了错误， 它并不会抛出一个错误，而是返回错误的状态。使用的 pcall 函数的方法如下所示：  

```
function myfunction ()
   n = n/nil
end

if pcall(myfunction) then
   print("Success")
else
	print("Failure")
end
```  

执行上面的程序，我们可以得到如下的输出结果：  

```
Failure
```  

xpcall(f,err) 函数调用函数 f 同时为其设置了错误处理方法 err，并返回调用函数的状态。任何发生在函数 f 中的错误都不会传播，而是由 xpcall 函数捕获错误并调用错误处理函数 err，传入的参数即是错误对象本身。xpcall 的使用示例如下：  

```
function myfunction ()
   n = n/nil
end

function myerrorhandler( err )
   print( "ERROR:", err )
end

status = xpcall( myfunction, myerrorhandler )
print( status)
```  

执行上面的程序，我们可以得到如下的输出结果：  

```
ERROR:	test2.lua:2: attempt to perform arithmetic on global 'n' (a nil value)
false
```  

作为程序开发人员，在程序中正确合理地处理错误是非常重要的。正确地处理错误可以保证发生意外情况不会影响到程序用户的使用。
