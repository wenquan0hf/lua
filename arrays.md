# Lua 数组  

数组是一组有序的对象排列，既可以是一维的也可以是多维的。  

在 Lua 语言中，数组是用整数索引表实现的。数组的大小并不固定，随着数组元素的增加，它可以动态地增加内存空间大小。  

## 一维数组  

一维数组可以使用一个简单的表结构表示。可以通过一个简单循环初始化数组或者读取数组中数据。示例代码如下所示：  

```
array = {"Lua", "Tutorial"}

for i= 0, 2 do
   print(array[i])
end
```  

执行上面的代码可以得到如下的输出结果：  

```
nil
Lua
Tutorial
```  

从上面的例子中可以看出，当我们尝试着访问数组中一个不存在的索引时，会得到 nil 值。 Lua 语言与 C 语言不同，Lua 数组的索引是从 1 开始的，而 C 语言中索引是从 0 开始的。不过呢，你也可以在索引值为 0 或小于 0 的位置创建对象。下面的代码演示了如何使用负索引值创建并初始化数组：  

```
array = {}

for i= -2, 2 do
   array[i] = i *2
end

for i = -2,2 do
   print(array[i])
end
```  

执行上面的代码可以得到如下的输出结果：  

```
-4
-2
0
2
4
```  

## 多维数组  

多维数组有以下两种实现方式：  
<ol>
	<li>数组的数组（译注：数组的每一个元素是一个数组）。</li>
	<li>修改一维数组的索引值（译注：将多维数组映射到一维数组中）。</li>
</ol>  

使用方法一创建 3x3 的二维数组：  

```
-- 初始化数组
array = {}
for i=1,3 do
   array[i] = {}
      for j=1,3 do
         array[i][j] = i*j
      end
end

-- 访问数组元素
for i=1,3 do
   for j=1,3 do
      print(array[i][j])
   end
end
```  

执行上面的代码可以得到如下的输出结果：  

```
1
2
3
2
4
6
3
6
9
```  

通过修改数组的的索引值实现 3x3 的二维数组，示例代码如下:  

```
-- 初始化数组
array = {}
maxRows = 3
maxColumns = 3
for row=1,maxRows do
   for col=1,maxColumns do
      array[row*maxColumns +col] = row*col
   end
end

-- 访问数组元素
for row=1,maxRows do
   for col=1,maxColumns do
      print(array[row*maxColumns +col])
   end
end
```  

执行上面的代码可以得到如下的输出结果：  

```
1
2
3
2
4
6
3
6
9
```  

正如从上面例子中所看到的那样，数组中数据是基于索引存储的。这使得数组可以以稀疏的方式存储，这也是 Lua 矩阵的存储方式。正是因为 Lua 中不会存储 nil 值，所以 Lua　不需要使用任何特殊的技术就可以节约大量的空间，这一点在其它语言中是做不到的。