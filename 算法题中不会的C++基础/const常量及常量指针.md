

## const 修饰基本数据类型

```c++
//两种写法都可以 一般用第一种  const用来限制int数据类型 即a的值不能更改
const int a = 5;  //不能修改 int
int const a = 10;  //不能修改 a 

//以上两种写法在变量中作用相同 但是用在指针上就不同 如下：
```



## const修饰基本数据类型的指针

```c++
int a = 10;
int b = 5;
const int *p = &a;  //const写在最前面限制 int *  表示p这个指针变量不能修改它指向地址里面的值 但是能够修改p的指向
*p = 5;  //错误
p = &b;  //正确

const int * const p = &a;  //前面一个const限制int * 它不能修改里面的值  const限制p变量 表示p不能更改指向
```





## const修饰对象

```c++
const Node n1({1, 2});  //跟上面相同  n1对象不能修改成员属性的值
//它必须初始化 (或者在struct中进行初始化)

//如果对象在创建的时候不进行手动初始化 或者struct中没有相应的构造函数 它就会报错
const Node n2;  //报错 ==>  n2对象没有初始化
```



## const修饰指针对象

```c++
const Node *pNode = new Node;  //跟上面相同 不能修改里面的值

const Node * const pNode = new Node;  //不能修改指向也不能修改值
```



## const修饰的成员函数 ==> 常量函数

```c++
//将const写在形参列表的后面就是用来常量函数 ==> 实际上它的作用是限制this指针的
void test () const {  
    cout << "test()" << endl;
}

//如果写在前面 它就不是用来修饰函数的 而是用来修饰返回值的！ 但是它并没有作用
const int test () const {
    cout << "test()" << endl;
}
//在外部即使使用一个变量也可以接收它的返回值
//可以这么理解：因为const修饰返回值修饰的只是右值 而将它赋值给了另外一个变量 它并不能限制左值变量
int a = n1.test();  //正确


//每一个成员函数都会有一个默认的形参this  哪一个对象调用了该函数 this就指向谁 而常量函数就是限制this指向的那个对象不能修改这个对象的值
//this默认的定义方式：
Node * const this = &对象名  //即this不能更改指向 必须指向调用它的那个对象
    
//而常量函数的this定义方式：
const Node * const this = &对象名  //既不能更改指向 也不能修改里面的值
```

## 常量对象只能调用常量函数

```c++
void test () const {
	cout << "test()" << endl;
}

void test2 () {
	cout << "test2()" << endl;
}

const Node n1({1, 2});  //常量对象
n1.test(); //正确 调用常量函数
n1.test2(); //错误  因为常量函数定义就是不能修改成员属性 而调用普通的成员函数就有可能会修改它里面的属性 所以报错
```



## 外部的函数不能定义为常量函数

```c++
void test () const{  //报错 因为w
	cout << "outer test()" << endl;
}
```

