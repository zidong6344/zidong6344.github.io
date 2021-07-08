## C++提高编程

针对C++泛式编程和STL中的概念

### 1.模板

#### 1.1模板的概念

定义：建立通用的模具，提高复用性

特点：

- 不可以直接使用，只是一个框架

- 模板的通用并不是万能的

#### 1.2函数模板

- C++另一种编程思想称为泛型编程，主要利用的技术就是模板
- C++提供两种模板机制：函数模板和类模板

##### 1.2.1函数模板的语法

函数模板的作用：建立一个通用函数，其函数返回值类型和形参类型可以不具体指定，用一个虚拟的类型来代表

（参数类型也通用了）

**语法：**

```C++
template<typename T>
函数声明或定义
```

**解释：**

template -- 声明创建模板

typename -- 表明其后面的符号式一种数据类型，可以用class代替

T -- 通用的数据类型，名称可以替换，通常为大写字母

**示例：**

```C++
template<typename T>  // 声明一个模板，告诉编译器后面代码中紧跟着的T不要报错，T是一个通用的数据类型
void mySwap(T& a, T& b) {
	T temp = a;
	a = b;
	b = temp;
}
void test() {
	int a = 10;
	int b = 20;
	//swapinit(a, b);
	//两种方式使用函数模板
	//1.自动类型推导
	//mySwap(a, b);
	//2.指定类型
	//mySwap<int>(a, b);
	cout << 'a=' << a << endl;
	cout << "b=" << b << endl;
 }
```

**总结**：

- 函数模板利用了关键字template
- 使用函数模板有两种方式：自动类型推导、显示指定类型
- 模板的目的式为了提高复用性，将类型参数化

##### 1.2.2函数模板的注意事项

- 函数模板用typename

  类模板用class

  (函数模板和类模板都可以用class）

- 自动类型推导的模板类型必须要一致
- 使用模板时必须要指定出通用的数据类型T

```C++
template<typename T> 
void func() {
	cout << "func调用" << endl;
}
void test1() {
	//func();//错误
	func<int>();//正确
}
```

##### 1.2.3普通函数和函数模板的区别

1. 普通函数 调用可以发生隐式类型转换
2. 函数模板 用自动类型推导，不可以发生隐式类型转换
3. 函数模板 用显示指定类型，可以发生隐式类型转换

```C++
template<class T>
T myadd(T a, T b) {
	return a + b;
}
void test() {
	int a = 10;
	int b = 20;
	char c = 'a';
	//cout << myadd(a, b) << endl;//错误
	cout << myadd<int>(a, c) << endl;//正确
}
```

##### 1.2.4普通函数和函数模板的调用规则

1. 如果函数模板和普通函数都可以实现，优先调用普通函数
2. 可以通过空模板参数列表来强制调用函数模板
3. 函数模板可以发生重载
4. 如果函数模板可以产生更好的匹配，优先调用函数模板

```c++
int myadd(int a, int b) {
	cout << "调用普通函数" << endl;
	return a + b;
}
template<class T>
T myadd(T a, T b) {
	cout << "调用函数模板" << endl;
	return a + b;
}
template<class T>
T myadd(T a, T b, T c) {
	cout << "调用函数模板" << endl;
	return a + b + c;
}
void test() {
	int a = 10;
	int b = 20;
    //1.如果函数模板和普通函数都可以实现，优先调用普通函数
	cout << myadd(a, b) << endl;
    //2.可以通过空模板参数列表来强制调用函数模板
    cout << myadd<>(a, b) << endl;
    //3.函数模板可以发生重载
    cout << myadd(a, b, c) << endl;
    //4.如果函数模板可以更好的匹配，优先调用函数模板
    char a = 'a';
	char b = 'b';
	cout << myadd(a, b) << endl;
}
```

注意：提供了函数模板就不要提供普通函数了，否则容易出现二义性

##### 1.2.5  模板的局限性

1. 如果模板传入的是一个数组，无法执行相减的操作
2. 如果模板传入的是一个Person这种的自定义数据类型，无法进行比较的操作（操作符重载除外）

因此C++为了解决这种问题，提供了模板的重载，可以为这些特定的类型提供具体化的模板

**示例**：

```c++
class Person {
public:
	Person(string name, int age) {
		this->m_Name = name;
		this->age = age;
	}
	//姓名
	string m_Name;
	//年龄
	int age;

};
//对比两个数据是否相等函数
template<class T>
bool myCompare(T& a, T& b) {
	if (a == b) {
		return true;
	}
	else {
		return false;
	}
}
//利用具体化的Person的版本实现，具体化会优先调用
template<> bool myCompare(Person& p1, Person& p2) {
	if (p1.m_Name == p2.m_Name && p1.age == p2.age) {
		return true;
	}
	else {
		return false;
	}
}
```

#### 1.3类模板

##### 1.3.1类模板语法

**作用**：

建立一个通用的类，类中的成员数据类型可以不具体指定，用一个虚拟的类型来代表。

**语法**：

```C++
template<typename T>
```

**解释**：

template --- 声明创建模板

typename --- 表明其后面的符号是一种数据类型，可以用class代替

T --- 通用的数据类型，名称可以替换，通常为大写字母

**示例**：

```C++
template<class nametype,class agetype>
class Person {
public:
	Person(nametype name, agetype age) {
		this->m_name = name;
		this->m_age = age;
	}
	void showperson() {
		cout << "name= " << this->m_name << "  age= " << this->m_age << endl;
	}
	nametype m_name;
	agetype m_age;
};
void test1() {
	Person<string,int> p1("孙悟空",99);
	p1.showperson();
}
```

总结：类模板和函数模板的语法相似，在temple后写一个类就是类模板；

##### 1.3.2类模板和函数模板的区别

**区别**：

1. 类模板没有自动类型推导的使用方式
2. 类模板在模板参数列表中可以有默认参数

**示例**：

```C++
//1.不能自动推导类型
//Person= p1("孙悟空", 99);//错误
Person<string,int> p1("孙悟空",99);//正确
//2.类模板在模板参数列表中可以有默认参数
//template<class nametype = string, class agetype = int>
Person<> p1("孙悟空", 99);
```

##### 1.3.3类模板中成员函数创建时机

类模板中成员函数和普通类中成员函数创建时机式有区别的：

- 普通类中的成员函数一开始就可以创建
- 类模板中的成员函数在调用时才可以创建

**示例**：

```C++
class Person1 {
public:
	void Person1show() {
		cout << "person1" << endl;
	}
};

class Person2 {
public:
	void Person2show() {
		cout << "person2" << endl;
	}
};

template<class T>
class Person {
public:
	T obj;
	void fun1() {obj.Person1show();}
	void fun2() {obj.Person2show();}
};
void test1() {
	Person<Person1> p1;
	p1.fun1();
	p1.fun2();//错误
}
```

**总结**：

类模板中的成员函数并不是一开始就创建，而是在调用时才去创建。

##### 1.3.4类模板对象做函数参数

**学习目标**：

- 类模板实例化出的对象，向函数传参

**传入方式（三种）**：

1. 指定传入的类型 --- 直接显示对象中的数据类型
2. 参数模板化        --- 将对象中参数变为模板进行传递
3. 整个类模板化    --- 将这个对象类型  模板化进行传递

**示例**：

```C++
//1.指定传入的类型
void personshow1(Person<string, int> p1) {
	cout << "name =  " << p1.m_name << " age= " << p1.m_age << endl;
}
//2.参数模板化
template<class T1, class T2>
void personshow2(Person<T1, T2> p1) {
	cout << "name =  " << p1.m_name << " age= " << p1.m_age << endl;
}
//3.整个类模板化
template<class T>
void personshow3(T p1) {
	cout << "name =  " << p1.m_name << " age= " << p1.m_age << endl;
}
void test1() {

	Person<string,int> p1("孙悟空",99);//正确
	personshow2(p1);
}
```

**总结**：

- 通过类模板创建的对象，可以有三种方式向函数中进行传参
- 应用广泛的时第一种：指定传入的类型
