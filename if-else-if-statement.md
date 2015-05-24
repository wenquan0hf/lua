#Lua 中的 if...else 语句  

如果 if 语句后面跟上 else 语句，那么条件为假时就执行 else 语句的代码。  

##语法  

Lua 语言中 if...else 语句的语法如下所示：  

```
if(boolean_expression)
then
   --[ 如何条件为真，则执行此处代码。 --]
else
   --[ 如何条件为假，则执行此处代码。 --]
end
```  

当布尔表达式为真时，执行 if 语句的代码块；如果条件为假时，则执行 else 语句的代码块。  
Lua 语言中所有布尔真与非 nil 的组合的结果被当作真，而布尔假与 nil 组合被当作假。值得注意的是，Lua 中零被当作真，这一点与其它大部分语言不一样。

##流程图  

![](http://www.tutorialspoint.com/lua/images/if_else_statement.jpg)  

##示例  

```
--[ 定义局部变量 --]
a = 100;
--[ 检查条件 --]
if( a < 20 )
then
   --[ 如果条件为真，则输出如下内容 --]
   print("a is less than 20" )
else
   --[ 如果条件为假，则输出如下内容 --]
   print("a is not less than 20" )
end
print("value of a is :", a)
```  

执行上面的代码则可以得到如下的输出结果：  

```
a is not less than 20
value of a is :	100
```  

##if...else if...else 语句  

if 语句后可以选择跟上 else if...else 语句。该语句对于检测多个条件时非常有用。  
使用 if，else if 以及 else 时，请注意以下三点：  
<ul>
	<li>if 语句后面至多可以有一个 else 语句。如果有　else if,则此 else 语句必须在 else if 语句之后。</li>
	<li>if 语句之后可以有多零个或多个 else if，但是这些 else if 必须在　else 语句之前。</li>
	<li>如果一个 if 语句的条件为真时，其后的所有剩余的 else 和 else if 的都不会再执行，也不会测试它们的条件真假。</li>
</ul>  

##语法  

Lua 中 if...else if...else 语句的语法规则如下：  

```
if(boolean_expression 1)
then
   --[ 如果布尔表达式 1 为真时，则执行此处代码。--]

else if( boolean_expression 2)
   --[ 如果布尔表达式 2 为真时，则执行此处代码。 --]

else if( boolean_expression 3)
   --[ 如果布尔表达式 3为真时，则执行此处代码。 --]
else 
   --[ 当上面所有布尔表达式条件都为假时执行此处代码。--]
end
```  

##示例  

```
--[ 定义局部变量 --]
a = 100
--[ 检查布尔条件 --]
if( a == 10 )
then
   --[ 条件为真时输出如下内容 --]
   print("Value of a is 10" )
elseif( a == 20 )
then   
   --[ if else if 条件为真时 --]
   print("Value of a is 20" )
elseif( a == 30 )
then
   --[ if else if 条件为真时 --]
   print("Value of a is 30" )
else
   --[ 如果上述条件全部为假时 --]
   print("None of the values is matching" )
end
print("Exact value of a is: ", a )
```  
  
执行上面的代码将得到如下的输出结果：  

```
None of the values is matching
Exact value of a is:	100
```  
