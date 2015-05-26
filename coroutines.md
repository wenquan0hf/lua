# Lua 协程  

## 概述 
 
协程具有协同的性质，它允许两个或多个方法以某种可控的方式协同工作。在任何一个时刻，都只有一个协程在运行，只有当正在运行的协程主动挂起时它的执行才会被挂起（暂停）。 
 
上面的定义可能看上去比较模糊。接下来让我讲得很清楚一点，假设我们有两个方法，一个是主程序方法，另一个是一个协程。当我们使用 resume 函数调用一个协程时，协程才开始执行。当在协程调用 yield 函数时，协程挂起执行。再次调用 resume 函数时，协程再从上次挂起的地方继续执行。这个过程一直持续到协程执行结束为止。  

## 协程中可用的函数  

下面的表中列出 Lua 语言为支持协程而提供的所有函数以及它们的用法。  

<table>
	<tr>
		<th>S.N.</th>
		<th>方法和功能</th>
	</tr>
	<tr>
		<td>1</td>
		<td>coroutine.create(f):用函数 f 创建一个协程，返回 thread 类型对象。</td>
	</tr>
	<tr>
		<td>2</td>
		<td>coroutine.resume(co[,val1,...]): 传入参数（可选），重新执行协程 co。此函数返回执行状态，也可以返回其它值。</td>
	</tr>
	<tr>
		<td>3</td>
		<td>coroutine.running():返回正在运行的协程，如果在主线程中调用此函数则返回 nil。</td>
	</tr>
	<tr>
		<td>4</td>
		<td>coroutine.status(co):返回指定协程的状态，状态值允许为：正在运行(running)，正常(normal)，挂起(suspended)，结束(dead)。</td>
	</tr>
	<tr>
		<td>5</td>
		<td>coroutine.wrap(f):与前面 coroutine.create 一样，coroutine.wrap 函数也创建一个协程，与前者返回协程本身不同，后者返回一个函数。当调用该函数时，重新执行协程。</td>
	</tr>
	<tr>
		<td>6</td>
		<td>coroutine.yield(...):挂起正在执行的协程。为此函数传入的参数值作为执行协程函数 resume 的额外返回值（默认会返回协程执行状态）。</td>
	</tr>
</table>

## 示例  

让我们通过下面的例子来理解一下协程这个概念。  

```
co = coroutine.create(function (value1,value2)
   local tempvar3 =10
   print("coroutine section 1", value1, value2, tempvar3)
   local tempvar1 = coroutine.yield(value1+1,value2+1)
   tempvar3 = tempvar3 + value1
   print("coroutine section 2",tempvar1 ,tempvar2, tempvar3)
   local tempvar1, tempvar2= coroutine.yield(value1+value2, value1-value2)
   tempvar3 = tempvar3 + value1
   print("coroutine section 3",tempvar1,tempvar2, tempvar3)
   return value2, "end"
end)

print("main", coroutine.resume(co, 3, 2))
print("main", coroutine.resume(co, 12,14))
print("main", coroutine.resume(co, 5, 6))
print("main", coroutine.resume(co, 10, 20))
```  

执行上面的程序，我们可以得到如下的输出结果：  

```
coroutine section 1	3	2	10
main	true	4	3
coroutine section 2	12	nil	13
main	true	5	1
coroutine section 3	5	6	16
main	true	2	end
main	false	cannot resume dead coroutine
```  

## 上面的例子到底做了些什么呢？  

和前面说到的一样，在例子中我们使用 resume 函数继续执行协程，用 yield 函数挂起协程。同样，从例子中也可以看出如何为执行协程的 resueme 函数返回多个值。下面我将逐步解释上面的代码。  

<ul>
	<li>首先，我们创建了一个协程并将其赋给变量 co。此协程允许传入两个参数。</li>
	<li>第一次调用函数 resume 时，协程内局部变量 value1 和 value2 的值分别为 3 和 2。</li>
	<li>为了便于理解，我们使用了局部变量 tempvar3 该变量被初始化为 10。由于变量 value1 的值为3，所以 tempvar3 在随后的协程调用过程中被先后更新为 13 和 16。</li>
	<li>第一次调用 coroutine.yield 时，为 resume 函数返回了值 4 和 3，这两个值是由传入的参数 3，2 分别加 1 后的结果，这一点可以从 yield 语句中得到证实。除了显示指定的返回值外，resume 还收到隐式的返回值 true，该值表示协程执行的状态，有 true 和 false 两个可能取值。</li>
	<li>上面的例子中，我们还应该关注在下一次调用 resume 时如何为协程传入参数。从例子中可以看到，coroutine.yield 函数返回后为两个变量赋值，该值即是第二次调用 resume 时传入的参数。这种参数传递的机制让可以结合前面传入的参数完成很多新的操作。</li>
	<li>最后，协程中所有语句执行完后，后面的调用就会返回 false 状态，同时返回 "cannot resume dead coroutine"消息。</li>
</ul>

## 另一个协程的示例  

下面这例子中的协程使用 yield 函数和 resume 函数依次返回数字 1 到 5。示例中，如果没有协程对象或对象已结束（dead），则重新创建一个新的协程对象；若协程已经存在，则执行已经存在的协程。  

```
function getNumber()
   local function getNumberHelper()
      co = coroutine.create(function ()
      coroutine.yield(1)
      coroutine.yield(2)
      coroutine.yield(3)
      coroutine.yield(4)
      coroutine.yield(5)
      end)
      return co
   end
   if(numberHelper) then
      status, number = coroutine.resume(numberHelper);
      if coroutine.status(numberHelper) == "dead" then
         numberHelper = getNumberHelper()
         status, number = coroutine.resume(numberHelper);
      end
      return number
   else
      numberHelper = getNumberHelper()
      status, number = coroutine.resume(numberHelper);
      return number
   end
end
for index = 1, 10 do
   print(index, getNumber())
end
```  
执行上述的程序，我们可以得到如下的输出结果：  

```
1	1
2	2
3	3
4	4
5	5
6	1
7	2
8	3
9	4
10	5
```

大家经常会把协程和多线程编程语言中的线程进行对比，但我们要明白，协程有着与线程类似的特性，但是协程与线程的区别在于协程不能并发，任意时刻只会有一个协程执行，而线程允许并发的存在。（译注：译者认为本质上协程其是就是线程，不过是用户态的线罢了，它将调度问题交由程序开发人员手动完成。）  

我们通过控制程序执行顺序以满足获取某些临时信息的需求。配合全局变量的使用，协和会变得更加的灵活方便。