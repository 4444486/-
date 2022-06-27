

## lambda表达式的格式

` [](){};`     中括号表示需要使用的外部变量   小括号表示传递的形参     大括号是函数体        如果要立即执行lambda表达式的话  需要在后面再加上一个小括号

例如： ` [=](int a){` 

​				`return a}(3)`       = 等号表示引用外部所有的变量的值拷贝   int a 接收外部的一个int实参    return a 是函数体   ()是执行函数   **实参列表写在调用的小括号中**



```c++
void test () {
	
	auto a = [](int a){return a;}(3);
	cout << a << endl;
	cout << sizeof(a) << endl;
}
```

​        

注意：

```c++
	//注意change只是一个lambda表达式类型 它并不是string类型
	auto change = [](string cur, int k, int p)->string{
    	...
        return string;
	};
```

