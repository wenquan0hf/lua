#Lua 中的 if 语句  

if　语句包括一个布尔表达式和一个或多个语句。　　

##语法  

Lua 语言 if 语句的语法如下：  

```
if(boolean_expression)
then
   --[如果布尔表达式为真，statement(s) 执行。--]
end
```  

如果布尔表达式计算结果为真，则 if 语句内的代码块执行；如果布尔表达式计算结果为假，跳过 if 语句中的代码直接执行 if 语句后面的代码。  
Lua 语言中所有布尔真与非 nil 的组合的结果被当作真，而布尔假与 nil 组合被当作假。值得注意的是，Lua 中零被当作真，这一点与其它大部分语言不一样：  

##流程图  

![](http://www.tutorialspoint.com/lua/images/if_statement.jpg)

##示例  

```
--[ 局部变量定义 --]
a = 10;
--[ 检查 if　语句使用的布尔条件 --]
if( a < 20 )
then
   --[ 如果条件为真则输出如下内容　--]
   print("a is less than 20" );
end
print("value of a is :", a);
```  

执行上面的代码可以得到如下的结果：　　

```
a is less than 20
value of a is : 10
```
