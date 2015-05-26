# Lua 模块  

## 什么是模块？
  
Lua 中的模块与库的概念相似，每个模块都有一个全局唯一名字，并且每个模块都包含一个表。使用一个模块时，可以使用 require 加载模块。模块中可以包括函数和变量，所有这些函数和变量被表存储于模块的表中。模块中的表的功能类似于命名空间，用于隔离不同模块中的相同的变量名。在使用模块的时候，我们应该遵守模块定义的规范，在 require 加载模块时返回模块中的包含函数和变量的表对象。  

## Lua 模块的特别之处  

模块中表的使用使得我们可在绝大多数情况下可以像操作其它表一样操作模块。由于 Lua 语言允许对模块本身进行操作，所以 Lua 也就具备了许多其它语言需要特殊机制才能实现的特殊性质。例如，这种自由的表操作机制使得编程人员可以用多种方法调用模块中的函数。下面的例子演示了其中的一些方法：  

```
-- 假设我们有一个板块 printFormatter
-- 该模块有一个函数 simpleFormat(arg)
-- 方法 1
require "printFormatter"
printFormatter.simpleFormat("test")

-- 方法 2
local formatter = require "printFormatter"
formatter.simpleFormat("test")

-- 方法 3
require "printFormatter"
local formatterFunction = printFormatter.simpleFormat
formatterFunction("test")
```  

从上面的例子中可以看出，Lua 不需要任何额外的代码就可以实现非常灵活的编程技巧。  

## require 函数  

Lua 提供了一个高层次抽象的函数 require，使用这个函数可以加载所有需要的模块。在设计之初，这个函数就被设计的尽可能的简单，以避免加载模块时需要太多的模块信息。require 函数加载模块时把所有模块都只当作一段定义了变量的代码（事实上是一些函数或者包含函数的表）而已，完全不需要更多的模块信息。  

## 示例  

让我们看一下面这个例子。在这个例子中，我们定义了模块 mymath,在这个模块中定义一些数学函数，并将该模块存储于 mymath.lua 文件中。具体内容如下：  

```
local mymath =  {}
function mymath.add(a,b)
   print(a+b)
end

function mymath.sub(a,b)
   print(a-b)
end

function mymath.mul(a,b)
   print(a*b)
end

function mymath.div(a,b)
   print(a/b)
end

return mymath
```  

接下来，我们在另一个文件　moduletutorial.lua 文件中访问这个模块。具体代码如下所示：  

```
mymathmodule = require("mymath")
mymathmodule.add(10,20)
mymathmodule.sub(30,20)
mymathmodule.mul(10,20)
mymathmodule.div(30,20)
```

运行这段代码这前，我们需要将两个 lua 源代码文件放在同一目录下，或者把模块代码文件放在包路径下（这种情况需要额外的配置）。运行上面的代码，可以得到如下的输出结果：  

```
30
10
200
1.5
```  

## 注意事项  

<ul>
	<li>把模块和待运行的文件放在相同的目录下。</li>
	<li>模块的名称与文件名称相同。</li>
	<li>为 require 函数返回模块（在模块中使用 return 命令返回存储了函数和变量的表）。尽管有其它的模块实现的方式，但是建议您使用上面的实现方法。</li>
</ul>  

## 更老的实现模块的方法 

下面，我们将用 package.seall 这种比较老的方法重新实现上面的例子。这种实现方法主要用于 Lua 5.1 或 5.2 版本。使用这种方式实现模块的代码如下所示：  

```
module("mymath", package.seeall)

function mymath.add(a,b)
   print(a+b)
end

function mymath.sub(a,b)
   print(a-b)
end

function mymath.mul(a,b)
   print(a*b)
end

function mymath.div(a,b)
   print(a/b)
end
```  

使用此模块的代码如下所示：  

```
require("mymath")
mymath.add(10,20)
mymath.sub(30,20)
mymath.mul(10,20)
mymath.div(30,20)
```  

当我们运行这段代码，我们会得到与前面相同的输出结果。但是建议你不要使用这种方式，因为普遍认为这种方式不及新的方法安全。许多用到 Lua 语言的 SDK 都已经不再使用这种方式定义模块，例如, Corna SDK。