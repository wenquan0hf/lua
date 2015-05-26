# Lua 表  

在 Lua 语言中，表是唯一可以用来创建不同数据类型的数据结构，比如常见的数组和字典都是用表来创建的。 Lua 语言中经常到关联数组这种数据类型，它不仅可以用数值作为索引值，除了 nil 以外的字符串同样可以作为其索引。表没有固定的大小，当数据量增加时表会自动增大。  

Lua 语言中的各种结构表示都用到了表，包括包（package）的表示。当我们使用方法 string.format 时，我们用到的其实是包 string 中的方法 format。  

## 使用表

表被称之为对象，它既不是值也不是变量。Lua 用构造表达式 {} 创建一个空表。需要注意的是，在存储表的变量和表本身之间没有什么固定的对应关系（译注：一个表可以被不同的变量引用，一个变量也可以随时改变其所引用的表对象）。  

```
--简单的表初始化
mytable = {}

--简单的表赋值
mytable[1]= "Lua"

--移除引用
mytable = nil
-- lua 的垃圾回收机制负责回收内存空间
```  

当我们有一个拥有一系列元素的表时，如果我们将其赋值给 b。那么 a 和 b 都会引用同一个表对象(a 先引用该表)，指向相同的内存空间。而不会再单独为 b 分配内存空间。即使给变量 a 赋值 nil，我们仍然可以用变量 b 访问表本身。如果已经没有变量引用表时，Lua　语言垃圾回收机制负责回收不再使用的内存以被重复使用。
  
下面的示例代码使用到了上面提到的表的特性。　　

```
-- 声明空表
mytable = {}
print("Type of mytable is ",type(mytable))

mytable[1]= "Lua"
mytable["wow"] = "Tutorial"
print("mytable Element at index 1 is ", mytable[1])
print("mytable Element at index wow is ", mytable["wow"])

-- alternatetable 与 mytable 引用相同的表
alternatetable = mytable

print("alternatetable Element at index 1 is ", alternatetable[1])
print("mytable Element at index wow is ", alternatetable["wow"])

alternatetable["wow"] = "I changed it"

print("mytable Element at index wow is ", mytable["wow"])

-- 只是变量被释放，表本身没有被释放
alternatetable = nil
print("alternatetable is ", alternatetable)

-- mytable 仍然可以访问
print("mytable Element at index wow is ", mytable["wow"])

mytable = nil
print("mytable is ", mytable)
```  

执行上面的代码，我们可以得到如下的输出结果：  

```
Type of mytable is 	table
mytable Element at index 1 is 	Lua
mytable Element at index wow is 	Tutorial
alternatetable Element at index 1 is 	Lua
mytable Element at index wow is 	Tutorial
mytable Element at index wow is 	I changed it
alternatetable is 	nil
mytable Element at index wow is 	I changed it
mytable is 	nil
```  

## 表的操作函数  

下面的表中列出了 Lua 语言内置的表操作函数，具体内容如下所示：  

<table>
	<tr>
		<th>S.N.</th>
		<th>方法与作用</th>
	</tr>
	<tr>
		<td>1</td>
		<td>table.concat(table[, sep [, i[,j]]]): 根据指定的参数合并表中的字符串。具体用法参考下面的示例。</td>
	</tr>
	<tr>
		<td>2</td>
		<td>table.insert(table,[pos,]value):在表中指定位置插入一个值。</td>
	</tr>
	<tr>
		<td>3</td>
		<td>table.maxn(table)：返回表中最大的数值索引。</td>
	</tr>
	<tr>
		<td>4</td>
		<td>table.remove(table[,pos]):从表中移出指定的值。</td>
	</tr>	<tr>
		<td>5</td>
		<td>table.sort(table[,comp]):根据指定的（可选）比较方法对表进行排序操作。</td>
	</tr>
</table>  

让我们一起看一些上述函数使用的例子。  

### 表连接操作  

我们可以使用表连接操作连接表中的元素，如下所示。  

```
fruits = {"banana","orange","apple"}
-- 返回表中字符串连接后的结果
print("Concatenated string ",table.concat(fruits))

--用字符串连接
print("Concatenated string ",table.concat(fruits,", "))

--基于索引连接 fruits 
print("Concatenated string ",table.concat(fruits,", ", 2,3))
```  

执行上面的代码，我们可以得到如下的输出结果：  

```
Concatenated string 	bananaorangeapple
Concatenated string 	banana, orange, apple
Concatenated string 	orange, apple
```  

### 插入与移出操作  

插入和移除表中元素是对表最常见的操作。使用方法如下所示：  

```
fruits = {"banana","orange","apple"}

-- 在 fruits 的末尾插入一种水果
table.insert(fruits,"mango")
print("Fruit at index 4 is ",fruits[4])

-- 在索引 2 的位置插入一种水果
table.insert(fruits,2,"grapes")
print("Fruit at index 2 is ",fruits[2])

print("The maximum elements in table is",table.maxn(fruits))

print("The last element is",fruits[5])
table.remove(fruits)
print("The previous last element is",fruits[5])
```  

执行上面的代码，我们可以得到如下的输出结果：  

```
Fruit at index 4 is 	mango
Fruit at index 2 is 	grapes
The maximum elements in table is	5
The last element is	mango
The previous last element is	nil
```  

### 表排序操作  

在程序开发过程中，常常有对表排序的需求。 sort 函数默认使用字母表对表中的元素进行排序（可以通过提供比较函数改变排序策略）。示例代码如下：  

```
fruits = {"banana","orange","apple","grapes"}
for k,v in ipairs(fruits) do
print(k,v)
end
table.sort(fruits)
print("sorted table")
for k,v in ipairs(fruits) do
print(k,v)
end
```  

执行上面的代码，我们可以得到如下的输出结果：  

```
1	banana
2	orange
3	apple
4	grapes
sorted table
1	apple
2	banana
3	grapes
4	orange
```  