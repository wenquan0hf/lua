#break 语句  

程序在解释执行过程中，在循环内遇到 break 语句时，循环将立即结束。程序将循环语句的下一条语句开始执行。  
如果你是在嵌套循环（即，一个循环内还有一个循环语句）内使用 break 语句，break 只结束内层循环，并从该代码块后的第一条语句处开始执行。  

##语法  

break 语句的语法如下所示：  

```
break;
```  

##流程图：  
![](images/break.jpg)  

##示例  

```
--[ 局部变量定义--]
a = 10

--[ 执行 while 循环--]
while( a < 20 )
do
   print("value of a:", a)
   a=a+1
   if( a > 15)
   then
      --[ terminate the loop using break statement --]
      break
   end
end
```  

执行上面的代码可以得到如下的结果：  

```
value of a:	10
value of a:	11
value of a:	12
value of a:	13
value of a:	14
value of a:	15
```



