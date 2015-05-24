#Lua——元表  

正如其名，元表也是表。不过，将元表与表相关联后，我们就可以通过设置元表的键和相关方法来改变表的行为。元方法的功能十分强大，使用元方法可以实现很多的功能，比如：  

<ul>
	<li>修改表的操作符功能或为操作符添加新功能（译注：如果您学过 C++ 之类的面向对象的语言，应该比较好理解，其实它实现的是操作的重载）。</li>
	<li>使用元表中的 __index 方法，我们可以实现在表中查找键不存在时转而在元表中查找键值的功能。</li>
</ul>  

Lua 提供了两个十分重要的用来处理元表的方法，如下：  
<ul>
	<li>setmetatable(table,metatable):此方法用于为一个表设置元表。</li>
	<li>getmetatable(table)：此方法用于获取表的元表对象。</li>
</ul>  

首先，让我们看一下如何将一个表设置为另一个表的元表。示例如下：  

```
mytable = {}
mymetatable = {}
setmetatable(mytable,mymetatable)
```  

上面的代码可以简写成如下的一行代码：  

```
mytable = setmetatable({},{})
```  

##__index  

下面的例子中，我们实现了在表中查找键不存在时转而在元表中查找该键的功能：  

```
mytable = setmetatable({key1 = "value1"}, {
  __index = function(mytable, key)
    if key == "key2" then
      return "metatablevalue"
    else
      return mytable[key]
    end
  end
})

print(mytable.key1,mytable.key2)
```  

运行上面的程序，我们可以得到如下的输出结果：  

```
value1	metatablevalue
```  

接下来逐步解释上面例子运行的过程：  

<ul>
	<li>表 mytable 为 {key = "values1"}</li>
	<li>为 mytable 设置了一个元表，该元表的键 __index 存储了一个函数，我们称这个函数为元方法。</li>
	<li>这个元方法的工作也十分简单。它仅查找索引 “key2”,如果找到该索引值，则返回 "metatablevalue",否则返回 mytable 中索引对应的值。</li>
</ul>

上面的程序同样可以简化成如下的形式：  

```
mytable = setmetatable({key1 = "value1"}, { __index = { key2 = "metatablevalue" } })
print(mytable.key1,mytable.key2)
```  

##__newindex  

为元表添加 __newindex 后，当访问的键在表中不存在时，此时添加新键值对的行为将由此元方法（__newindex）定义。下面的例子中，如果访问的索引在表中不存在则在元表中新加该索引值（注意，是添加在另外一个表 mymetatable 中而非在原表 mytable 中。），具体代码如下(译注：请注意此处 __newindex 的值并非一个方法而是一个表。)：  

```
mymetatable = {}
mytable = setmetatable({key1 = "value1"}, { __newindex = mymetatable })

print(mytable.key1)

mytable.newkey = "new value 2"
print(mytable.newkey,mymetatable.newkey)

mytable.key1 = "new  value 1"
print(mytable.key1,mymetatable.newkey1)
```  

执行上面的程序，我们可以得到如下的输出结果：  

```
value1
nil	new value 2
new  value 1	nil
```  

可以看出，在上面的程序中，如果键存在于主表中，只会简单更新相应的键值。而如果键不在表中时，会在另外的表 mymetatable 中添加该键值对。  
在接下来这个例子中，我们用 rawset 函数在相同的表（主表）中更新键值，而不再是将新的键添加到另外的表中。代码如下所示：  

```
mytable = setmetatable({key1 = "value1"}, {
  __newindex = function(mytable, key, value)
		rawset(mytable, key, "\""..value.."\"")

  end
})

mytable.key1 = "new value"
mytable.key2 = 4

print(mytable.key1,mytable.key2)
```  

执行上面的程序，我们可以得到如下的输出结果：  

```
new value	"4"
```  

rawset 函数设置值时不会使用元表中的 __newindex 元方法。同样的，Lua 中也存的一个 rawget 方法，该方法访问表中键值时也不会调用 __index 的元方法。  

##为表添加操作符行为  

使用 + 操作符完成两个表组合的方法如下所示（译注：可以看出重载的意思了）：  

```
mytable = setmetatable({ 1, 2, 3 }, {
  __add = function(mytable, newtable)
    for i = 1, table.maxn(newtable) do
      table.insert(mytable, table.maxn(mytable)+1,newtable[i])
    end
    return mytable
  end
})

secondtable = {4,5,6}

mytable = mytable + secondtable
for k,v in ipairs(mytable) do
print(k,v)
end
```  

执行上面的的程序，我们可以得到如下的输出结果：  

```
1	1
2	2
3	3
4	4
5	5
6	6
```  

元表中 __add 键用于修改加法操作符的行为。其它操作对应的元表中的键值如下表所示。  

<table>
	<tr>
		<th>键</th>
		<th>描述</th>
	</tr>
	<tr>
		<td>__add</td>
		<td>改变加法操作符的行为。</td>
	</tr>
	<tr>
		<td>__sub</td>
		<td>改变减法操作符的行为。</td>
	</tr>
	<tr>
		<td>__mul</td>
		<td>改变乘法操作符的行为。</td>
	</tr>
	<tr>
		<td>__div</td>
		<td>改变除法操作符的行为。</td>
	</tr>
	<tr>
		<td>__mod</td>
		<td>改变模除操作符的行为。</td>
	</tr>
	<tr>
		<td>__unm</td>
		<td>改变一元减操作符的行为。</td>
	</tr>
	<tr>
		<td>__concat</td>
		<td>改变连接操作符的行为。</td>
	</tr>
	<tr>
		<td>__eq</td>
		<td>改变等于操作符的行为。</td>
	</tr>
	<tr>
		<td>__lt</td>
		<td>改变小于操作符的行为。</td>
	</tr>
	<tr>
		<td>__le</td>
		<td>改变小于等于操作符的行为。</td>
	</tr>
</table>  

##__call  

使用 __call 可以使表具有像函数一样可调用的特性。下面的例子中涉及两个表，主表 mytable 和 传入的实参表结构 newtable，程序完成两个表中值的求和。

```
mytable = setmetatable({10}, {
  __call = function(mytable, newtable)
	sum = 0
	for i = 1, table.maxn(mytable) do
		sum = sum + mytable[i]
	end
    for i = 1, table.maxn(newtable) do
		sum = sum + newtable[i]
	end
	return sum
  end
})
newtable = {10,20,30}
print(mytable(newtable))
```  

运行上面的代码，我们可以得到如下的输出结果：  

```
70
```  

##__tostring  

要改变 print 语句的行为，我们需要用到 __tostring 元方法。下面是一个简单的例子：  

```
mytable = setmetatable({ 10, 20, 30 }, {
  __tostring = function(mytable)
    sum = 0
    for k, v in pairs(mytable) do
		sum = sum + v
	end
    return "The sum of values in the table is " .. sum
  end
})
print(mytable)
```  

运行上面的代码，我们可以得到如下的输出结果：  

```
The sum of values in the table is 60
```  

如果你完全掌握了元表的用法，你就可以实现很多看上面很复杂的操作。如果不使用元表，就不仅仅是看上去很复杂了，而是真的非常复杂。所以，多做一些使用元表的练习，并熟练掌握所有元表的可选项，这会让你受益匪浅。