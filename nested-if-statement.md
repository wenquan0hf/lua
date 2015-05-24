#Lua 中的嵌套 if 语句  

在 Lua 语言中，你可以合法的嵌套使用 if-else 语句。这也就是说，你可以在一个 if 或 if-else 语句内再使用一个　if 或 if-else 语句。  

##语法  

嵌套 if 语句的语法规则如下：  

```
if( boolean_expression 1)
then
   --[ 如果布尔表达式 1 为真，则执行此处代码。 --]
   if(boolean_expression 2)
   then
      --[ 如果布尔表达式 2 为真（注：布尔表达式 1 为真），则执行此处代码）。 --]
   end
end
```  

你也可以像嵌套使用 if 语句那样使用嵌套使用 else if...else 语句。  

##示例  

```
--[ 定义局部变量 --]
a = 100;
b = 200;
--[ 检查条件真假 --]
if( a == 100 )
then
   --[ 如果前面的条件为真，再检查下面的条件。 --]
   if( b == 200 )
   then
      --[ 如果条件为真，则输出如下内容 --]
      print("Value of a is 100 and b is 200" );
   end
end
print("Exact value of a is :", a );
print("Exact value of b is :", b );
```  

执行上面的代码，可以得到如下的输出结果：  

```
Value of a is 100 and b is 200
Exact value of a is :	100
Exact value of b is :	200
```