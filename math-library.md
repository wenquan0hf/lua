# Lua 数学函数库  

在科学计算与工程计算领域，我们都需要用到大量的数学函数。在 Lua 的数学库提供了大量的数学函数，如下表所示：  


<table>
	<tr>
		<th>S.N.</th>
		<th>函数与功能</th>
	</tr>
	<tr>
		<td>1</td>
		<td>math.abs(x)：返回 x 的绝对值。</td>
	</tr>
	<tr>
		<td>2</td>
		<td>math.acos(x)：返回 x 的反余弦值（弧度）。</td>
	</tr>
	<tr>
		<td>3</td>
		<td>math.asin(x)：返回 x 的反正弦值（弧度）。</td>
	</tr>
	<tr>
		<td>4</td>
		<td>math.atan(x)：返回 x 的反正切值（弧度）。</td>
	</tr>
	<tr>
		<td>5</td>
		<td>math.atan2(y,x)，返回 y/x 的反正切值，使用两个参数的符号查找象限（ x 为 0 时也能正确的处理）。</td>
	</tr>
	<tr>
		<td>6</td>
		<td>math.ceil（x）:返回大于或等于 x 的最小整数。</td>
	</tr>
	<tr>
		<td>7</td>
		<td>math.cos(x)：返回 x 的余弦值（x 以弧度为单位）。</td>
	</tr>
	<tr>
		<td>8</td>
		<td>math.cosh(x)：返回 x 的双曲余弦值。</td>
	</tr>
	<tr>
		<td>9</td>
		<td>math.deg(x)：返回 x　的角度值（ｘ为弧度）。</td>
	</tr>
	<tr>
		<td>10</td>
		<td>math.exp(x)：返回 e 的 x 次幂。</td>
	</tr>
	<tr>
		<td>11</td>
		<td>math.floor(x)：返回小于或等于 x 的最大整数。</td>
	</tr>
	<tr>
		<td>12</td>
		<td>math.fmod(x,y): 返回 x%y。</td>
	</tr>
	<tr>
		<td>13</td>
		<td>math.frexp(x)：返回两个值 m，e，满足 x = m*2^e。其中，e 是整数，m 的绝对值属于区间 [0.5,1)。</td>
	</tr>
	<tr>
		<td>14</td>
		<td>math.huge：最大值，不小于任何其它数值。</td>
	</tr>
	<tr>
		<td>15</td>
		<td>math.ldexp(m,e)：返回 m*2^e(e 为整数)。</td>
	</tr>
	<tr>
		<td>16</td>
		<td>math.log(x)：计算自然对数。</td>
	</tr>
	<tr>
		<td>17</td>
		<td>math.log10(x)：计数以 10 为底的对数。</td>
	</tr>
	<tr>
		<td>18</td>
		<td>math.max(x,...)：返回输入参数的最大值。</td>
	</tr>
	<tr>
		<td>19</td>
		<td>math.min(x,...)：返回输入参数的最小值。</td>
	</tr>
	<tr>
		<td>20</td>
		<td>math.modf(x)：返回 x 的整数部分与小数部分。</td>
	</tr>
	<tr>
		<td>21</td>
		<td>math.pi：数值 PI。</td>
	</tr>
	<tr>
		<td>22</td>
		<td>math.pow(x,y)：等价于 x^y。</td>
	</tr>
	<tr>
		<td>23</td>
		<td>math.rad(x)：返回角度 x 的弧度值。</td>
	</tr>
	<tr>
		<td>24</td>
		<td>math.random([m,[n]])：该函数直接调用 ANSI C 的伪随机生成函数。无参数时，生成 [0,1) 区间的均匀分布的随机值；只传入参数 m 时，函数生成一个位于区间 [1,m] 的均匀分布伪随机值；同时传入参数 m,n 时，生成位于区间 [m,n] 的均匀分布伪随机值。</td>
	</tr>
	<tr>
		<td>25</td>
		<td>math.randomseed(x)：初始化伪随机数生成器种子值。</td>
	</tr>
	<tr>
		<td>26</td>
		<td>math.sin(x)：返回 x 的正弦值。</td>
	</tr>
	<tr>
		<td>27</td>
		<td>math.sinh(x)：返回 x 的双曲正弦值。</td>
	</tr>
	<tr>
		<td>28</td>
		<td>math.sqrt(x)：返回 x 的平方根。</td>
	</tr>
	<tr>
		<td>29</td>
		<td>math.tan(x)：返回 x 的正切值。</td>
	</tr>
	<tr>
		<td>30</td>
		<td>math.tanh(x)：返回 x 的双曲正切值。</td>
	</tr>
</table>

## 三角函数  

三角函数的使用方法示例如下：  

```
radianVal = math.rad(math.pi / 2)

io.write(radianVal,"\n")

-- Sin value of 90(math.pi / 2) degrees
io.write(string.format("%.1f ", math.sin(radianVal)),"\n")
-- Cos value of 90(math.pi / 2) degrees
io.write(string.format("%.1f ", math.cos(radianVal)),"\n")
-- Tan value of 90(math.pi / 2) degrees
io.write(string.format("%.1f ", math.tan(radianVal)),"\n")
-- Cosh value of 90(math.pi / 2) degrees
io.write(string.format("%.1f ", math.cosh(radianVal)),"\n")
-- Pi Value in degrees
io.write(math.deg(math.pi),"\n")
```  

运行上面的程序，我们可以得到如下的输出结果：  

```
0.027415567780804
0.0 
1.0 
0.0 
1.0 
180
```  

## 另外一些常用的数学函数  

```
-- Floor
io.write("Floor of 10.5055 is ", math.floor(10.5055),"\n")
-- Ceil
io.write("Ceil of 10.5055 is ", math.ceil(10.5055),"\n")
-- Square root
io.write("Square root of 16 is ",math.sqrt(16),"\n")
-- Power
io.write("10 power 2 is ",math.pow(10,2),"\n")
io.write("100 power 0.5 is ",math.pow(100,0.5),"\n")
-- Absolute
io.write("Absolute value of -10 is ",math.abs(-10),"\n")
--Random
math.randomseed(os.time())
io.write("Random number between 1 and 100 is ",math.random(),"\n")
--Random between 1 to 100
io.write("Random number between 1 and 100 is ",math.random(1,100),"\n")
--Max
io.write("Maximum in the input array is ",math.max(1,100,101,99,999),"\n")
--Min
io.write("Minimum in the input array is ",math.min(1,100,101,99,999),"\n")
```  

运行上面的程序，我们可以得到如下的输出结果：  

```
Floor of 10.5055 is 10
Ceil of 10.5055 is 11
Square root of 16 is 4
10 power 2 is 100
100 power 0.5 is 10
Absolute value of -10 is 10
Random number between 1 and 100 is 0.22876674703207
Random number between 1 and 100 is 7
Maximum in the input array is 999
Minimum in the input array is 1
```  

上面的例子只是给出了数学函数一些简单的使用方法，在实际中我们可以根据自己的需要进行选择和使用。通过更多的练习可以对这些函数更加的熟悉。
