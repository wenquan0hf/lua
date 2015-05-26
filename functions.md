# Lua 函数  

函数用于将一组语句组合起来完成一个任务。你可以将你的代码分割到不同的函数中。如何将你的代码分到不同的函数中完全由你自己决定，不过一般会按照逻辑功能进行划分，每个函数都执行一个特定的任务。 
 
在 Lua 中提供了大量的内置函数供我们使用。例如，print() 函数用于将输入的参数输出到终端。 
 
函数往往也被称作方法，子例程或过程等等。  

## 函数定义  

Lua 中函数定义的语法如下所示：　　

```
optional_function_scope function function_name( argument1, argument2, argument3..., argumentn)
function_body
return result_params_comma_separated
end
```  

Lua 中函数定义包括函数头和函数名两部分。如下列出函数的所有部分：  

<ul>
	<li>可选的函数作用域：你可以使用关键字 local 限制函数的作用域，你也可以忽略此部分而使用默认值。函数作用域默认是全局。</li>
	<li>函数名：函数的真正名称。函数名与函数的参数列表一起被称为函数签名。</li>
	<li>参数：一个参数就一个占位符一样。函数调用时，把值传递给参数。这个值被称之为实际参数或直参数。参数列表指参数的类型，顺序与数量。参数是可选的，一个函数可以没有参数。</li>
	<li>函数体：函数体是代码语句集合，定义了函数的功能。</li>
	<li>返回：在 Lua 中，可以使用 return 关键字同时返回多返回值，每个返回值之间使用逗号分隔。</li>
</ul>  

## 示例  

下面是函数 max() 源代码。此函数接受两个参数 num1 与 num2，返回两个输入参数的最大值。  

```
--[[ function returning the max between two numbers --]]
function max(num1, num2)

   if (num1 > num2) then
      result = num1;
   else
      result = num2;
   end

   return result; 
end
```  

## 函数参数  

如果函数需要用到参数，则它必须声明接受参数值的变量。这些被声明的变量被称为函数的形式参数或简称形参。 
 
函数的形参与函数中其它局部变量一样，在函数的入口处被创建，函数结束时被销毁。  

## 调用函数  

创建函数的时候，我们已经定义了函数做什么。接下来，我们就可以调用函数来完成已定义的任务或功能。
  
当程序中调用一个函数时，程序的控制转移到被调用的函数中。被调用的函数执行定义的任务；当 return 语句被执行或者到达函数末尾时，程序的控制回到主程序中。  
 
调用函数的方法很简单，你只需要将函数要求的参数传递给函数就可以实现函数的调用。如果函数有返回值，你也可以将函数的返回值存储下来。如下如示：  

```
function max(num1, num2)
   if (num1 > num2) then
      result = num1;
   else
      result = num2;
   end

   return result; 
end

-- 调用函数
print("The maximum of the two numbers is ",max(10,4))
print("The maximum of the two numbers is ",max(5,6))
```  

执行上面的代码，可以得到如下的输出结果：  

```
The maximum of the two numbers is 	10
The maximum of the two numbers is 	6
```  

## 赋值与传递函数  

在 Lua 语言中，我们可以将函数赋值给一个变量，也可以将函数作为参数传递给另外一个函数。下面是赋值传递函数的一个例子：  

```
myprint = function(param)
   print("This is my print function -   ##",param,"##")
end

function add(num1,num2,functionPrint)
   result = num1 + num2
   functionPrint(result)
end
myprint(10)
add(2,5,myprint)
```  

执行上面的代码，可以得到如下的输出结果：  

```
This is my print function -   ##	10	##
This is my print function -   ##	7	##
```  

## 变参函数  

在 Lua 语言中，使用 ... 作为参数可以创建参数个数可变的函数，即变参函数。我们可以使用下面的这个例子来理解变参函数的概念。下面的这个例子中函数返回输入参数的平均值：  

```
function average(...)
   result = 0
   local arg={...}
   for i,v in ipairs(arg) do
      result = result + v
   end
   return result/#arg
end

print("The average is",average(10,5,3,4,5,6))
```  

执行上面的代码，可以得到如下的输出结果：  

```
The average is	5.5
``` 