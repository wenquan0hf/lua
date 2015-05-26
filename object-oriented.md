# Lua 面向对象  

## 面向对象概述  

面向对象编程技术是目前最常用的编程技术之一。目前大量的编程语言都支持面向对象的特性： 
<ul>
	<li>C++</li>
	<li>Java</li>
	<li>Objective-C</li>
	<li>Smalltalk</li>
	<li>C#</li>
	<li>Ruby</li>
</ul>

## 面向对象的特征  

<ul>
	<li>类（class）：类是可以创建对象，并为状态（成员变量）提供初值及行为实现的可扩展模板。</li>
	<li>对象（objects）：对象是类的实例，每个对象都有独立的内存区域。</li>
	<li>继承（inheritance）：继承用于描述一个类的变量和函数被另一个类继承的行为。</li>
	<li>封装（encapsulation）：封装是指将数据和函数组织在一个类中。外部可以通过类的方法访问内中的数据。封装也被称之为数据抽象。</li>
</ul>
## Lua 中的面向对象  

在 Lua 中，我们可以使用表和函数实现面向对象。将函数和相关的数据放置于同一个表中就形成了一个对象。继承可以用元表实现，它提供了在父类中查找存在的方法和变量的机制。  
Lua 中的表拥有对象的特征，比如状态和独立于其值的标识。两个有相同值的对象（表）是两个不同的对象，但是一个对象在不同的时间可以拥有不同的值。与对象一样，表拥有独立于其创建者和创建位置的生命周期。  

## 一个真实世界的例子  

面向对象已经是一个广泛使用的概念，但是你需要正确清楚地理解它。  
让我们看一个数学方面的例子。我们经常需要处理各种形状，比如圆、矩形、正方形。  
这些形状有一个共同的特征——面积。所以，所有其它的形状都可以从有一个公共特征——面积的基类扩展而来。每个对象都可以有它自己的特征和函数，比如矩阵有属性长、宽和面积，printArea 和 calculateArea 方法。  

### 创建一个简单的类  

下面例子实现了矩阵类的三个属性：面积、长和宽。它还同时实现了输出面积的函数 printArea。  

```
-- 元类
Rectangle = {area = 0, length = 0, breadth = 0}

-- 继承类的方法 new
function Rectangle:new (o,length,breadth)
  o = o or {}
  setmetatable(o, self)
  self.__index = self
  self.length = length or 0
  self.breadth = breadth or 0
  self.area = length*breadth;
  return o
end

-- 继承类的方法 printArea
function Rectangle:printArea ()
  print("The area of Rectangle is ",self.area)
end
```  

### 创建对象  

创建对象即是为类的实例分配内存空间的过程。每个对象都有自己独立的内存区域，同时还会共享类的数据。  

```
r = Rectangle:new(nil,10,20)
```  

### 访问属性

我们可以使用点操作符访问类中属性。  

```
print(r.length)
```  

### 访问成员方法  

使用冒号操作符可以访问对象的成员方法，如下所示：  

```
r:printArea()
```  

初始化阶段，调用函数为对象分配内存同时设置初值。这与其它与面向对象的语言中的构造器很相似。其实，构造器本身也就和上面的初始化代码一样，并没有什么特别之处。  

## 完整的例子  

让我们一起看一个 Lua 实现面向对象的完整例子。  

```
-- 元类
Shape = {area = 0}

-- 基类方法 new
function Shape:new (o,side)
  o = o or {}
  setmetatable(o, self)
  self.__index = self
  side = side or 0
  self.area = side*side;
  return o
end

-- 基类方法 printArea
function Shape:printArea ()
  print("The area is ",self.area)
end

-- 创建对象
myshape = Shape:new(nil,10)

myshape:printArea()
```  

运行上面的程序，我们可以得到如下的输出结果：  

```
The area is 	100
```  

### Lua 中的继承  

继承就是从基对象扩展的过程，正如从图形扩展至矩形、正方形等等。在现实世界中，常用来共享或扩展某些共同的属性和方法。  
让我们看一个简单的类扩展的例子。我们有如下的类：  

```
 -- 元类
Shape = {area = 0}
-- 基类方法 new
function Shape:new (o,side)
  o = o or {}
  setmetatable(o, self)
  self.__index = self
  side = side or 0
  self.area = side*side;
  return o
end
-- 基类方法 printArea
function Shape:printArea ()
  print("The area is ",self.area)
end
```  

我们从上面的类中扩展出正方形类，如下所示：  

```
Square = Shape:new()
-- 继承类方法 new
function Square:new (o,side)
  o = o or Shape:new(o,side)
  setmetatable(o, self)
  self.__index = self
  return o
end
```  

### 重写基类的函数  

继承类可以重写基类的方法，从而根据自己的实际情况实现功能。示例代码如下所示：  

```
-- 继承方法 printArea
function Square:printArea ()
  print("The area of square is ",self.area)
end
```  

## 继承的完整示例  

在元表的帮助下，我们可以使用新的 new 方法实现类的扩展（继承）。子类中保存了所有基类的成员变量和方法。  

```
 -- Meta class
Shape = {area = 0}
-- Base class method new
function Shape:new (o,side)
  o = o or {}
  setmetatable(o, self)
  self.__index = self
  side = side or 0
  self.area = side*side;
  return o
end
-- Base class method printArea
function Shape:printArea ()
  print("The area is ",self.area)
end

-- Creating an object
myshape = Shape:new(nil,10)
myshape:printArea()

Square = Shape:new()
-- Derived class method new
function Square:new (o,side)
  o = o or Shape:new(o,side)
  setmetatable(o, self)
  self.__index = self
  return o
end

-- Derived class method printArea
function Square:printArea ()
  print("The area of square is ",self.area)
end

-- Creating an object
mysquare = Square:new(nil,10)
mysquare:printArea()

Rectangle = Shape:new()
-- Derived class method new
function Rectangle:new (o,length,breadth)
  o = o or Shape:new(o)
  setmetatable(o, self)
  self.__index = self
  self.area = length * breadth
  return o
end

-- Derived class method printArea
function Rectangle:printArea ()
  print("The area of Rectangle is ",self.area)
end

-- Creating an object
myrectangle = Rectangle:new(nil,10,20)
myrectangle:printArea()
```  

运行上面的程序，我们可以得到如下的输出结果：  

```
The area is 	100
The area of square is 	100
The area of Rectangle is 	200
```  

上面的例子中，我们继承基类 Shape 创建了两个子类 Rectange 与 Square。在子类中可以重写基类提供的方法。在这个例子中，子类重写了 printArea 方法。