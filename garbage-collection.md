#Lua——垃圾回收机制  

Lua 通过特定算法的垃圾回收机制实现自动内存管理。由于自动内存管理机制的存在，作为程序开发人员：  
<ul>
	<li>不需要关心对象的内存分配问题。</li>
	<li>不再使用对象时，除了将引用它的变量设为 nil，不需要主动释放对象。</li>
</ul>  
Lua 的垃圾回收器会不断运行去收集不再被 Lua 程序访问的对象。  
所有的对象，包括表、userdata、函数、线程、字符串等都是由自动内存管理机制管理它们空间的分配和翻放。Lua 实现了一个增量式标记清除垃圾收集器。它用两个数字控制垃圾回收周期，垃圾收集器暂停时间（garbage-collector pause） 和垃圾收集器步长倍增器（garbage-collector step multiplier）。其数值是以百分制计数的，即数值 100 内部表示 1。  

##垃圾收集器暂停时间 

该数值被用于控制垃圾收集器被 Lua 自动内存管理再次运行之前需要的等待时长。当其小于 100 时意味着收集器在新周期开始前不再等待。同样地，值越大垃圾回收器被运行的频率越低，越不主动。当其值 200 时，收集器在总使用内存数量达到上次垃圾收集时的两倍时再开启新的收集周期。因此，根据程序不同的特征，可以通过修改该值使得程序达到最佳的性能。  

##垃圾收集器步长倍增器  

步长倍增器用于控制了垃圾收集器相对内存分配的速度。数值越大收集器工作越主动，但同时也增加了垃圾收集每次迭代步长的大小。值小于 100 可能会导致垃圾器一个周期永远不能结束，建议不要这么设置。默认值为 200，表示垃圾收集器运行的速率是内存分配的两倍。

##垃圾回收器相关函数  

作为开发人员，我们可能需要控制 Lua 的自动内存管理机制，可以使用下面的这些方法：  
<ul>
	<li>collectgarbage("collect")：运行一个完整的垃圾回收周期。</li>
	<li>collectgarbage("count")：返回当前程序使用的内存总量，以 KB 为单位。</li>
	<li>collectgarbage("restart")：如果垃圾回收器停止，则重新运行它。</li>
	<li>collectgarbage("setpause")：设置垃圾收集暂停时间变量的值，值由第二个参数指出（第二参数的值除以 100 后赋予变量）。稍后，我们将详细讨论它的用法。</li>
	<li>collectgarbage("setsetmul")：设置垃圾收集器步长倍增器的值，第二个参数的含义与上同。</li>
	<li>collectgarbage("step")：进行一次垃圾回收迭代。第二个参数值越大，一次迭代的时间越长；如果本次迭代是垃圾回收的最后一次迭代则此函数返回 true。</li>
	<li>collectgarbage("stop")：停止垃圾收集器运行。</li>
</ul>

下面的示例代码中使用了垃圾收集器相关函数，如下所示：  

```
mytable = {"apple", "orange", "banana"}

print(collectgarbage("count"))

mytable = nil

print(collectgarbage("count"))

print(collectgarbage("collect"))

print(collectgarbage("count"))
```  

运行上面的程序，我们可以得到如下的输出结果。请注意，输出结果与操作系统类型与 Lua 自动内存管理都有关，所以可能运行的结果与下面不相同。  

```
20.9560546875
20.9853515625
0
19.4111328125
```  

从上面的程序，我们可以看出，一旦垃圾回收运行后，使用的内存量立即就减少了。但是，我们并不需要主动去调用它。因为，即使我们不调用此函数，Lua 也会按配置的周期自动的调用垃圾回收器。  
显然，如果需要，我们可以用上面的这些函数调整垃圾回收器的行为。这些函数帮且程序开发人员处理更加复杂的场景。根据开发的不同程序的内存需求，我们可以使用到这些方法来提高程序的性能。虽然大部分情况下，我们都不会用到这些函数，但是了解这些方法可以帮助我们调试程序，以免应用上线后带来的损失。 

译注：更多垃圾回收器的内容请参考官网或者<a href= "http://www.xuebuyuan.com/1636688.html">此博客</a>。