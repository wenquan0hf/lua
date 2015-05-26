# Lua 迭代器  

迭代器是用于遍历集合或容器中元素的一种结构。在 Lua 语言中，集合往往指的是可以用来创建各种数据结构的表。比如，数组就是用表来创建的。  

## 通用迭代器  

通用迭代器可以访问集合中的键值对。下面是通用迭代器的一个简单例子：  

```
array = {"Lua", "Tutorial"}

for key,value in ipairs(array) 
do
   print(key, value)
end
```  

执行的上面的代码，我们可以得到如下的输出结果：  

```
1  Lua
2  Tutorial
```  

上面的例子中使用了 Lua 提供的默认迭代器函数 ipairs。 
 
在 Lua 语言中，我们使用函数表示迭代器。根据是否在迭代器函数中是否维护状态信息，我们将迭代器分为以下两类：  

<ul>
	<li>无状态迭代器</li>  
	<li>有状态迭代器</li>
</ul>  

## 无状态迭代器  

由此迭代器的名称就可以看出来，这一类的迭代器函数中不会保存任何中间状态。 
 
让我们一起来看一下下面这个例子。在这个例子中，我们用一个简单的函数创建了一个自己的迭代器。这个迭代器用以输出 n 个数的平方值。  

```
function square(iteratorMaxCount,currentNumber)
   if currentNumber<iteratorMaxCount
   then
      currentNumber = currentNumber+1
   return currentNumber, currentNumber*currentNumber
   end
end

for i,n in square,3,0
do
   print(i,n)
end
```  

执行上面的代码，我们可以得到如下的输出结果：  

```
1	1
2	4
3	9
```  

我们可以稍微的修改一下上面的代码，使得此迭代器可以像 ipairs 那样工作。如下所示：  

```
function square(iteratorMaxCount,currentNumber)
   if currentNumber<iteratorMaxCount
   then
      currentNumber = currentNumber+1
   return currentNumber, currentNumber*currentNumber
   end
end

function squares(iteratorMaxCount)
   return square,iteratorMaxCount,0
end  

for i,n in squares(3)
do 
	print(i,n)
end
```  
执行上面的代码，我们可以得到如下的输出结果：  

```
1	1
2	4
3	9
```  

## 有状态迭代器  

前面的例子使用的迭代器函数是不保存状态的。每次调用迭代器函数时，函数基于传入函数的第二个变量访问集合的下一个元素。在 Lua 中可以使用闭包来存储当前元素的状态。闭包通过函数调用得到变量的值。为了创建一个新的闭包，我们需创建两个函数，包括闭包函数本身和一个工厂函数，其中工厂函数用于创建闭包。  

下面的示例中，我们将使用闭包来创建我们的迭代器。  

```
array = {"Lua", "Tutorial"}

function elementIterator (collection)
   local index = 0
   local count = #collection
   -- 返回闭包函数
   return function ()
      index = index + 1
      if index <= count
      then
         -- 返回迭代器的当前元素
         return collection[index]
      end
   end
end

for element in elementIterator(array)
do
   print(element)
end
```  

执行上面的代码，我们可以得到如下的输出结果：  

```
Lua
Tutorial
```  

上面的例子中我们可以看到，在　elementIterator 函数内定义了另外一个匿名函数。此匿名函数中使用了一个外部变量 index (译注：此变量在匿名函数之外，elementIterator 函数内)。每次内部的匿名函数被调用时，都会将 index 的值增加 1，并统计数返回的每个元素。 
 
我们可以参照上面的方法使用闭包创建一个迭代器函数。每次我们使用迭代器遍历集合时，它都可以返回多个元素。