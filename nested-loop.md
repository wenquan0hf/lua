#Lua 循环嵌套  

Lua 编程语言允许使用循环嵌套。接下来这一节中将用例子来说嵌套循环的使用方法：  

##语法  

for 循环嵌套的语法如下：  

```
for init,max/min value, increment
do
   for init,max/min value, increment
   do
      statement(s)
   end
   statement(s)
end
```  
while 循环嵌套的语法如下：  

```
while(condition)
do
   while(condition)
   do
      statement(s)
   end
   statement(s)
end
```  

repeat...until  循环嵌套的语法如下：  

```
repeat
   statement(s)
   repeat
      statement(s)
   until( condition )
until( condition )
```  

需要注意的是，在任何外层循环类型内可以使用任何内层循环类型。  

##示例  

下面的例子中使用了嵌套循环：  

```
j =2
for i=2,10 do
   for j=2,(i/j) , 2 do
      if(not(i%j)) 
      then
         break 
      end
      if(j > (i/j))then
         print("Value of i is",i)
      end
   end
end
```

运行上面的代码，可以得到如下的输出结果：　　

```
Value of i is	8
Value of i is	9
Value of i is	10
```
