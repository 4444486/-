

## stoi与atoi

 它可以将字符串数字转化为int 类型   

**如果单纯使用 (int)  进行转化的话   它只能转化char 单个字符**

```c++
	//stoi要接收的形参是string &  而不是char  如果是char则会报错
	string a = "123";
	cout << stoi(a) << endl;
	int b = stoi(a);

	//如果要将char转化为int  就要使用atoi  但是需要传入 char *
	char d = a[0];
	int e = atoi(&d);
```



## 将int类型转化为string类型

to_string()

```c++
	string c = to_string(b);
	cout << c << endl;
```



## 通过ASCII码实现int与char之间的转换(仅限数字)

1.char -->  int   ：  char - '0'  即可  

2.int --> char   ：  char + '0'   即可



## 除了上面的 stoi和atoi 之外 还有stringstream也可以进行int与string之间的转换

```c++
	//需要包含一个头文件
	#include <sstrea>	

	stringstream ss;  //stringstream是一个数据类型
	int number = 1123;
	ss << number;  //将number写进ss中

	string s;
	ss >> s;  //从ss取出里面的int  它就会自动转化为string
	cout << s << endl;
```

