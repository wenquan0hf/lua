# Lua 调试  

Lua 提供一个调试库，这个库中提供了创建自己的调试器所需的所有原语函数。虽然，Lua 没有内置调试器，但是开发者们为 Lua 开发了许多的开源调试器。 
 
Lua 调试库包括的函数如下表所示。
  
<table>
	<tr>
		<th>S.N.</th>
		<th>方法和描述</th>
	</tr>
	<tr>
	<td>1</td>
		<td>debug():进入交互式调试模式，在此模式下用户可以用其它函数查看变量的值。</td>
	</tr>
	<tr>
		<td>2</td>
		<td>getfenv(object):返回对象的环境。</td>
	</tr>
	<tr>
		<td>3</td>
		<td>gethook(optional thread)：返回线程当前的钩子设置，总共三个值：当前钩子函数、当前的钩子掩码与当前的钩子计数。</td>
	</tr>
	<tr>
		<td>4</td>
		<td>getinfo(optional thread,function or stack leve,optional flag)：返回保存函数信息的一个表。你可以直接指定函数，或者你也可以通过一个值指定函数，该值为函数在当前线程的函数调用栈的层次。其中，0 表示当前函数（getinfo 本身）；层次 1 表示调用 getinfo 的函数，依次类推。如果数值大于活跃函数的总量，getinfo 则返回 nil。</td>
	</tr>
	<tr>
		<td>5</td>
		<td>getlocal(optional thread,stack level,local index)：此函数返回在 level 层次的函数中指定索引位置处的局部变量和对应的值。如果指定的索引处不存在局部变量，则返回 nil。当 level 超出范围时，则抛出错误。</td>
	</tr>
	<tr>
		<td>6</td>
		<td>getmetatable(value)：返回指定对象的元表，如果不存在则返回 nil。</td>
	</tr>
	<tr>
		<td>7</td>
		<td>getregistry()：返回寄存器表。寄存器表是一个预定义的用于 C 代码存储 Lua 值的表。</td>
	</tr>
	<tr>
		<td>8</td>
		<td>getupvalue(func function,upvalue index)：根据指定索引返回函数 func 的 upvalue 值（译注：upvalue 值与函数局部变量的区别在于，即使函数并非活跃状态也可能有 upvalue 值，而非活跃函数则不存在局部变量，所以其第一个参数不是栈的层次而是函数）。如果不存在，则返回 nil。</td>
	</tr>
	<tr>
		<td>9</td>
		<td>setfenv(function or thread or userdata,environment table)：将指定的对象的环境设置为 table,即改变对象的作用域。</td>
	</tr>
	<tr>
		<td>10</td>
		<td>sethook(optional thread,hook function,hook mask string with "c" and/or "r" and/or "l",optional instruction count)：把指定函数设置为钩子。字符串掩码和计数值表示钩子被调用的时机。这里，c 表示每次调用函数时都会执行钩子；r 表示每次从函数中返回时都调用钩子；l 表示每进入新的一行调用钩子。</td>
	</tr>
	<tr>
		<td>11</td>
		<td>setlocal(optional thread,stack level,local index,value):在指定的栈深度的函数中，为 index 指定的局部变量赋予值。如果局部变量不存在，则返回 nil。若 level 超出范围则抛出错误；否则返回局部变量的名称。</td>
	</tr>
	<tr>
		<td>12</td>
		<td>setmetatable(value,metatable):为指定的对象设置元表，元表可以为 nil。</td>
	</tr>
	<tr>
		<td>13</td>
		<td>setupvalue(function,upvalue index,value):为指定函数中索引指定的 upvalue 变量赋值。如果 upvalue 不存在，则返回 nil。否则返回此 upvalue 的名称。</td>
	</tr>
	<tr>
		<td>14</td>
		<td>traceback(optional thread,optional meesage string,opitona level argument)：用 traceback 构建扩展错误消息。</td>
	</tr>
</table>

上面的表中列出了 Lua 的全部调试函数，我们经常用到的调试库都会用到上面的函数，它让调试变得非常容易。虽然提供了便捷的接口，但是想要用上面的函数创建一个自己的调试器并不是件容易的事。无论怎样，我们可以看一下下面这个例子中怎么使用这些调试函数的。  

```
function myfunction ()
print(debug.traceback("Stack trace"))
print(debug.getinfo(1))
print("Stack trace end")
	return 10
end
myfunction ()
print(debug.getinfo(1))
```

执行上面的程序，我们可以得到如下的栈轨迹信息：  

```
Stack trace
stack traceback:
	test2.lua:2: in function 'myfunction'
	test2.lua:8: in main chunk
	[C]: ?
table: 0054C6C8
Stack trace end
```  

上面的例子中，我们使用 debug.trace 函数输出了栈轨迹。 debug.getinfo 函数获得函数的当前表。  

## 示例二  

在调试过程中，我们常常需要查看或修改函数局部变量的值。因此，我们可以用 getupvalue 获得变量的值，用 setupvalue 修改变量的值。示例如下：  

```
function newCounter ()
  local n = 0
  local k = 0
  return function ()
    k = n
    n = n + 1
    return n
    end
end

counter = newCounter ()
print(counter())
print(counter())

local i = 1

repeat
  name, val = debug.getupvalue(counter, i)
  if name then
    print ("index", i, name, "=", val)
	if(name == "n") then
		debug.setupvalue (counter,2,10)
	end
    i = i + 1
  end -- if
until not name

print(counter())
```  

运行上面的程序，我们可以得到如下面的输出结果：  

```
1
2
index	1	k	=	1
index	2	n	=	2
11
```  

在这个例子中，每次调用 counter 都会更新该闭包函数。我们可以通过 getupvalue 查看其当前的局部变量值。随后，我们更新局部变量的值。在为 n 设置新值之前，其值为 2。调用 setupvalue 后，n 被设置为 10。再调用 counter 时，它就会返回值 11 而不再是 3。  

## 调试类型  

<ul>
	<li>命令行调试</li>
	<li>图形界面调试</li>
</ul>

### 命令行调试工具  

命令行调试就是使用命令行命令和 print 语句来调试程序。已经有许多现成的 Lua 命令行调试工具，下面列出了其中的一部分：
  
- RemDebug：RemDebug 是一个远程的调试器，它支持 Lua 5.0 和 5.1 版本。允许远程调试 Lua 程序，设置断点以及查看程序的当前状态。同时，它还能调试 CGILua 脚本。
- clidebugger：此调试器是用纯 Lua 脚本开发的命令行调试工具，支持 Lua 5.1。除了 Lua 5.1 标准库以外，它不依赖于任何其它的 Lua 库。虽然它受到了 RemDebug 影响而产生的，但是它没有远程调试的功能。
- ctrace：跟踪 Lua API 调用的小工具。
- xdbLua：windows 平台下的 Lua 命令行调试工具。
- LuaInterface - Debuger：这个项目是 LuaInterface 的扩展，它对 Lua 调试接口进行进一步的抽象，允许通过事件和方法调用的方式调试程序。
- RIdb：使用套接字的远程 Lua 调试器，支持 Linux 和 Windows 平台。它的特性比任何其它调试器都丰富。
- ModDebug：允许远程控制另外一个 Lua　程序的执行、设置断点以及查看程序的当前状态。


### 图形界面调试工具  

图形界面的调试工具往往和集成开发环境（IDE）打包在一起。它允许在可视环境下进行调试，比如查看变量值，栈跟踪等。通过 IDE 的图形界面，你可以设置断点单步执行程序。  

下面列出了几种图形界面的调试工具。  

<ul>
	<li>SciTE：Windows 系统上默认的 Lua 集成开发环境，它提供了丰富的调试功能，比如，断点、单步、跳过、查看变量等等。</li>
	<li>Decoda：一个允许远程调试的图形界面调试工具。</li>
	<li>ZeroBrane Studio：一个 Lua 的集成开发环境，它集成了远程调试器、栈视图、远程控制终端、静态分析等诸多功能。它兼容各类 Lua 引擎，例如 LuaJIT,Love2d,Moai等。支持 Windows, OSX, Linux；开源。</li>
	<li>akdebugger：eclipse 的 Lua 调试器和编辑器插件。</li>
	<li>luaedit：支持运程调试、本地调试、语法高亮、自动补完、高级断点管理（包括有条件地触发断和断点计数）、函数列表、全局和本地变量列表、面对方案的管理等。</li>
</ul>