

## size_t 与 int

size_t 只包含正数     它跟int一样占四个字节       只是将int表示负数的部分加到了size_t的正数上

也就是说size_t 的正数最大值是int 的两倍

**使用size_t 这种数据类型能够访问到数组中的任何一个元素   因为数组的下标不含负数**



## long long 与 long int 的区别

long int 就是 long  它与int数据类型没有区别    

而long int 占4个字节     long long 占 8 个字节



## 1ll ==> 表示数字1  但它是long long 类型   因此可以定义long long p = 0ll 表示数字0

1ll 用于数据之间进行计算时   防止其原本为int的数据类型溢出   只要乘上 1ll 就能够变成8字节的long long 类型

` long long a1 = 1ll * a2 * 10;`



## 次方表示形式

` int d = 1e+3;`   表示 10的三次方 ==>  注意 +3 必须连着写  中间不能有空格

` int d = 1e3`   同样表示10的三次方



## 无穷大的表示

long long 为 LONG_MAX    int 为 INT_MAX    

这样定义无穷大如果需要进行加1 的操作就会出现溢出

所以可以这样定义int的无穷大：  0x3f3f3f3f ==>  转化为十进制约等于 INT_MAX的1/2     所以它进行相加并不会溢出
