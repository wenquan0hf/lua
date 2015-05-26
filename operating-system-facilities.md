# Lua 操作系统工具库  

在很多应用中，我们都需要访问到操作系统级别的函数，操作系统库就给我们提供了这样的工具。下面的列表给出操作系统工具包提供的方法：  


<table>
	<tr>
		<th>S.N.</th>
		<th>函数与功能</th>
	</tr>
	<tr>
		<td>1</td>
		<td>os.clock()：以秒为单位返回程序运行所用 CPU 时间的近似值。</td>
	</tr>
	<tr>
		<td>2</td>
		<td>os.date([format[,time]])：返回时间字符串或包含时间的表，时间按指定格式格式化。</td>
	</tr>
	<tr>
		<td>3</td>
		<td>os.difftime(t2,t1)：返回从 t1 时刻至 t2 时刻经历的时间。在 POSIX，windows，及其它某些系统中，该值就是 t2-t1。</td>
	</tr>
	<tr>
		<td>4</td>
		<td>os.execute([command])：该函数等价于 ANSI C 中的 system 函数。传递的参数 command 由操作系统的 shell 执行。如果命令成功结束，则返回的第一个值为 true，否则为 nil。</td>
	</tr>
	<tr>
		<td>5</td>
		<td>os.exit([code[,close]])：调用 ANSI C 的 exit 函数，结束程序。如果 code 为　true, 则返回状态为 EXIT_SUCESS；若 code 为 false,则返回状态为 EXIT_FAILURE。如果 code 为数值，则返回状态也就为该数值。</td>
	</tr>
	<tr>
		<td>6</td>
		<td>os.getenv(varname)：返回进程的环境变量 varname 的值，如果此环境变量没有定义则返回 nil。</td>
	</tr>
	<tr>
		<td>7</td>
		<td>os.remove(filename)：删除文件（或 POSIX 系统中的空目录）。如果函数失败，则返回 nil 以及描述错误的字符串与错误代码。</td>
	</tr>
	<tr>
		<td>8</td>
		<td>os.rename(oldname,newname)：重命名文件或目录。如果函数失败，则返回 nil 以及描述错误的字符串与错误代码。</td>
	</tr>
	<tr>
		<td>9</td>
		<td>os.setlocale(locale[,category])：设置程序当前的地区（locale），locale 是一个与操作系统相关的字符串。category 是一个可选的字符串，描述设置更改的范围，包括: all，collate，ctype，monetary，numeric，time。默认为 all。函数返回新地区的名称，如果函数调用失败则返回 nil。</td>
	</tr>
	<tr>
		<td>10</td>
		<td>os.time([table])：无参数时，返回当前时间；传入参数时，则返回指定参数表示的日期和时间。传入的参数必须包含以下的域:年、月、日。时（默认 12）、分（默认 0）、秒（默认 0）、isdst（默认 nil） 四个域是可选的。</td>
	</tr>
	<tr>
		<td>11</td>
		<td>os.tmpname()：返回一个可作为临时文件名的字符串。这个临时文件必须显式地打开，使用结束时也必须显式地删除。</td>
	</tr>
</table>

## 常用的 OS 函数  

示例如下：  

```
-- 格式化日期
io.write("The date is ", os.date("%m/%d/%Y"),"\n")

-- 日期与时间
io.write("The date and time is ", os.date(),"\n")

-- 时间
io.write("The OS time is ", os.time(),"\n")

-- 等待一段时间
for i=1,1000000 do
end

-- Lua 启动的时长
io.write("Lua started before ", os.clock(),"\n")
```  

执行上面的程序，我们可以得到如下的输出结果： 

```
The date is 01/25/2014
The date and time is 01/25/14 07:38:40
The OS time is 1390615720
Lua started before 0.013
```  

上面的例子只是简单的说明了一下操作系统相关函数的使用，实际开发过程中，可以根据实际情况使用所有函数。可以多做一些练习熟悉上面所列出的函数。
