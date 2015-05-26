# Lua 标准库  

Lua 标准库利用 C 语言 API 实现并提供了丰富的函数，它们内置于 Lua 语言中。该标准库不仅可以提供 Lua 语言内服务，还能提供外部服务，比如文件或数据库的操作。  

这些标准库使用标准的 C API 接口实现，它们作为独立的 C 语言模块提供给使用者。主要包括以下的内容：
<ul>
	<li>基本库，包括协程子库</li>
	<li>模块库</li>
	<li>字符串操作</li>
	<li>表操作</li>
	<li>数学计算库</li>
	<li>文件输入与输出</li>
	<li>操作系统工具库</li>
	<li>调试工具库</li>
</ul>

## 基本库  

在本教程中，我们已经在很多地方都使用了基本库的内容。下面的表中列出了相关的函数与链接。  

<table>
	<tr>
		<th>S.N.</th>
		<th>库或者方法</th>
	</tr>
	<tr>
		<td>1</td>
		<td>错误处理库，包括错误处理函数，比如 assert，error等，详见<a href="./error-handling.md">错误处理</a>。</td>
	</tr>
	<tr>
		<td>2</td>
		<td>内存管理，包括与垃圾回收相关的自动内存管理的内容，详见<a href="./garbage-collection.md">垃圾回收</a>。</td>
	</tr>
	<tr>
		<td>3</td>
		<td>dofile([filename])，打开指定文件，并将文件内容作为代码执行。如果没有传入参数，则函数执行标准输入的内容。错误会传递至函数调用者。</td>
	</tr>
	<tr>
		<td>4</td>
		<td>_G，全局变量，它存储全局的环境。Lua 本身不使用此变量。</td>
	</tr>
	<tr>
		<td>5</td>
		<td>getfenv([f])，返回指定函数使用的当前环境（作用域），可以通过函数名或栈深度值指定函数。 1 表示调用 getfenv 的函数。如果传入的参数不是函数或者 f 为 0,则返回全局环境。f 的默认值为 1。</td>
	</tr>
	<tr>
		<td>6</td>
		<td>getmetatable(object)：如果对象没有元表，则返回 nil。如果对象的元表有 __metable 域，则返回该值；否则返回对象的元表。</td>
	</tr>
	<tr>
		<td>7</td>
		<td>ipairs(t)，用于遍历表，此函数返回三个值：next 函数，表 t, 以及 0。</td>
	</tr>
	<tr>
		<td>8</td>
		<td>load(func[,chunkname])，使用函数 func 加载一个块（代码块），每次调用func必须返回与先前的结果连接后的字符串</td>
	</tr>
	<tr>
		<td>9</td>
		<td>loadfile([filename]),与 load 函数相似，此函数从文件中或标准输入（没指定文件名时）读入代码块。</td>
	</tr>
	<tr>
		<td>10</td>
		<td>loadstring(string,[,chunkname])，与 load 类似，从指定字符串中获得代码块。</td>
	</tr>
	<tr>
		<td>11</td>
		<td>next(table[,index])，此函数用于遍历表结构。第一参数为表，第二个参数是一个索引值。返回值为指定索引的下一个索引与相关的值。</td>
	</tr>
	<tr>
		<td>12</td>
		<td>pairs(t),用于遍历表，此函数返回三个值：next 函数，表 t, 以及 nil。</td>
	</tr>
	<tr>
		<td>13</td>
		<td>print(...)，打印输出传入参数。</td>
	</tr>
	<tr>
		<td>14</td>
		<td>rawequal(v1,v2)，判断 v1 与 v2 是否相等，不会调用任何元方法。返回布尔值。</td>
	</tr>
	<tr>
		<td>15</td>
		<td>rawget(table,index)，返回 table[index]，不会调用元方法。table 必须是表，索引可以是任何值。</td>
	</tr>
	<tr>
		<td>16</td>
		<td>rawset(table,index,value)，等价于 table[index] = value，但是不会调用元方法。函数返回表。</td>
	</tr>
	<tr>
		<td>17</td>
		<td>select(index,...)，如果 index 为数字n,那么 select 返回它的第 n 个可变实参，否则只能为字符串 "#",这样select会返回变长参数的总数。</td>
	</tr>
	<tr>
		<td>18</td>
		<td>setfenv(f,table)，设置指定函数的作用域。可以通过函数名或栈深度值指定函数。 1 表示调用 setfenv 的函数。返回值为指定函数。特别地，如果 f 为 0，则改变当前线程的运行环境，这时候函数无返回值。</td>
	</tr>
	<tr>
		<td>19</td>
		<td>setmetatable(table,metatable)，设置指定表的元表（不能从 Lua 中改变其它类型的元表，其它类型的元表只能从 C 语言中修改）。如果 metatable 为 nil，则删除表的元表；如果原来的元表有 __metatable 域，则出错。函数返回 table。 </td>
	</tr>
	<tr>
		<td>20</td>
		<td>tonumber(e[,base])，将参数转换为数值。如果参数本身已经是数值或者是可以转换为数值的字符串，则 tonumber 返回数值，否则返回 nil。</td>
	</tr>
	<tr>
		<td>21</td>
		<td>tostring(e)，将传递的实参以合理的格式转换为字符串。精确控制字符串的转换可以使用 string.format 函数。</td>
	</tr>
	<tr>
		<td>22</td>
		<td>type(v)，以字符串的形式返回输入参数的类型。该函数的返回值可以取字符串：nil，number，string，boolean，table，function，thread，userdata。</td>
	</tr>
	<tr>
		<td>23</td>
		<td>unpack(list[,i[,j]])，从指定的表中返回元素。</td>
	</tr>
	<tr>
		<td>24</td>
		<td>_VERSION，存储当前解释器版本信息的全局变量。该变量当前存储的内容为：Lua 5.1。（译注：与解释器版本有关）</td>
	</tr>
	<tr>
		<td>25</td>
		<td>协程库，包括协程相关的函数，详见<a href="./coroutines.md">协程</a></td>
	</tr>
</table>

## 模块库  

模块库提供了加载模块的基本函数。它在全局作用域内提供了 require 函数。其它的函数都是通过包管理的模块库提供的。详细内容请参见<a href="./modules.md">模块</a>。

## 字符串操作库  

详细内容请参见<a href="./strings.md">字符串</a>。

## 表操作库  

详细内容请参见<a href="./tables.md">表</a>。

## 数学函数库

详细内容请参见<a href="./math-library.md">数学函数库</a>。 

## 文件 IO 库  

详细内容请参见<a href="./file-io.md">文件 IO</a>。 

## 操作系统工具库  

详细内容请参见<a href="./operating-system-facilities.md">操作系统工具库</a>。 

## 调试工具库  

详细内容请参见<a href="./debugging.md">调试</a>。 