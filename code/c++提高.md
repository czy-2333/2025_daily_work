# c++提高编程



* 本阶段主要针对C++==泛型编程==和==STL==技术做详细讲解，探讨C++更深层的使用





## 1 模板

### 1.1 模板的概念



模板就是建立**通用的模具**，大大**提高复用性**



例如生活中的模板：

<img src="assets/ppt模板1.png" style="zoom:120%;" />

<img src="assets/ppt模板2.png" style="zoom:120%;" />





一寸照片模板：



![1547105026929](assets/照片模板.png)





模板的特点：

* 模板不可以直接使用，它只是一个框架
* 模板的通用并不是万能的









### 1.2 函数模板

* C++另一种编程思想称为 ==泛型编程== ，主要利用的技术就是模板


* C++提供两种模板机制:**函数模板**和**类模板** 



#### 1.2.1 函数模板语法

函数模板作用：

建立一个通用函数，其函数返回值类型和形参类型可以不具体制定，用一个**虚拟的类型**来代表。



**语法：** 

```C++
template<typename T>
函数声明或定义
```

**解释：**

template  ---  声明创建模板

typename  --- 表面其后面的符号是一种数据类型，可以用class代替

T    ---   通用的数据类型，名称可以替换，通常为大写字母



**示例：**

```C++

//交换整型函数
void swapInt(int& a, int& b) 
{
	int temp = a;
	a = b;
	b = temp;
}

//交换浮点型函数
void swapDouble(double& a, double& b)
{
	double temp = a;
	a = b;
	b = temp;
}

//利用模板提供通用的交换函数
template<typename T>
void mySwap(T& a, T& b)
{
	T temp = a;
	a = b;
	b = temp;
}

void test01()
{
	int a = 10;
	int b = 20;
	
	//swapInt(a, b);

	//利用模板实现交换
	//1、自动类型推导
	mySwap(a, b);

	//2、显示指定类型
	mySwap<int>(a, b);	//函数模板指定类型的 <> 在函数名与传入的实参之间

	cout << "a = " << a << endl;
	cout << "b = " << b << endl;

}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：

* 函数模板利用关键字 template
* 使用函数模板有两种方式：自动类型推导、显示指定类型
* 模板的目的是为了提高复用性，将类型参数化









#### 1.2.2 函数模板注意事项

注意事项：

* 自动类型推导，必须推导出一致的数据类型T,才可以使用


* 模板必须要确定出T的数据类型，才可以使用



**示例：**

```C++
//利用模板提供通用的交换函数
template<class T>
void mySwap(T& a, T& b)
{
	T temp = a;
	a = b;
	b = temp;
}


// 1、自动类型推导，必须推导出一致的数据类型T,才可以使用
void test01()
{
	int a = 10;
	int b = 20;
	char c = 'c';

	mySwap(a, b); // 正确，可以推导出一致的T
	//mySwap(a, c); // 错误，推导不出一致的T类型
}


// 2、模板必须要确定出T的数据类型，才可以使用
template<class T>
void func()
{
	cout << "func 调用" << endl;
}

void test02()
{
	//func(); //错误，模板不能独立使用，必须确定出T的类型
	func<int>(); //利用显示指定类型的方式，给T一个类型(如果不传参的话,任何类型都可以)，才可以使用该模板
}

int main()
{
	test01();
	test02();

	system("pause");

	return 0;
}
```

总结：

* 使用模板时必须确定出通用数据类型T，并且能够推导出一致的类型











#### 1.2.3 函数模板案例

案例描述：

* 利用函数模板封装一个排序的函数，可以对**不同数据类型数组**进行排序
* 排序规则从大到小，排序算法为**选择排序**
* 分别利用**char数组**和**int数组**进行测试



示例：

```C++
//交换的函数模板
template<typename T>
void mySwap(T &a, T&b)
{
	T temp = a;
	a = b;
	b = temp;
}


template<class T> // 也可以替换成typename
//利用选择排序，进行对数组从大到小的排序
void mySort(T arr[], int len)
{
	for (int i = 0; i < len; i++)
	{
		int max = i; //最大数的下标
		for (int j = i + 1; j < len; j++)
		{
			if (arr[max] < arr[j])
			{
				max = j;
			}
		}
		if (max != i) //如果最大数的下标不是i，交换两者
		{
			mySwap(arr[max], arr[i]);
		}
	}
}
template<typename T>
void printArray(T arr[], int len)
{
	for (int i = 0; i < len; i++)
    {
		cout << arr[i] << " ";
	}
	cout << endl;
}
void test01()
{
	//测试char数组
	char charArr[] = "bdcfeagh";
	int num = sizeof(charArr) / sizeof(char);
	mySort(charArr, num);
	printArray(charArr, num);
}

void test02()
{
	//测试int数组
	int intArr[] = { 7, 5, 8, 1, 3, 9, 2, 4, 6 };
	int num = sizeof(intArr) / sizeof(int);
	mySort(intArr, num);
	printArray(intArr, num);
}

int main()
{
	test01();
	test02();

	system("pause");

	return 0;
}
```

总结：模板可以提高代码复用，需要熟练掌握











#### 1.2.4 普通函数与函数模板的区别

**普通函数与函数模板区别：**

* 普通函数调用时可以发生自动类型转换（隐式类型转换）
* 函数模板调用时，如果利用自动类型推导，不会发生隐式类型转换
* 如果利用显示指定类型的方式，可以发生隐式类型转换



自动类型转换 : 在表达式计算过程中,编译器自动将操作数的类型转换为符合运算需求的类型,而无需开发者显式地进行类型转换



**示例：**

```C++
//普通函数
int myAdd01(int a, int b)
{
	return a + b;
}

//函数模板
template<class T>
T myAdd02(T a, T b)  
{
	return a + b;
}

//使用函数模板时，如果用自动类型推导，不会发生自动类型转换,即隐式类型转换
void test01()
{
	int a = 10;
	int b = 20;
	char c = 'c';
	
	cout << myAdd01(a, c) << endl; //正确，将char类型的'c'隐式转换为int类型  'c' 对应 ASCII码 99

	//myAdd02(a, c); // 报错，使用自动类型推导时，不会发生隐式类型转换

	myAdd02<int>(a, c); //正确，如果用显示指定类型，可以发生隐式类型转换
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：建议使用显示指定类型的方式，调用函数模板，因为可以自己确定通用类型T











#### 1.2.5 普通函数与函数模板的调用规则

调用规则如下：

1. 如果函数模板和普通函数都可以实现 ( 同时拥有同名的普通函数和模板函数 ) ，优先调用普通函数

2. 可以通过空模板参数列表来强制调用函数模板

    语法：` 模板函数名 <> ( 参数1 , 参数2 );`

3. 函数模板也可以发生重载

4. 如果函数模板可以产生更好的匹配,优先调用函数模板

    如果普通函数参数是   int   类型,传入的   char   类型,调用模板函数



**示例：**

```C++
//普通函数与函数模板调用规则
void myPrint(int a, int b)
{
	cout << "调用的普通函数" << endl;
}

template<typename T>
void myPrint(T a, T b) 
{ 
	cout << "调用的模板" << endl;
}

template<typename T>
void myPrint(T a, T b, T c) 
{ 
	cout << "调用重载的模板" << endl; 
}

void test01()
{
	//1、如果函数模板和普通函数都可以实现，优先调用普通函数
	// 注意 如果告诉编译器  普通函数是有的，但只是声明没有实现，或者不在当前文件内实现，就会报错找不到
	int a = 10;
	int b = 20;
	myPrint(a, b); //调用普通函数

	//2、可以通过空模板参数列表来强制调用函数模板
	myPrint<>(a, b); //调用函数模板

	//3、函数模板也可以发生重载
	int c = 30;
	myPrint(a, b, c); //调用重载的函数模板

	//4、 如果函数模板可以产生更好的匹配,优先调用函数模板
	char c1 = 'a';
	char c2 = 'b';
	myPrint(c1, c2); //调用函数模板
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：既然提供了函数模板，最好就不要提供普通函数，否则容易出现二义性











#### 1.2.6 模板的局限性

**局限性：**

* 模板的通用性并不是万能的



**例如：**

```C++
template<class T>
void f(T a, T b)
{ 
    a = b;
}
```

在上述代码中提供的赋值操作，如果传入的a和b是一个数组，就无法实现了



再例如：

```C++
template<class T>
void f(T a, T b)
{ 
	if(a > b) { ... }
}
```

在上述代码中，如果T的数据类型传入的是像Person这样的自定义数据类型，也无法正常运行



因此C++为了解决这种问题，提供模板的重载，可以为这些**特定的类型**提供**具体化的模板**



**示例：**

```C++
#include<iostream>
using namespace std;

#include <string>

class Person
{
public:
	Person(string name, int age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}
	string m_Name;
	int m_Age;
};

//普通函数模板
template<class T>
bool myCompare(T& a, T& b)
{
	if (a == b)
	{
		return true;
	}
	else
	{
		return false;
	}
}


//具体化，显示具体化的原型和定意思以template<>开头，并通过名称来指出类型
//具体化优先于常规模板
template<> bool myCompare(Person &p1, Person &p2)
{
	if ( p1.m_Name  == p2.m_Name && p1.m_Age == p2.m_Age)
	{
		return true;
	}
	else
	{
		return false;
	}
}

void test01()
{
	int a = 10;
	int b = 20;
	//内置数据类型可以直接使用通用的函数模板
	bool ret = myCompare(a, b);
	if (ret)
	{
		cout << "a == b " << endl;
	}
	else
	{
		cout << "a != b " << endl;
	}
}

void test02()
{
	Person p1("Tom", 10);
	Person p2("Tom", 10);
	//自定义数据类型，不会调用普通的函数模板
	//可以创建具体化的Person数据类型的模板，用于特殊处理这个类型
	bool ret = myCompare(p1, p2);
	if (ret)
	{
		cout << "p1 == p2 " << endl;
	}
	else
	{
		cout << "p1 != p2 " << endl;
	}
}

int main()
{
	test01();

	test02();

	system("pause");

	return 0;
}
```

总结：

* 利用具体化的模板，可以解决自定义类型的通用化
* 学习模板并不是为了写模板，而是在STL能够运用系统提供的模板









### 1.3 类模板

#### 1.3.1 类模板语法

类模板作用：

* 建立一个通用类，类中的成员 数据类型可以不具体制定，用一个**虚拟的类型**来代表。



**语法：** 

```c++
template<typename T>
类
```

**解释：**

template  ---  声明创建模板

typename  --- 表面其后面的符号是一种数据类型，可以用   ==class==   代替

T    ---   通用的数据类型，名称可以替换，通常为大写字母



**示例：**

```C++
#include <string>
//类模板
template<class NameType, class AgeType> 
class Person
{
public:
	Person(NameType name, AgeType age)
	{
		this->mName = name;
		this->mAge = age;
	}
	void showPerson()
	{
		cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};

void test01()
{
	// 指定NameType 为string类型，AgeType 为 int类型
	Person<string, int>P1("孙悟空", 999);	//类模板指定类型的 <> 在类型名和变量名之间
	P1.showPerson();
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：类模板和函数模板语法相似，在声明模板template后面加类，此类称为类模板











#### 1.3.2 类模板与函数模板区别

类模板与函数模板区别主要有两点：

1. 类模板没有自动类型推导的使用方式
2. 类模板在模板参数列表中可以有默认参数 ( 给模板参数类型,设置默认类型 )




**示例：**

```C++
#include <string>
//类模板
template<class NameType, class AgeType = int>	//设置AgeType的默认类型为int,且AgeType只能传入int类型的值,或者被强制转换为int类型
class Person
{
public:
	Person(NameType name, AgeType age)
	{
		this->mName = name;
		this->mAge = age;
	}
	void showPerson()
	{
		cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};

//1、类模板没有自动类型推导的使用方式
void test01()
{
	// Person p("孙悟空", 1000); // 错误 类模板使用时候，不可以用自动类型推导
	Person <string ,int>p("孙悟空", 1000); //必须使用显示指定类型的方式，使用类模板
	p.showPerson();
}

//2、类模板在模板参数列表中可以有默认参数
void test02()
{
	Person <string> p("猪八戒", 999); //类模板中的模板参数列表 可以指定默认参数类型
	p.showPerson();
}

int main()
{
	test01();

	test02();

	system("pause");

	return 0;
}
```

总结：

* 类模板使用只能用显示指定类型方式
* 类模板中的模板参数列表可以有默认参数











#### 1.3.3 类模板中成员函数创建时机

类模板中成员函数和普通类中成员函数创建时机是有区别的：

* 普通类中的成员函数一开始就可以创建
* 类模板中的成员函数在调用时才创建





**示例：**

```C++
class Person1
{
public:
	void showPerson1()
	{
		cout << "Person1 show" << endl;
	}
};

class Person2
{
public:
	void showPerson2()
	{
		cout << "Person2 show" << endl;
	}
};

template<class T>
class MyClass
{
public:
	T obj;

	//类模板中的成员函数，并不是一开始就创建的，而是在模板调用时再生成

	void fun1() { obj.showPerson1(); }
	void fun2() { obj.showPerson2(); }

};

void test01()
{
	MyClass<Person1> m;
	
	m.fun1();

	//m.fun2();//编译会出错，说明函数调用才会去创建成员函数
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：类模板中的成员函数并不是一开始就创建的，在调用时才去创建









#### 1.3.4 类模板对象做函数参数

学习目标：

* 类模板实例化出的对象，向函数传参的方式



一共有三种传入方式：

1. 指定传入的类型   --- 直接显示对象的数据类型
2. 参数模板化           --- 将对象中的参数变为模板进行传递
3. 整个类模板化       --- 将这个对象类型 模板化进行传递





**示例：**

```C++
#include <string>
//类模板
template<class NameType, class AgeType = int> 
class Person
{
public:
	Person(NameType name, AgeType age)
	{
		this->mName = name;
		this->mAge = age;
	}
	void showPerson()
	{
		cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};

//1、指定传入的类型
void printPerson1(Person<string, int> &p)	//在函数形参处指定传入类型
{
	p.showPerson();
}
void test01()
{
	Person <string, int >p("孙悟空", 100);
	printPerson1(p);
}

//2、参数模板化
template <class T1, class T2>
void printPerson2(Person<T1, T2>&p)	//函数模板,用模板给对象的模板类型传参
{
	p.showPerson();
	cout << "T1的类型为： " << typeid(T1).name() << endl;
	cout << "T2的类型为： " << typeid(T2).name() << endl;
}
void test02()
{
	Person <string, int >p("猪八戒", 90);
	printPerson2(p);
}

//3、整个类模板化
template<class T>
void printPerson3(T & p)	//函数模板,直接传入模板,编译器自动识别类型
{
	cout << "T的类型为： " << typeid(T).name() << endl;
	p.showPerson();

}
void test03()
{
	Person <string, int >p("唐僧", 30);
	printPerson3(p);
}

int main() {

	test01();
	test02();
	test03();

	system("pause");

	return 0;
}
```

总结：

* 通过类模板创建的对象，可以有三种方式向函数中进行传参
* 使用比较广泛是第一种：指定传入的类型









#### 1.3.5 类模板与继承

当类模板碰到继承时，需要注意一下几点：

* 当子类继承的父类是一个类模板时，子类在声明的时候，要指定出父类中T的类型
* 如果不指定，编译器无法给子类分配内存
* 如果想灵活指定出父类中T的类型，子类也需变为类模板




**示例：**

```C++
template<class T>
class Base
{
	T m;
};

//class Son:public Base  //错误，c++编译需要给子类分配内存，必须知道父类中T的类型才可以向下继承
class Son1 :public Base<int> //必须为父类的模板指定一个类型
{};

void test01()
{
	Son1 c;
}

//类模板继承类模板 ,可以用T2指定父类中的T类型
template<class T1, class T2>	//T1模板类型对应自身的成员,T2模板类型对应继承的父类成员
class Son2 :public Base<T2>
{
public:
    T1 obj;
	Son2()
	{
		cout << typeid(T1).name() << endl;
		cout << typeid(T2).name() << endl;
	}
};

void test02()
{
	Son2<int, char> child1;
}


int main()
{
	test01();

	test02();

	system("pause");

	return 0;
}
```

总结：如果父类是类模板，子类需要指定出父类中T的数据类型









#### 1.3.6 类模板成员函数类外实现

学习目标：能够掌握类模板中的成员函数类外实现



**示例：**

```C++
#include <string>

//类模板中成员函数类外实现
template<class T1, class T2>
class Person
{
public:
	//成员函数类内声明
	Person(T1 name, T2 age);
	void showPerson();

public:
	T1 m_Name;
	T2 m_Age;
};

//构造函数 类外实现
template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age)
{
	this->m_Name = name;
	this->m_Age = age;
}

//成员函数 类外实现
template<class T1, class T2>	//设置模板变量,可以和模板实例不同
void Person<T1, T2>::showPerson()
{
	cout << "姓名: " << this->m_Name << " 年龄:" << this->m_Age << endl;
}
//类外实现都需要在::前加<T1,T2>,让编译器需要知道具体使用的是哪个模板实例,对应之前的模板类

void test01()
{
	Person<string, int> p("Tom", 20);
	p.showPerson();
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：类模板中成员函数类外实现时，需要加上模板参数列表









#### 1.3.7 类模板分文件编写

学习目标：

* 掌握类模板成员函数分文件编写产生的问题以及解决方式



问题：将类的声名写 .h 文件中,定义写 .cpp 文件中，类模板中成员函数创建时机是在调用阶段，导致分文件编写时链接不到


解决：

* 解决方式1：直接包含.cpp源文件
* 解决方式2：将声明和实现写到同一个文件中，并更改后缀名为.hpp，意思是类模板头文件，hpp是约定的名称，并不是强制




**示例：**

person.hpp中代码：

```C++
#pragma once
#include <iostream>
using namespace std;
#include <string>

template<class T1, class T2>
class Person
{
public:
	Person(T1 name, T2 age);
	void showPerson();
public:
	T1 m_Name;
	T2 m_Age;
};

//构造函数 类外实现
template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age) {
	this->m_Name = name;
	this->m_Age = age;
}

//成员函数 类外实现
template<class T1, class T2>
void Person<T1, T2>::showPerson() {
	cout << "姓名: " << this->m_Name << " 年龄:" << this->m_Age << endl;
}
```



类模板分文件编写.cpp中代码

```C++
#include<iostream>
using namespace std;

//#include "person.h"
#include "person.cpp" //解决方式1，包含cpp源文件

//解决方式2，将声明和实现写到一起，文件后缀名改为.hpp
#include "person.hpp"
void test01()
{
	Person<string, int> p("Tom", 10);
	p.showPerson();
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：主流的解决方式是第二种，将类模板成员函数写到一起，并将后缀名改为.hpp









#### 1.3.8 类模板与友元

学习目标：

* 掌握类模板配合友元函数的类内和类外实现



全局函数类内实现 - 直接在类内声明友元即可

全局函数类外实现 - 需要提前让编译器知道全局函数的存在



**示例：**

```C++
#include <string>

//2、全局函数配合友元  类外实现 - 先做函数模板声明，下方在做函数模板定义，在做友元
template<class T1, class T2>
class Person;	//需要先定义模板类,然后才能使用以Person模板类为参数的全局函数

//如果声明了函数模板，可以将实现写到后面，否则需要将实现体写到类的前面让编译器提前看到
//template<class T1, class T2> void printPerson2(Person<T1, T2> & p); 

template<class T1, class T2>
void printPerson2(Person<T1, T2> & p)	//参数模板化,传入一个模板类的实参
{
	cout << "类外实现 ---- 姓名： " << p.m_Name << " 年龄：" << p.m_Age << endl;
}

template<class T1, class T2>
class Person
{
	//1、全局函数配合友元   类内实现
	friend void printPerson(Person<T1, T2> & p)
	{
		cout << "姓名： " << p.m_Name << " 年龄：" << p.m_Age << endl;
	}

	//全局函数配合友元  类外实现
	friend void printPerson2<>(Person<T1, T2> & p);	//若不加<>,该函数只是一个形参为模板类的普通函数,加<>告诉编译器该函数为模板函数

public:

	Person(T1 name, T2 age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}


private:
	T1 m_Name;
	T2 m_Age;

};

//1、全局函数在类内实现
void test01()
{
	Person <string, int >p("Tom", 20);
	printPerson(p);
}


//2、全局函数在类外实现
void test02()
{
	Person <string, int >p("Jerry", 30);
	printPerson2(p);
}

int main()
{
	//test01();

	test02();

	system("pause");

	return 0;
}
```

总结：建议全局函数做类内实现，用法简单，而且编译器可以直接识别











#### 1.3.9 类模板案例

案例描述:  实现一个通用的数组类，要求如下：



* 可以对内置数据类型以及自定义数据类型的数据进行存储
* 将数组中的数据存储到堆区
* 构造函数中可以传入数组的容量
* 提供对应的拷贝构造函数以及operator=防止浅拷贝问题
* 提供尾插法和尾删法对数组中的数据进行增加和删除
* 可以通过下标的方式访问数组中的元素
* 可以获取数组中当前元素个数和数组的容量





**示例：**

myArray.hpp中代码

```C++
#pragma once
#include <iostream>
using namespace std;

template<class T>
class MyArray
{
public:
    
	//构造函数
	MyArray(int capacity)
	{
		this->m_Capacity = capacity;
		this->m_Size = 0;
		pAddress = new T[this->m_Capacity];
	}

	//拷贝构造
	MyArray(const MyArray & arr)
	{
		this->m_Capacity = arr.m_Capacity;
		this->m_Size = arr.m_Size;
		this->pAddress = new T[this->m_Capacity];
		for (int i = 0; i < this->m_Size; i++)
		{
			//如果T为对象，而且还包含指针，必须需要重载 = 操作符，因为这个等号不是 构造 而是赋值，
			// 普通类型可以直接= 但是指针类型需要深拷贝
			this->pAddress[i] = arr.pAddress[i];
		}
	}

	//重载= 操作符  防止浅拷贝问题
	MyArray& operator=(const MyArray& myarray) {

		if (this->pAddress != NULL) {
			delete[] this->pAddress;
			this->m_Capacity = 0;
			this->m_Size = 0;
		}

		this->m_Capacity = myarray.m_Capacity;
		this->m_Size = myarray.m_Size;
		this->pAddress = new T[this->m_Capacity];
		for (int i = 0; i < this->m_Size; i++) {
			this->pAddress[i] = myarray[i];
		}
		return *this;
	}

	//重载[] 操作符  arr[0]
	T& operator [](int index)
	{
		return this->pAddress[index]; //不考虑越界，用户自己去处理
	}

	//尾插法
	void Push_back(const T & val)
	{
		if (this->m_Capacity == this->m_Size)
		{
			return;
		}
		this->pAddress[this->m_Size] = val;
		this->m_Size++;
	}

	//尾删法
	void Pop_back()
	{
		if (this->m_Size == 0)
		{
			return;
		}
		this->m_Size--;
	}

	//获取数组容量
	int getCapacity()
	{
		return this->m_Capacity;
	}

	//获取数组大小
	int	getSize()
	{
		return this->m_Size;
	}


	//析构
	~MyArray()
	{
		if (this->pAddress != NULL)
		{
			delete[] this->pAddress;
			this->pAddress = NULL;
			this->m_Capacity = 0;
			this->m_Size = 0;
		}
	}

private:
	T * pAddress;  //指向一个堆空间，这个空间存储真正的数据
	int m_Capacity; //容量
	int m_Size;   // 大小
};
```



类模板案例—数组类封装.cpp中

```C++
#include "myArray.hpp"
#include <string>

void printIntArray(MyArray<int>& arr) {
	for (int i = 0; i < arr.getSize(); i++) {
		cout << arr[i] << " ";
	}
	cout << endl;
}

//测试内置数据类型
void test01()
{
	MyArray<int> array1(10);
	for (int i = 0; i < 10; i++)
	{
		array1.Push_back(i);
	}
	cout << "array1打印输出：" << endl;
	printIntArray(array1);
	cout << "array1的大小：" << array1.getSize() << endl;
	cout << "array1的容量：" << array1.getCapacity() << endl;

	cout << "--------------------------" << endl;

	MyArray<int> array2(array1);
	array2.Pop_back();
	cout << "array2打印输出：" << endl;
	printIntArray(array2);
	cout << "array2的大小：" << array2.getSize() << endl;
	cout << "array2的容量：" << array2.getCapacity() << endl;
}

//测试自定义数据类型
class Person {
public:
	Person() {} 
		Person(string name, int age) {
		this->m_Name = name;
		this->m_Age = age;
	}
public:
	string m_Name;
	int m_Age;
};

void printPersonArray(MyArray<Person>& personArr)
{
	for (int i = 0; i < personArr.getSize(); i++) {
		cout << "姓名：" << personArr[i].m_Name << " 年龄： " << personArr[i].m_Age << endl;
	}

}

void test02()
{
	//创建数组
	MyArray<Person> pArray(10);
	Person p1("孙悟空", 30);
	Person p2("韩信", 20);
	Person p3("妲己", 18);
	Person p4("王昭君", 15);
	Person p5("赵云", 24);

	//插入数据
	pArray.Push_back(p1);
	pArray.Push_back(p2);
	pArray.Push_back(p3);
	pArray.Push_back(p4);
	pArray.Push_back(p5);

	printPersonArray(pArray);

	cout << "pArray的大小：" << pArray.getSize() << endl;
	cout << "pArray的容量：" << pArray.getCapacity() << endl;

}

int main() {

	//test01();

	test02();

	system("pause");

	return 0;
}
```

总结：

能够利用所学知识点实现通用的数组





## 2 STL初识

### 2.1 STL的诞生

* 长久以来，软件界一直希望建立一种可重复利用的东西

* C++的**面向对象**和**泛型编程**思想，目的就是**复用性的提升**

* 大多情况下，数据结构和算法都未能有一套标准,导致被迫从事大量重复工作

* 为了建立数据结构和算法的一套标准,诞生了**STL**

  


### 2.2 STL基本概念

* STL(Standard Template Library,**标准模板库**)
* STL 从广义上分为: **容器(container) 算法(algorithm) 迭代器(iterator)**
* **容器**和**算法**之间通过**迭代器**进行无缝连接。
* STL 几乎所有的代码都采用了模板类或者模板函数





### 2.3 STL六大组件

STL大体分为六大组件，分别是:**容器、算法、迭代器、仿函数、适配器（配接器）、空间配置器**



1. 容器：各种数据结构，如vector、list、deque、set、map等,用来存放数据。
2. 算法：各种常用的算法，如sort、find、copy、for_each等
3. 迭代器：扮演了容器与算法之间的胶合剂。
4. 仿函数：行为类似函数，可作为算法的某种策略。
5. 适配器：一种用来修饰容器或者仿函数或迭代器接口的东西。
6. 空间配置器：负责空间的配置与管理。





### 2.4  STL中容器、算法、迭代器

**容器：**置物之所也

STL**容器**就是将运用**最广泛的一些数据结构**实现出来

常用的数据结构：数组, 链表,树, 栈, 队列, 集合, 映射表 等

这些容器分为**序列式容器**和**关联式容器**两种:

​			**序列式容器：**强调值的排序，序列式容器中的每个元素均有固定的位置
​			**关联式容器：**二叉树结构，各元素之间没有严格的物理上的顺序关系



**算法：**问题之解法也

有限的步骤，解决逻辑或数学上的问题，这一门学科我们叫做算法(Algorithms)

算法分为：**质变算法**和**非质变算法**。

质变算法：是指运算过程中会更改区间内的元素的内容。例如拷贝，替换，删除等等

非质变算法：是指运算过程中不会更改区间内的元素内容，例如查找、计数、遍历、寻找极值等等



**迭代器：**容器和算法之间粘合剂

提供一种方法，使之能够依序寻访某个容器所含的各个元素，而又无需暴露该容器的内部表示方式

每个容器都有自己专属的迭代器

迭代器使用非常类似于指针，初学阶段我们可以先理解迭代器为指针



迭代器种类：

| 种类           | 功能                                                     | 支持运算                                |
| -------------- | -------------------------------------------------------- | --------------------------------------- |
| 输入迭代器     | 对数据的只读访问                                         | 只读，支持++、==、！=                   |
| 输出迭代器     | 对数据的只写访问                                         | 只写，支持++                            |
| 前向迭代器     | 读写操作，并能向前推进迭代器                             | 读写，支持++、==、！=                   |
| 双向迭代器     | 读写操作，并能向前和向后操作                             | 读写，支持++、--，                      |
| 随机访问迭代器 | 读写操作，可以以跳跃的方式访问任意数据，功能最强的迭代器 | 读写，支持++、--、[n]、-n、<、<=、>、>= |

常用的容器中迭代器种类为双向迭代器，和随机访问迭代器







### 2.5 容器算法迭代器初识

了解STL中容器、算法、迭代器概念之后，我们利用代码感受STL的魅力

STL中最常用的容器为Vector，可以理解为数组，下面我们将学习如何向这个容器中插入数据、并遍历这个容器



#### 2.5.1 vector存放内置数据类型

容器： `vector`	数组类型的容器，通过 `<typename>` 设置模板类型

算法： `for_each`	遍历算法

​				头文件是	`<algorithm>`

​				三个参数分别为：遍历的起始位置，遍历的结束位置，遍历的回调函数

迭代器： `vector<int>::iterator`	一个指向容器元素的指针



**示例：**

```C++
#include <vector>
#include <algorithm>	//STL 标准算法的头文件

void MyPrint(int val)
{
	cout << val << endl;
}

void test01()
{
	//创建vector容器对象,并且通过模板参数指定容器中存放的数据的类型
	vector<int> v;
    
	//向容器中放数据,将数据插入到数组的末尾
	v.push_back(10);
	v.push_back(20);
	v.push_back(30);
	v.push_back(40);

	//每一个容器都有自己的迭代器，迭代器是用来遍历容器中的元素
	//v.begin()返回迭代器，这个迭代器指向容器中第一个数据
	//v.end()返回迭代器，这个迭代器指向容器元素的最后一个元素的下一个位置
	//vector<int>::iterator 拿到vector<int>这种容器的迭代器类型

	vector<int>::iterator pBegin = v.begin();
	vector<int>::iterator pEnd = v.end();

	//第一种遍历方式：
	while (pBegin != pEnd)
    {
		cout << *pBegin << endl;
		pBegin++;
	}

	
	//第二种遍历方式：
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
    {
		cout << *it << endl;
	}
	cout << endl;

	//第三种遍历方式：
	//使用STL提供标准遍历算法  头文件 algorithm
	for_each(v.begin(), v.end(), MyPrint);
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```



#### 2.5.2 Vector存放自定义数据类型



学习目标：vector中存放自定义数据类型，并打印输出



**示例：**

```c++
#include <vector>
#include <string>

//自定义数据类型
class Person
{
public:
	Person(string name, int age)
    {
		mName = name;
		mAge = age;
	}
public:
	string mName;
	int mAge;
};

//存放对象
void test01()
{
	vector<Person> v;

	//创建数据
	Person p1("aaa", 10);
	Person p2("bbb", 20);
	Person p3("ccc", 30);
	Person p4("ddd", 40);
	Person p5("eee", 50);

	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	v.push_back(p5);

	for (vector<Person>::iterator it = v.begin(); it != v.end(); it++)
    {
		cout << "Name:" << (*it).mName << " Age:" << (*it).mAge << endl;
	}
}


//放对象指针,优点是省内存
void test02()
{
	vector<Person*> v;

	//创建数据
	Person p1("aaa", 10);
	Person p2("bbb", 20);
	Person p3("ccc", 30);
	Person p4("ddd", 40);
	Person p5("eee", 50);

	v.push_back(&p1);
	v.push_back(&p2);
	v.push_back(&p3);
	v.push_back(&p4);
	v.push_back(&p5);

	for (vector<Person*>::iterator it = v.begin(); it != v.end(); it++)
    {
		Person * p = (*it);
		cout << "Name:" << p->mName << " Age:" << (*it)->mAge << endl;
	}
}


int main()
{
	test01();
    
	test02();

	system("pause");

	return 0;
}
```



#### 2.5.3 Vector容器嵌套容器



学习目标：容器中嵌套容器，我们将所有数据进行遍历输出



**示例：**

```C++
#include <vector>

//容器嵌套容器
void test01()
{
	vector< vector<int> >  v;	//相当于一个二维数组

	vector<int> v1;
	vector<int> v2;
	vector<int> v3;
	vector<int> v4;

	for (int i = 0; i < 4; i++)
    {
		v1.push_back(i + 1);
		v2.push_back(i + 2);
		v3.push_back(i + 3);
		v4.push_back(i + 4);
	}

	//将容器元素插入到vector v中
	v.push_back(v1);
	v.push_back(v2);
	v.push_back(v3);
	v.push_back(v4);


	for (vector<vector<int>>::iterator it = v.begin(); it != v.end(); it++)
    {
		for (vector<int>::iterator vit = (*it).begin(); vit != (*it).end(); vit++)
        {
			cout << *vit << " ";
		}
		cout << endl;
	}
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```





## 3 STL- 常用容器

### 3.1 string 字符串



#### 3.1.1 string基本概念

**本质：**

`string` 是C++风格的字符串，而string本质上是一个类



**string和char * 区别：**

* char * 是一个指针
* string是一个类，类内部封装了char\*，管理这个字符串，是一个char*型的容器。



**特点：**

string 类内部封装了很多成员方法

例如：查找find，拷贝copy，删除delete 替换replace，插入insert

string管理char*所分配的内存，不用担心复制越界和取值越界等，由类内部进行负责



#### 3.1.2 string构造函数

**原型：**

```C++
string();					//创建一个空的字符串 例如: string str;
string(const char* s);	 	//使用字符串s初始化
string(const string& str);	//使用一个string对象初始化另一个string对象
string(int n, char c);		//使用n个字符c初始化 
```





**示例：**

```C++
#include <string>
//string构造
void test01()
{
	string s1; //创建空字符串，调用无参构造函数
	cout << "str1 = " << s1 << endl;

	const char* str = "hello world";
	string s2(str); //把c_string转换成了string

	cout << "str2 = " << s2 << endl;

	string s3(s2); //调用拷贝构造函数
	cout << "str3 = " << s3 << endl;

	string s4(10, 'a');
	cout << "str3 = " << s3 << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：string的多种构造方式没有可比性，灵活使用即可









#### 3.1.3 string赋值操作

**功能：**

给 `string` 字符串进行赋值





**函数原型：**


```C++
string& operator=(const char* s);		//char*类型字符串 赋值给当前的字符串
string& operator=(const string &s);		//把字符串s赋给当前的字符串
string& operator=(char c);				//字符赋值给当前的字符串
string& assign(const char *s);			//把字符串s赋给当前的字符串
string& assign(const char *s, int n);	//把字符串s的前n个字符赋给当前的字符串
string& assign(const string &s);		//把字符串s赋给当前字符串
string& assign(int n, char c);			//用n个字符c赋给当前字符串
```





**示例：**

```C++
//赋值
void test01()
{
	string str1;
	str1 = "hello world";
	cout << "str1 = " << str1 << endl;

	string str2;
	str2 = str1;
	cout << "str2 = " << str2 << endl;

	string str3;
	str3 = 'a';
	cout << "str3 = " << str3 << endl;

	string str4;
	str4.assign("hello c++");
	cout << "str4 = " << str4 << endl;

	string str5;
	str5.assign("hello c++",5);
	cout << "str5 = " << str5 << endl;


	string str6;
	str6.assign(str5);
	cout << "str6 = " << str6 << endl;

	string str7;
	str7.assign(5, 'x');
	cout << "str7 = " << str7 << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：string的赋值方式很多，`operator=`  这种方式是比较实用的









#### 3.1.4 string字符串拼接

**功能：**

实现在字符串末尾拼接字符串





**函数原型：**


```C++
string& operator+=(const char* str);				//重载+=操作符
string& operator+=(const char c);					//重载+=操作符
string& operator+=(const string& str);				//重载+=操作符
string& append(const char *s);						//把字符串s连接到当前字符串结尾
string& append(const char *s, int n);				//把字符串s的前n个字符连接到当前字符串结尾
string& append(const string &s);					//同operator+=(const string& str)
string& append(const string &s, int pos, int n);	//字符串s中从pos下标开始的n个字符连接到字符串结尾
```





**示例：**


```C++
//字符串拼接
void test01()
{
	string str1 = "我";

	str1 += "爱玩游戏";

	cout << "str1 = " << str1 << endl;
	
	str1 += ':';

	cout << "str1 = " << str1 << endl;

	string str2 = "LOL DNF";

	str1 += str2;

	cout << "str1 = " << str1 << endl;

	string str3 = "I";
    
	str3.append(" love ");
    
	str3.append("game abcde", 4);
    
	//str3.append(str2);
	str3.append(str2, 4, 3); // 从下标4位置开始 ，截取3个字符，拼接到字符串末尾
    
	cout << "str3 = " << str3 << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：字符串拼接的重载版本很多，初学阶段记住几种即可







#### 3.1.5 string查找和替换

**功能：**

* 查找：查找指定字符串是否存在
* 替换：在指定的位置替换字符串



**函数原型：**

```C++
int find(const string& str, int pos = 0) const;		//查找str第一次出现位置,从pos开始查找
int find(const char* s, int pos = 0) const;			//查找s第一次出现位置,从pos开始查找
int find(const char* s, int pos, int n) const;		//从pos位置查找s的前n个字符第一次位置
int find(const char c, int pos = 0) const;			//查找字符c第一次出现位置
int rfind(const string& str, int pos = npos) const;	//查找str最后一次位置,从pos开始查找
int rfind(const char* s, int pos = npos) const;		//查找s最后一次出现位置,从pos开始查找
int rfind(const char* s, int pos, int n) const;		//从pos查找s的前n个字符最后一次位置
int rfind(const char c, int pos = 0) const;			//查找字符c最后一次出现位置
string& replace(int pos, int n, const string& str);	//替换从pos开始n个字符为字符串str
string& replace(int pos, int n,const char* s);		//替换从pos开始的n个字符为字符串s
```






**示例：**

```C++
//查找和替换
void test01()
{
	//查找
	string str1 = "abcdefgde";

	int pos = str1.find("de");

	if (pos == -1)
	{
		cout << "未找到" << endl;
	}
	else
	{
		cout << "pos = " << pos << endl;
	}

    //std::string::npos 表示找到无效位置
    if(str1.find("de")!=string::npos)
    {
        cout<<"找到有效位置"<<endl;
    }
    else
    {
        cout<<"找到无效位置"<<endl;
    }

	pos = str1.rfind("de");

	cout << "pos = " << pos << endl;
}

void test02()
{
	//替换
	string str1 = "abcdefgde";
	str1.replace(1, 3, "1111");

	cout << "str1 = " << str1 << endl;
}

int main()
{
	//test01();
	//test02();

	system("pause");

	return 0;
}
```

总结：

* find查找是从左往后，rfind从右往左
* find找到字符串后返回查找的第一个字符位置，找不到返回-1
* replace在替换时，要指定从哪个位置起，多少个字符，替换成什么样的字符串，替换的字符串和被替换的字符串长度可以不相同
















####    3.1.6 string字符串比较

**功能：**

字符串之间的比较



**比较方式：**

* 字符串比较是按字符的ASCII码进行对比
* `=` 返回   0
* `>` 返回   1 
* `<` 返回  -1





**函数原型：**

```c++
//与字符串s比较
int compare(const string &s) const;
int compare(const char *s) const;

//重载了 == 和 != 运算符,大部分容器都可以使用该运算符进行比较,但没有必要
bool operator==(const string& lhs, const string& rhs);
bool operator==(const string& lhs, const char *rhs);
bool operator!=(const string& lhs, const string& rhs);
bool operator!=(const string& lhs, const char *rhs);
```





**示例：**

```C++
//字符串比较
void test01()
{

	string s1 = "hello";
	string s2 = "aello";

	int ret = s1.compare(s2);

	if (ret == 0)
		cout << "s1 等于 s2" << endl;
	else if (ret > 0)
		cout << "s1 大于 s2" << endl;
	else
		cout << "s1 小于 s2" << endl;
    
    if(s1==s2)
        cout << "s1 与 s2 相等" << endl;
    if(s1!=s2)
        cout << "s1 与 s2 不等" << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：字符串对比主要是用于比较两个字符串是否相等，判断谁大谁小的意义并不是很大





#### 3.1.7 string字符存取(访问)

`string` 中单个字符存取方式有两种





**函数原型：**

```c++
char& operator[](int n);		//通过[]方式取字符
char& at(int n);				//通过at方法获取字符
```





**示例：**

```C++
void test01()
{
	string str = "hello world";

	for (int i = 0; i < str.size(); i++)
	{
		cout << str[i] << " ";
	}
	cout << endl;

	for (int i = 0; i < str.size(); i++)
	{
		cout << str.at(i) << " ";
	}
	cout << endl;


	//字符修改
	str[0] = 'x';
	str.at(1) = 'x';
	cout << str << endl;
	
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：string字符串中单个字符存取有两种方式，利用 [ ] 或 at









#### 3.1.8 string插入和删除

**功能：**对 `string` 字符串进行插入和删除字符操作

**函数原型：**

```c++
string& insert(int pos, const char* s);		   //插入字符串
string& insert(int pos, const string& str);	   //插入字符串
string& insert(int pos, int n, char c);		   //在指定位置插入n个字符c
string& erase(int pos, int n = npos);		   //删除从Pos开始的n个字符 
```







**示例：**

```C++
//字符串插入和删除
void test01()
{
	string str = "hello";
	str.insert(1, "111");
	cout << str << endl;

	str.erase(1, 3);  //从1号位置开始3个字符
	cout << str << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：**插入和删除的起始下标都是从0开始











#### 3.1.9 string子串

**功能：**

从字符串中获取想要的子串



**函数原型：**

```c++
string substr(int pos = 0, int n = npos) const;		//返回由pos开始的n个字符组成的字符串
```






**示例：**

```C++
//子串
void test01()
{
	string str = "abcdefg";
	string subStr = str.substr(1, 3);
	cout << "subStr = " << subStr << endl;

	string email = "hello@sina.com";
	int pos = email.find("@");
	string username = email.substr(0, pos);
	cout << "username: " << username << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：**灵活的运用求子串功能，可以在实际开发中获取有效的信息







### 3.2 vector 动态数组



#### 3.2.1 vector基本概念

功能：vector数据结构和**数组非常相似**，也称为**单端数组**



**vector与普通数组区别：**

* 不同之处在于数组是静态空间，而vector可以**动态扩展**



**动态扩展：**

* 并不是在原空间之后续接新空间，而是找更大的内存空间，然后将原数据拷贝新空间，释放原空间



![说明: 2015-11-10_151152](assets/向量容器.jpg)



* vector容器的迭代器是支持随机访问的迭代器





#### 3.2.2 vector构造函数



**功能描述：**

* 创建vector容器



**函数原型：**

```c++
vector<T> v;					//采用模板实现类实现，默认构造函数
vector<T> v(n);					//初始化一个有 n 个元素的vector对象,每个元素都为默认值

vector(v.begin(), v.end());		//将v[begin(), end())区间中的元素拷贝给本身
vector(n, elem);				//构造函数将n个elem拷贝给本身
vector(const vector &vec);		//拷贝构造函数
```




**示例：**


```C++
#include <vector>

void printVector(vector<int>& v)
{
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
    {
		cout << *it << " ";
	}
	cout << endl;
}

void test01()
{
	vector<int> v1; 	//无参构造
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);

	vector<int> v2(v1.begin(), v1.end());
	printVector(v2);

	vector<int> v3(10, 100);
	printVector(v3);
	
	vector<int> v4(v3);
	printVector(v4);
    
    //初始化矩阵
    vector<vector<int>> v5(rows, vector<int>(cols, 0));
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：**vector的多种构造方式没有可比性，灵活使用即可









#### 3.2.3 vector赋值操作



**功能描述：**

* 给vector容器进行赋值



**函数原型：**

```c++
vector& operator=(const vector &vec);	//重载等号操作符
assign(beg, end);	//将[beg, end)区间中的数据拷贝赋值给本身。
assign(n, elem);	//将n个elem拷贝赋值给本身。
```



**示例：**

```C++
#include <vector>

void printVector(vector<int>& v) {

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//赋值操作
void test01()
{
	vector<int> v1; //无参构造
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);

	vector<int>v2;
	v2 = v1;
	printVector(v2);

	vector<int>v3;
	v3.assign(v1.begin(), v1.end());
	printVector(v3);

	vector<int>v4;
	v4.assign(10, 100);
	printVector(v4);
}

int main()
{
	test01();

	system("pause");

	return 0;
}

```

总结： vector赋值方式比较简单，使用operator=，或者assign都可以







#### 3.2.4  vector容量和大小

**功能描述：**

* 对vector容器的容量和大小操作



**函数原型：**

```c++
empty();		//判断容器是否为空
capacity();		//容器的容量,可以理解为数组现在的长度
size();			//返回容器中元素的个数

resize(int num);
//重新指定容器的长度为num, 若容器变长, 则以默认值填充新位置
//如果容器变短，则末尾超出容器长度的元素被删除。

resize(int num, elem);
//重新指定容器的长度为num, 若容器变长, 则以elem值填充新位置
//如果容器变短, 则末尾超出容器长度的元素被删除
```






**示例：**


```C++
#include <vector>

void printVector(vector<int>& v)
{
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test01()
{
	vector<int> v1;
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);
	if (v1.empty())
	{
		cout << "v1为空" << endl;
	}
	else
	{
		cout << "v1不为空" << endl;
		cout << "v1的容量 = " << v1.capacity() << endl;
		cout << "v1的大小 = " << v1.size() << endl;
	}

	//resize 重新指定大小 ，若指定的更大，默认用0填充新位置，可以利用重载版本替换默认填充
	v1.resize(15,10);
	printVector(v1);

	//resize 重新指定大小 ，若指定的更小，超出部分元素被删除
	v1.resize(5);
	printVector(v1);
}

int main()
{
	test01();

	system("pause");

	return 0;
}

```

总结：

* 判断是否为空  --- empty
* 返回元素个数  --- size
* 返回容器容量  --- capacity
* 重新指定大小  ---  resize













#### 3.2.5 vector插入和删除

**功能描述：**

* 对vector容器进行插入、删除操作



**函数原型：**

```c++
push_back(ele);		//尾部插入元素ele
pop_back();			//删除最后一个元素
insert(const_iterator pos, ele);					//迭代器指向位置pos插入元素ele
insert(const_iterator pos, int count,ele);			//迭代器指向位置pos插入count个元素ele
erase(const_iterator pos);							//删除迭代器指向的元素
erase(const_iterator start, const_iterator end);	//删除迭代器从start到end之间的元素
clear();			//删除容器中所有元素
```





**示例：**

```C++
#include <vector>

void printVector(vector<int>& v)
{

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//插入和删除
void test01()
{
	vector<int> v1;
	//尾插
	v1.push_back(10);
	v1.push_back(20);
	v1.push_back(30);
	v1.push_back(40);
	v1.push_back(50);
	printVector(v1);
	//尾删
	v1.pop_back();
	printVector(v1);
	//插入
	v1.insert(v1.begin(), 100);
	printVector(v1);

	v1.insert(v1.begin(), 2, 1000);
	printVector(v1);

	//删除
    vector<int>::iterator it = v.begin()+2;
	v1.erase(v1.begin());
    v1.erase(it);
	printVector(v1);
    
    //删除特定元素
    v1.erase(find(v1.begin(), v1.end(), 3));	//删除第一个 元素3
    v1.erase(remove(v1.begin(), v1.end(), 3), v.end());	//移动所有 元素3 至end()之后

	//清空
	v1.erase(v1.begin(), v1.end());
	v1.clear();
	printVector(v1);
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：

* 尾插  --- push_back
* 尾删  --- pop_back
* 插入  --- insert    (位置迭代器)
* 删除  --- erase  （位置迭代器）
* 清空  ---  clear  













#### 3.2.6 vector数据存取



**功能描述：**

* 对vector中的数据的存取操作



**函数原型：**

```c++
at(int idx);	//返回索引idx所指的数据
operator[];		//返回索引idx所指的数据
front();		//返回容器中第一个数据元素
back();			//返回容器中最后一个数据元素
```





**示例：**

```C++
#include <vector>

void test01()
{
	vector<int>v1;
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}

	for (int i = 0; i < v1.size(); i++)
	{
		cout << v1[i] << " ";
	}
	cout << endl;

	for (int i = 0; i < v1.size(); i++)
	{
		cout << v1.at(i) << " ";
	}
	cout << endl;

	cout << "v1的第一个元素为： " << v1.front() << endl;
	cout << "v1的最后一个元素为： " << v1.back() << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：

* 除了用迭代器获取vector容器中元素，[ ]和at也可以
* front返回容器第一个元素
* back返回容器最后一个元素











#### 3.2.7 vector互换容器

**功能描述：**

* 实现两个容器内元素进行互换
* 通过连续两次的呼唤可以对vector容器进行收缩



**函数原型：**

```C++
swap(vec);		// 将vec与本身的元素互换
```





**示例：**

```C++
#include <vector>

void printVector(vector<int>& v)
{

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
    {
		cout << *it << " ";
	}
	cout << endl;
}

void test01()
{
	vector<int>v1;
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);

	vector<int>v2;
	for (int i = 10; i > 0; i--)
	{
		v2.push_back(i);
	}
	printVector(v2);

	//互换容器
	cout << "互换后" << endl;
	v1.swap(v2);
	printVector(v1);
	printVector(v2);
}

void test02()
{
	vector<int> v;
	for (int i = 0; i < 100000; i++)
    {
		v.push_back(i);
	}

	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;

	v.resize(3);

	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;

	//收缩内存
	vector<int>(v).swap(v);
    //匿名对象是通过拷贝 v 中的元素而创建的,故容器的容量和大小相差不大

	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;
}

int main()
{
	test01();

	test02();

	system("pause");

	return 0;
}

```

总结：swap可以使两个容器互换，可以达到实用的收缩内存效果









#### 3.2.8 vector预留空间

**功能描述：**

* 减少vector在动态扩展容量时的扩展次数



**函数原型：**

```c++
reserve(int len);	//容器预留len个元素长度，预留位置不初始化，元素不可访问。
```





**示例：**

```C++
#include <vector>

void test01()
{
	vector<int> v;

	//预留空间
	v.reserve(100000);

	int num = 0;
	int* p = NULL;
	for (int i = 0; i < 100000; i++)
    {
		v.push_back(i);
		if (p != &v[0])		//如果容器的空间不足,容器将会重新分配内存,首地址的位置会发生改变
        {
			p = &v[0];
			num++;		//记录重新分配内存的次数
		}
	}

	cout << "num:" << num << endl;
}

int main()
{
	test01();
    
	system("pause");

	return 0;
}
```

总结：如果数据量较大，可以一开始利用reserve预留空间









#### 3.2.9 vector比较大小

`vector` 是可以直接进行比较的，且可以使用这些运算符，例如 `<`， `>`， `<=`， `>=`， `==`， `!=`



**比较规则：**

1. **逐元素比较**：
    - 比较从第一个元素开始，依次比较两个 `vector` 的对应元素
    - 一旦某个位置的元素满足比较条件，比较就会结束
2. **词典序**：
    - 类似于字符串比较，按元素的大小决定结果
3. **长度差异**：
    - 如果比较到一个 `vector` 的所有元素，而另一个还有剩余元素，则长度较短的 `vector` 被视为小



**注意事项：**

1. 元素类型
    - 如果 `vector` 的元素类型是用户自定义类型，则需要为该类型定义 `<` 操作符，或者传递自定义的比较函数
    - 不同元素类型的 `vector` 是 **不能直接比较的**
2. 性能
    - 比较操作的时间复杂度为 O(min⁡(n,m))O(\min(n, m))O(min(n,m))，其中 nnn 和 mmm 是两个 `vector` 的长度



**示例：**

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main()
{
    vector<int> v1 = {1, 2, 3};
    vector<int> v2 = {1, 2, 4};
    vector<int> v3 = {1, 2};

    // 逐元素比较
    cout << (v1 < v2) << endl;  // 输出 1 (true，因为 3 < 4)
    cout << (v1 > v3) << endl;  // 输出 1 (true，因为 v1 比 v3 长)
    cout << (v1 == v3) << endl; // 输出 0 (false，因为长度和内容不同)

    return 0;
}

```









### 3.3 deque 双端数组

#### 3.3.1 deque容器基本概念



**功能：**

* 双端数组，可以对头端进行插入删除操作



**deque与vector区别：**

* vector对于头部的插入删除效率低，数据量越大，效率越低
* deque相对而言，对头部的插入删除速度回比vector快
* vector访问元素时的速度会比deque快,这和两者内部实现有关

![说明: 2015-11-19_204101](assets/双端向量.jpg)



deque内部工作原理:

deque内部有个**中控器**，维护每段缓冲区中的内容，缓冲区中存放真实数据

中控器维护的是每个缓冲区的地址，使得使用deque时像一片连续的内存空间

![clip_image002-1547547896341](assets/中控器和缓冲区.jpg)

* deque容器的迭代器也是支持随机访问的



#### 3.3.2 deque构造函数

**功能描述：**

* deque容器构造



**函数原型：**

* `deque<T>` deqT;                      //默认构造形式
* `deque(beg, end);`                  //构造函数将[beg, end)区间中的元素拷贝给本身。
* `deque(n, elem);`                    //构造函数将n个elem拷贝给本身。
* `deque(const deque &deq);`   //拷贝构造函数





**示例：**

```C++
#include <deque>

void printDeque(const deque<int>& d) 
{
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";

	}
	cout << endl;
}
//deque构造
void test01() {

	deque<int> d1; //无参构造函数
	for (int i = 0; i < 10; i++)
	{
		d1.push_back(i);
	}
	printDeque(d1);
	deque<int> d2(d1.begin(),d1.end());
	printDeque(d2);

	deque<int>d3(10,100);
	printDeque(d3);

	deque<int>d4 = d3;
	printDeque(d4);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

**总结：**deque容器和vector容器的构造方式几乎一致，灵活使用即可









#### 3.3.3 deque赋值操作



**功能描述：**

* 给deque容器进行赋值



**函数原型：**

* `deque& operator=(const deque &deq); `         //重载等号操作符


* `assign(beg, end);`                                           //将[beg, end)区间中的数据拷贝赋值给本身。
* `assign(n, elem);`                                             //将n个elem拷贝赋值给本身。





**示例：**

```C++
#include <deque>

void printDeque(const deque<int>& d) 
{
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";

	}
	cout << endl;
}
//赋值操作
void test01()
{
	deque<int> d1;
	for (int i = 0; i < 10; i++)
	{
		d1.push_back(i);
	}
	printDeque(d1);

	deque<int>d2;
	d2 = d1;
	printDeque(d2);

	deque<int>d3;
	d3.assign(d1.begin(), d1.end());
	printDeque(d3);

	deque<int>d4;
	d4.assign(10, 100);
	printDeque(d4);

}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：deque赋值操作也与vector相同，需熟练掌握







#### 3.3.4 deque大小操作

**功能描述：**

* 对deque容器的大小进行操作



**函数原型：**

* `deque.empty();`                       //判断容器是否为空

* `deque.size();`                         //返回容器中元素的个数

* `deque.resize(num);`                //重新指定容器的长度为num,若容器变长，则以默认值填充新位置。

  			                             //如果容器变短，则末尾超出容器长度的元素被删除。

* `deque.resize(num, elem);`     //重新指定容器的长度为num,若容器变长，则以elem值填充新位置。

                                                       //如果容器变短，则末尾超出容器长度的元素被删除。

  



**示例：**

```C++
#include <deque>

void printDeque(const deque<int>& d) 
{
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";

	}
	cout << endl;
}

//大小操作
void test01()
{
	deque<int> d1;
	for (int i = 0; i < 10; i++)
	{
		d1.push_back(i);
	}
	printDeque(d1);

	//判断容器是否为空
	if (d1.empty()) {
		cout << "d1为空!" << endl;
	}
	else {
		cout << "d1不为空!" << endl;
		//统计大小
		cout << "d1的大小为：" << d1.size() << endl;
	}

	//重新指定大小
	d1.resize(15, 1);
	printDeque(d1);

	d1.resize(5);
	printDeque(d1);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

* deque没有容量的概念
* 判断是否为空   --- empty
* 返回元素个数   --- size
* 重新指定个数   --- resize









#### 3.3.5 deque 插入和删除

**功能描述：**

* 向deque容器中插入和删除数据



**函数原型：**

两端插入操作：

- `push_back(elem);`          //在容器尾部添加一个数据
- `push_front(elem);`        //在容器头部插入一个数据
- `pop_back();`                   //删除容器最后一个数据
- `pop_front();`                 //删除容器第一个数据

指定位置操作：

* `insert(pos,elem);`         //在pos位置插入一个elem元素的拷贝，返回新数据的位置。

* `insert(pos,n,elem);`     //在pos位置插入n个elem数据，无返回值。

* `insert(pos,beg,end);`    //在pos位置插入[beg,end)区间的数据，无返回值。

* `clear();`                           //清空容器的所有数据

* `erase(beg,end);`             //删除[beg,end)区间的数据，返回下一个数据的位置。

* `erase(pos);`                    //删除pos位置的数据，返回下一个数据的位置。

  

  



**示例：**

```C++
#include <deque>

void printDeque(const deque<int>& d) 
{
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";

	}
	cout << endl;
}
//两端操作
void test01()
{
	deque<int> d;
	//尾插
	d.push_back(10);
	d.push_back(20);
	//头插
	d.push_front(100);
	d.push_front(200);

	printDeque(d);

	//尾删
	d.pop_back();
	//头删
	d.pop_front();
	printDeque(d);
}

//插入
void test02()
{
	deque<int> d;
	d.push_back(10);
	d.push_back(20);
	d.push_front(100);
	d.push_front(200);
	printDeque(d);

	d.insert(d.begin(), 1000);
	printDeque(d);

	d.insert(d.begin(), 2,10000);
	printDeque(d);

	deque<int>d2;
	d2.push_back(1);
	d2.push_back(2);
	d2.push_back(3);

	d.insert(d.begin(), d2.begin(), d2.end());
	printDeque(d);

}

//删除
void test03()
{
	deque<int> d;
	d.push_back(10);
	d.push_back(20);
	d.push_front(100);
	d.push_front(200);
	printDeque(d);

	d.erase(d.begin());
	printDeque(d);

	d.erase(d.begin(), d.end());
	d.clear();
	printDeque(d);
}

int main() {

	//test01();

	//test02();

    test03();
    
	system("pause");

	return 0;
}

```

总结：

* 插入和删除提供的位置是迭代器！
* 尾插   ---  push_back
* 尾删   ---  pop_back
* 头插   ---  push_front
* 头删   ---  pop_front











#### 3.3.6 deque 数据存取



**功能描述：**

* 对deque 中的数据的存取操作



**函数原型：**

- `at(int idx); `     //返回索引idx所指的数据
- `operator[]; `      //返回索引idx所指的数据
- `front(); `            //返回容器中第一个数据元素
- `back();`              //返回容器中最后一个数据元素



**示例：**

```C++
#include <deque>

void printDeque(const deque<int>& d) 
{
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";

	}
	cout << endl;
}

//数据存取
void test01()
{

	deque<int> d;
	d.push_back(10);
	d.push_back(20);
	d.push_front(100);
	d.push_front(200);

	for (int i = 0; i < d.size(); i++) {
		cout << d[i] << " ";
	}
	cout << endl;


	for (int i = 0; i < d.size(); i++) {
		cout << d.at(i) << " ";
	}
	cout << endl;

	cout << "front:" << d.front() << endl;

	cout << "back:" << d.back() << endl;

}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

- 除了用迭代器获取deque容器中元素，[ ]和at也可以
- front返回容器第一个元素
- back返回容器最后一个元素













#### 3.3.7  deque 排序

**功能描述：**

* 利用算法实现对deque容器进行排序



**算法：**

* `sort(iterator beg, iterator end)`  //对beg和end区间内元素进行排序





**示例：**

```C++
#include <deque>
#include <algorithm>

void printDeque(const deque<int>& d) 
{
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";

	}
	cout << endl;
}

void test01()
{

	deque<int> d;
	d.push_back(10);
	d.push_back(20);
	d.push_front(100);
	d.push_front(200);

	printDeque(d);
	sort(d.begin(), d.end());
	printDeque(d);

}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：sort算法非常实用，使用时包含头文件 algorithm即可











### 3.4 array 定长数组

`array` 是 STL 提供的**固定大小数组容器**，可以看作 `C` 风格数组的封装，比 `vector` 更轻量，大小在编译时确定，不能动态扩展



基本特性

1. **固定大小**：不像 `vector`，`array` 的大小必须在编译时确定，不能动态调整

2. **支持 STL 容器操作**：如 `size()`、`front()`、`back()` 等

3. **支持 `std::algorithm` 相关算法**（如 `sort()`、`fill()`）

4. **底层仍是连续存储**（可以与 C 语言数组兼容，如 `data()` 返回 `T*` 指针）





#### 3.4.1 构造函数

`array` **没有构造函数**，但其可以通过 `c` 数组的方式进行初始化

```cpp
array<T, n> arr = {elem1, elem2, elem3, elem4, elem5};  // 推荐, 直接初始化
array<T, n> arr{elem1, elem2, elem3, elem4};  	// 也可以用大括号, 未设置的值默认为0
array<T, n> arr;	//默认初始化, 若为局部变量, 值随机
fill(elem);			// 将所有元素设置为统一值
```



**示例：**

```cpp
#include <iostream>
#include <array>
#include <algorithm>

int main()
{
    array<int, 5> arr1 = {4, 3, 2, 1};
    array<int, 5> arr2{1, 2, 3, 4};
    array<int, 5> arr3;

    // 用 = 赋值
    for (int i = 0; i < 5; ++i)
        cout << arr1[i] << " ";
    cout << endl;
    // 4 3 2 1 0
    
    // 直接用 {} 赋值
    for (int i = 0; i < 5; ++i)
        cout << arr2[i] << " ";
    cout << endl;
    // 1 2 3 4 0
    
    // 默认随机值
    for (int i = 0; i < 5; ++i)
        cout << arr3[i] << " ";
    cout << endl;
    // -858993460 -858993460 -858993460 -858993460 -858993460
    
    // 所有元素统一赋值
    arr3.fill(2);
    for (int i = 0; i < 5; ++i)
        cout << arr3[i] << " ";
    cout << endl;
    // 2 2 2 2 2

    return 0;
}
```





#### 3.4.2 常用函数

`vector` 的函数 `array ` 除了改变数组大小的（`push_back`，`pop`，`clear`，`resize`，`erase`，`insert`，`reserve` ）函数外，其余函数共用



**常用函数：**

| 函数        | 作用                                             |
| ----------- | ------------------------------------------------ |
| size()      | 获取数组大小（编译期固定）                       |
| front()     | 返回第一个元素                                   |
| back()      | 返回最后一个元素                                 |
| fill(value) | 用 `value` 填充整个数组                          |
| data()      | 返回指向底层 C 数组的指针（`T*`）                |
| at(index)   | **安全访问**元素，带边界检查，超出范围会抛出异常 |



**示例：**

```cpp
#include <iostream>
#include <array>
#include <algorithm>
using namespace std;

int main()
{
    // 1. 定义 array，必须指定大小
    array<int, 5> arr = { 3, 1, 4, 1, 5 };

    // 2. 访问元素
    cout << "第一个元素：" << arr[0] << "\n";
    cout << "最后一个元素：" << arr.back() << "\n";

    // 3. 获取大小
    cout << "数组大小：" << arr.size() << "\n";

    // 4. 遍历元素
    cout << "所有元素：";
    for (int x : arr)
        cout << x << " ";
    cout << "\n";

    // 5. 排序
    sort(arr.begin(), arr.end());
    cout << "排序后：";
    for (int x : arr)
        cout << x << " ";
    cout << "\n";

    return 0;
}
```









### 3.5 stack 栈

#### 3.5.1 stack 基本概念



**概念：**stack是一种**先进后出**(First In Last Out,FILO)的数据结构，它只有一个出口





![说明: 2015-11-15_195707](assets/栈容器.jpg)

栈中只有顶端的元素才可以被外界使用，因此栈不允许有遍历行为

栈中进入数据称为  --- **入栈**  `push`

栈中弹出数据称为  --- **出栈**  `pop`



生活中的栈：

![img](assets/栈案例.png)







#### 3.5.2 stack 常用接口

功能描述：栈容器常用的对外接口



```cpp
//构造函数
stack<T> stk;            	//stack采用模板类实现， stack对象的默认构造形式
stack(const stack &stk);	//拷贝构造函数

//赋值操作
stack& operator=(const stack &stk);		//重载等号操作符

//数据存取
push(elem);		//向栈顶添加元素
pop();			//从栈顶移除第一个元素
top();			//返回栈顶元素

//大小操作
empty();	//判断堆栈是否为空
size();     //返回栈的大小
```



注意：栈（stack）和队列（queue）==不能使用 [] 访问==





**示例：**

```C++
#include <stack>

//栈容器常用接口
void test01()
{
	//创建栈容器 栈容器必须符合先进后出
	stack<int> s;

	//向栈中添加元素，叫做 压栈 入栈
	s.push(10);
	s.push(20);
	s.push(30);

	while (!s.empty())
    {
		//输出栈顶元素
		cout << "栈顶元素为： " << s.top() << endl;
		//弹出栈顶元素
		s.pop();
	}	//stack容器没有clear操作,只能通过循环pop的方式清空

	cout << "栈的大小为：" << s.size() << endl;

}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：

* 入栈   --- push
* 出栈   --- pop
* 返回栈顶   --- top
* 判断栈是否为空   --- empty
* 返回栈大小   --- size











### 3.6 queue 队列

#### 3.6.1 queue 基本概念

**概念：**Queue是一种**先进先出**(First In First Out,FIFO)的数据结构，它有两个出口







![说明: 2015-11-15_214429](assets/队列容器.jpg)

队列容器允许从一端新增元素，从另一端移除元素

队列中只有队头和队尾才可以被外界使用，因此队列不允许有遍历行为

队列中进数据称为 --- **入队**    `push`

队列中出数据称为 --- **出队**    `pop`



生活中的队列：

![1547606785041](assets/队列案例.png)







#### 3.6.2 queue 常用接口



功能描述：队列容器常用的对外接口



构造函数：

- `queue<T> que;`                                 	//queue采用模板类实现，queue对象的默认构造形式
- `queue(const queue &que);`              //拷贝构造函数

赋值操作：

- `queue& operator=(const queue &que);`           //重载等号操作符

数据存取：

- `push(elem);`                            //往队尾添加元素
- `pop();`                                      //从队头移除第一个元素
- `back();`                                    //返回最后一个元素
- `front(); `                                  //返回第一个元素

大小操作：

- `empty();`            //判断队列是否为空
- `size(); `              //返回队列的长度



**示例：**

```C++
#include <queue>
#include <string>
class Person
{
public:
	Person(string name, int age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}

	string m_Name;
	int m_Age;
};

void test01() {

	//创建队列
	queue<Person> q;

	//准备数据
	Person p1("唐僧", 30);
	Person p2("孙悟空", 1000);
	Person p3("猪八戒", 900);
	Person p4("沙僧", 800);

	//向队列中添加元素  入队操作
	q.push(p1);
	q.push(p2);
	q.push(p3);
	q.push(p4);

	//队列不提供迭代器，更不支持随机访问	
	while (!q.empty()) {
		//输出队头元素
		cout << "队头元素-- 姓名： " << q.front().m_Name 
              << " 年龄： "<< q.front().m_Age << endl;
        
		cout << "队尾元素-- 姓名： " << q.back().m_Name  
              << " 年龄： " << q.back().m_Age << endl;
        
		cout << endl;
		//弹出队头元素
		q.pop();
	}

	cout << "队列大小为：" << q.size() << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

- 入队   --- push
- 出队   --- pop
- 返回队头元素   --- front
- 返回队尾元素   --- back
- 判断队是否为空   --- empty
- 返回队列大小   --- size







#### 3.6.3 priority_queue 优先级队列

**概念：**元素根据优先级出队，而不是入队顺序

*   默认情况下，优先级最高的元素最先出队

*   如果是容器类型是pair，以第一个元素为基准



**区别：**

-   queue：
    -   push()：入队
    -   pop()：出队（移除队首元素）
    -   front()：访问队首元素
-   priority_queue：
    -   push()：插入元素，并自动调整顺序
    -   pop()：移除优先级最高的元素
    -   top()：访问优先级最高的元素



**排序：**默认使用最大堆（最大优先级），可以通过自定义比较器实现不同的排序方式（例如最小堆）



**示例：**

```cpp
#include <iostream>
#include <queue>		//该头文件也包含了 priority_queue 容器
using namespace std;

int main()
{
    //使用 queue
    queue<int> q;
    q.push(1);
    q.push(3);
    q.push(2);
    while (!q.empty())
    {
        cout << q.front() << " "; 	//输出: 1 3 2
        q.pop();
    }
    cout << endl;

    //使用 priority_queue
    priority_queue<int> pq1;
    pq1.push(1);
    pq1.push(3);
    pq1.push(2);
    while (!pq1.empty())
    {
        cout << pq1.top() << " "; 	//输出: 3 2 1
        pq1.pop();
    }
    cout << endl;

    //使用小根堆设置优先队列
    priority_queue<int, vector<int>, greater<int>> pq2;
    pq2.push(1);
    pq2.push(3);
    pq2.push(2);
    while (!pq2.empty())
    {
        cout << pq2.top() << " "; 	//输出: 1 2 3
        pq2.pop();
    }
    cout << endl;
    return 0;
}
```















### 3.7 list 链表

#### 3.7.1 list基本概念



**功能：**将数据进行链式存储

**链表**（list）是一种物理存储单元上非连续的存储结构，数据元素的逻辑顺序是通过链表中的指针链接实现的



链表的组成：链表由一系列**结点**组成



结点的组成：一个是存储数据元素的**数据域**，另一个是存储下一个结点地址的**指针域**



STL中的链表是一个双向循环链表



![说明: 2015-11-15_225145](assets/链表容器.jpg)

由于链表的存储方式并不是连续的内存空间，因此链表list中的迭代器只支持前移和后移，属于**双向迭代器**



list的优点：

* 采用动态存储分配，不会造成内存浪费和溢出
* 链表执行插入和删除操作十分方便，修改指针即可，不需要移动大量元素

list的缺点：

* 链表灵活，但是空间(指针域) 和 时间（遍历）额外耗费较大



List有一个重要的性质，插入操作和删除操作都不会造成原有list迭代器的失效，这在vector是不成立的。



总结：STL中**List和vector是两个最常被使用的容器**，各有优缺点





#### 3.7.2  list构造函数

**功能描述：**

* 创建list容器



**函数原型：**

```cpp
list<T> lst;			//list采用采用模板类实现,对象的默认构造形式
list(beg,end);			//构造函数将[beg, end)区间中的元素拷贝给本身。
list(n,elem);			//构造函数将n个elem拷贝给本身。
list(const list &lst);	//拷贝构造函数。
```





**示例：**

```C++
#include <list>

void printList(const list<int>& L) {

	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test01()
{
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);

	printList(L1);

	list<int>L2(L1.begin(),L1.end());
	printList(L2);

	list<int>L3(L2);
	printList(L3);

	list<int>L4(10, 1000);
	printList(L4);
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：list构造方式同其他几个STL常用容器，熟练掌握即可













#### 3.7.3 list 赋值和交换

**功能描述：**

* 给list容器进行赋值，以及交换list容器

**函数原型：**

```cpp
assign(beg, end);            		//将[beg, end)区间中的数据拷贝赋值给本身。
assign(n, elem);              		//将n个elem拷贝赋值给本身。
list& operator=(const list &lst);   //重载等号操作符
swap(lst);                          //将lst与本身的元素互换。
```





**示例：**

```C++
#include <list>

void printList(const list<int>& L)
{
	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++)
		cout << *it << " ";

	cout << endl;
}

//赋值和交换
void test01()
{
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);
	printList(L1);

	//赋值
	list<int>L2;
	L2 = L1;
	printList(L2);

	list<int>L3;
	L3.assign(L2.begin(), L2.end());
	printList(L3);

	list<int>L4;
	L4.assign(10, 100);
	printList(L4);

}

//交换
void test02()
{
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);

	list<int>L2;
	L2.assign(10, 100);

	cout << "交换前： " << endl;
	printList(L1);
	printList(L2);

	cout << endl;

	L1.swap(L2);

	cout << "交换后： " << endl;
	printList(L1);
	printList(L2);
}

int main()
{
	//test01();

	test02();

	system("pause");

	return 0;
}
```

总结：list赋值和交换操作能够灵活运用即可















#### 3.7.4 list 大小操作

**功能描述：**

* 对list容器的大小进行操作



**函数原型：**

```cpp
size();         //返回容器中元素的个数
empty();        //判断容器是否为空
resize(num);	//重新指定容器的长度为num，若容器变长，则以默认值填充新位置
	//如果容器变短，则末尾超出容器长度的元素被删除

resize(num, elem);	//重新指定容器的长度为num，若容器变长，则以elem值填充新位置
	//如果容器变短，则末尾超出容器长度的元素被删除。
```





**示例：**

```C++
#include <list>

void printList(const list<int>& L)
{
	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++)
    {
		cout << *it << " ";
	}
	cout << endl;
}

//大小操作
void test01()
{
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);

	if (L1.empty())
	{
		cout << "L1为空" << endl;
	}
	else
	{
		cout << "L1不为空" << endl;
		cout << "L1的大小为： " << L1.size() << endl;
	}

	//重新指定大小
	L1.resize(10);
	printList(L1);

	L1.resize(2);
	printList(L1);
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：

- 判断是否为空   --- empty
- 返回元素个数   --- size
- 重新指定个数   --- resize











#### 3.7.5 list 插入和删除

**功能描述：**

* 对list容器进行数据的插入和删除



**函数原型：**

```cpp
push_back(elem);		//在容器尾部加入一个元素
pop_back();				//删除容器中最后一个元素
push_front(elem);		//在容器开头插入一个元素
pop_front();			//从容器开头移除第一个元素

insert(pos,elem);		//在pos位置插elem元素的拷贝,返回新数据的位置
insert(pos,n,elem);		//在pos位置插入n个elem数据,无返回值
insert(pos,beg,end);	//在pos位置插入[beg,end)区间的数据,无返回值

clear();			//移除容器的所有数据
erase(beg,end);		//删除[beg,end)区间的数据,返回下一个数据的位置
erase(pos);			//删除pos位置的数据,返回下一个数据的位置。
remove(elem);		//删除容器中所有与elem值匹配的元素
```







**示例：**

```C++
#include <list>

void printList(const list<int>& L)
{

	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++)
    {
		cout << *it << " ";
	}
	cout << endl;
}

//插入和删除
void test01()
{
	list<int> L;
	//尾插
	L.push_back(10);
	L.push_back(20);
	L.push_back(30);
	//头插
	L.push_front(100);
	L.push_front(200);
	L.push_front(300);

	printList(L);

	//尾删
	L.pop_back();
	printList(L);

	//头删
	L.pop_front();
	printList(L);

	//插入
	list<int>::iterator it = L.begin();
	L.insert(++it, 1000);
	printList(L);

	//删除
	it = L.begin();
	L.erase(++it);
	printList(L);

	//移除
	L.push_back(10000);
	L.push_back(10000);
	L.push_back(10000);
	printList(L);
	L.remove(10000);
	printList(L);
    
    //清空
	L.clear();
	printList(L);
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：

* 尾插   --- push_back
* 尾删   --- pop_back
* 头插   --- push_front
* 头删   --- pop_front
* 插入   --- insert
* 删除   --- erase
* 移除   --- remove
* 清空   --- clear

















#### 3.7.6 list 数据存取

**功能描述：**

* 对list容器中数据进行存取



**函数原型：**

```cpp
front(); 	//返回第一个元素。
back();     //返回最后一个元素。
```







**示例：**

```C++
#include <list>

//数据存取
void test01()
{
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);

	
	//cout << L1.at(0) << endl;//错误 不支持at访问数据
	//cout << L1[0] << endl; //错误  不支持[]方式访问数据
	cout << "第一个元素为： " << L1.front() << endl;
	cout << "最后一个元素为： " << L1.back() << endl;

	//list容器的迭代器是双向迭代器，不支持随机访问
	list<int>::iterator it = L1.begin();
	//it = it + 1;//错误，不可以跳跃访问，即使是+1
}

int main()
{
	test01();

	system("pause");

	return 0;
}

```

总结：

* list容器中不可以通过[]或者at方式访问数据
* 返回第一个元素   --- front
* 返回最后一个元素   --- back











#### 3.7.7 list 反转和排序

**功能描述：**

* 将容器中的元素反转，以及将容器中的数据进行排序



**函数原型：**

```cpp
reverse();		//反转链表
sort();        	//链表排序
```







**示例：**

```C++
void printList(const list<int>& L)
{
	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++)
    {
		cout << *it << " ";
	}
	cout << endl;
}

bool myCompare(int val1 , int val2)
{
	return val1 > val2;
}

//反转和排序
void test01()
{
	list<int> L;
	L.push_back(90);
	L.push_back(30);
	L.push_back(20);
	L.push_back(70);
	printList(L);

	//反转容器的元素
	L.reverse();
	printList(L);

	//排序
	L.sort(); //默认的排序规则 从小到大
	printList(L);

	L.sort(myCompare); //指定规则，从大到小
	printList(L);
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：

* 反转   --- reverse
* 排序   --- sort （成员函数）











#### 3.7.8 排序案例

案例描述：将Person自定义数据类型进行排序，Person中属性有姓名、年龄、身高

排序规则：按照年龄进行升序，如果年龄相同按照身高进行降序



**示例：**

```C++
#include <list>
#include <string>
class Person
{
public:
	Person(string name, int age , int height)
    {
		m_Name = name;
		m_Age = age;
		m_Height = height;
	}

public:
	string m_Name;  //姓名
	int m_Age;      //年龄
	int m_Height;   //身高
};


bool ComparePerson(Person& p1, Person& p2)
{

	if (p1.m_Age == p2.m_Age)
    {
		return p1.m_Height  > p2.m_Height;
	}
	else
	{
		return  p1.m_Age < p2.m_Age;
	}

}

void test01()
{
	list<Person> L;

	Person p1("刘备", 35 , 175);
	Person p2("曹操", 45 , 180);
	Person p3("孙权", 40 , 170);
	Person p4("赵云", 25 , 190);
	Person p5("张飞", 35 , 160);
	Person p6("关羽", 35 , 200);

	L.push_back(p1);
	L.push_back(p2);
	L.push_back(p3);
	L.push_back(p4);
	L.push_back(p5);
	L.push_back(p6);

	for (list<Person>::iterator it = L.begin(); it != L.end(); it++)
    {
		cout << "姓名： " << it->m_Name << " 年龄： " << it->m_Age 
              << " 身高： " << it->m_Height << endl;
	}

	cout << "---------------------------------" << endl;
	L.sort(ComparePerson); //排序

	for (list<Person>::iterator it = L.begin(); it != L.end(); it++)
    {
		cout << "姓名： " << it->m_Name << " 年龄： " << it->m_Age 
              << " 身高： " << it->m_Height << endl;
	}
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```



总结：

* 对于自定义数据类型，必须要指定排序规则，否则编译器不知道如何进行排序


* 高级排序只是在排序规则上再进行一次逻辑规则制定，并不复杂



















### 3.8 set/ multiset 容器

#### 3.8.1 set基本概念

**简介：**

* 所有元素都会在插入时自动被排序





**本质：**

* set/multiset属于**关联式容器**，底层结构是用**二叉树**实现。





**set和multiset区别**：

* set不允许容器中有重复的元素
* multiset允许容器中有重复的元素





#### 3.8.2 set构造和赋值

功能描述：创建set容器以及赋值



**函数原型：**

```cpp
//构造
set<T> st;				//默认构造函数：
set(const set &st);		//拷贝构造函数

//赋值
set& operator=(const set &st);		//重载等号操作符
```





**示例：**

```C++
#include <set>

void printSet(set<int> & s)
{
	for (set<int>::iterator it = s.begin(); it != s.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}

//构造和赋值
void test01()
{
	set<int> s1;

	s1.insert(10);
	s1.insert(30);
	s1.insert(20);
	s1.insert(40);
	printSet(s1);

	//拷贝构造
	set<int>s2(s1);
	printSet(s2);

	//赋值
	set<int>s3;
	s3 = s2;
	printSet(s3);
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：

* set容器插入数据时用insert
* set容器插入数据的数据会自动排序











#### 3.8.3 set大小和交换

**功能描述：**

* 统计set容器大小以及交换set容器



**函数原型：**

```cpp
size();        //返回容器中元素的数目
empty();       //判断容器是否为空
swap(st);      //交换两个集合容器
```





**示例：**

```C++
#include <set>

void printSet(set<int> & s)
{
	for (set<int>::iterator it = s.begin(); it != s.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}

//大小
void test01()
{

	set<int> s1;
	
	s1.insert(10);
	s1.insert(30);
	s1.insert(20);
	s1.insert(40);

	if (s1.empty())
	{
		cout << "s1为空" << endl;
	}
	else
	{
		cout << "s1不为空" << endl;
		cout << "s1的大小为： " << s1.size() << endl;
	}

}

//交换
void test02()
{
	set<int> s1;

	s1.insert(10);
	s1.insert(30);
	s1.insert(20);
	s1.insert(40);

	set<int> s2;

	s2.insert(100);
	s2.insert(300);
	s2.insert(200);
	s2.insert(400);

	cout << "交换前" << endl;
	printSet(s1);
	printSet(s2);
	cout << endl;

	cout << "交换后" << endl;
	s1.swap(s2);
	printSet(s1);
	printSet(s2);
}

int main()
{
	//test01();

	test02();

	system("pause");

	return 0;
}
```

总结：

* 统计大小   --- size
* 判断是否为空   --- empty
* 交换容器   --- swap

















#### 3.8.4 set插入和删除

**功能描述：**

* set容器进行插入数据和删除数据





**函数原型：**

```cpp
insert(elem);		//在容器中插入元素。
clear();			//清除所有元素
erase(pos);			//删除pos迭代器所指的元素,返回下一个元素的迭代器
erase(beg, end);   	//删除区间[beg,end)的所有元素,返回下一个元素的迭代器
erase(elem);		//删除容器中值为elem的元素
```







**示例：**

```C++
#include <set>

void printSet(set<int> & s)
{
	for (set<int>::iterator it = s.begin(); it != s.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}

//插入和删除
void test01()
{
	set<int> s1;
	//插入
	s1.insert(10);
	s1.insert(30);
	s1.insert(20);
	s1.insert(40);
	printSet(s1);

	//删除
	s1.erase(s1.begin());
	printSet(s1);

	s1.erase(30);
	printSet(s1);

	//清空
	//s1.erase(s1.begin(), s1.end());
	s1.clear();
	printSet(s1);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

* 插入   --- insert
* 删除   --- erase
* 清空   --- clear











#### 3.8.5 set查找和统计

**功能描述：**

* 对set容器进行查找数据以及统计数据



**函数原型：**

```cpp
find(key); 		//查找key是否存在,若存在,返回该键的元素的迭代器;若不存在,返回set.end()
count(key);     //统计key的元素个数
```







**示例：**

```C++
#include <set>

//查找和统计
void test01()
{
	set<int> s1;
	//插入
	s1.insert(10);
	s1.insert(30);
	s1.insert(20);
	s1.insert(40);
	
	//查找
	set<int>::iterator pos = s1.find(30);

	if (pos != s1.end())
	{
		cout << "找到了元素 ： " << *pos << endl;
	}
	else
	{
		cout << "未找到元素" << endl;
	}

	//统计
	int num = s1.count(30);
	cout << "num = " << num << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：

* 查找   ---  find    （返回的是迭代器）
* 统计   ---  count  （对于set，结果为0或者1）

















#### 3.8.6 set和multiset区别

**学习目标：**

* 掌握 `set` 和 `multiset` 的区别



**区别：**

* set不可以插入重复数据，而multiset可以
* set插入数据的同时会返回插入结果，表示插入是否成功
* multiset不会检测数据，因此可以插入重复数据





**示例：**

```C++
#include <set>

//set和multiset区别
void test01()
{
	set<int> s;
	pair<set<int>::iterator, bool>  ret = s.insert(10);
	if (ret.second)
		cout << "第一次插入成功!" << endl;
	else
		cout << "第一次插入失败!" << endl;

	ret = s.insert(10);
	if (ret.second)
		cout << "第二次插入成功!" << endl;
	else
		cout << "第二次插入失败!" << endl;
    
	//multiset
	multiset<int> ms;
	ms.insert(10);
	ms.insert(10);

	for (multiset<int>::iterator it = ms.begin(); it != ms.end(); it++)
		cout << *it << " ";
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：

* 如果不允许插入重复数据可以利用set
* 如果需要插入重复数据利用multiset











#### 3.8.7 pair对组创建

**功能描述：**

* 成对出现的数据，利用对组可以返回两个数据





**两种创建方式：**

```cpp
pair<type, type> p ( value1, value2 );
pair<type, type> p = make_pair( value1, value2 );
```







**示例：**

```C++
#include <string>

//对组创建
void test01()
{
	pair<string, int> p(string("Tom"), 20);
	cout << "姓名： " <<  p.first << " 年龄： " << p.second << endl;

	pair<string, int> p2 = make_pair("Jerry", 10);
	cout << "姓名： " << p2.first << " 年龄： " << p2.second << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：

两种方式都可以创建对组，记住一种即可













#### 3.8.8 set容器排序

学习目标：

* set容器默认排序规则为从小到大，掌握如何改变排序规则



主要技术点：

* 利用仿函数，可以改变排序规则





**示例一**   set存放内置数据类型

```C++
#include <set>

class MyCompare 
{
public:
	bool operator()(int v1, int v2) {
		return v1 > v2;
	}
};
void test01() 
{    
	set<int> s1;
	s1.insert(10);
	s1.insert(40);
	s1.insert(20);
	s1.insert(30);
	s1.insert(50);

	//默认从小到大
	for (set<int>::iterator it = s1.begin(); it != s1.end(); it++)
		cout << *it << " ";

	cout << endl;

	//指定排序规则
	set<int,MyCompare> s2;
	s2.insert(10);
	s2.insert(40);
	s2.insert(20);
	s2.insert(30);
	s2.insert(50);

	for (set<int, MyCompare>::iterator it = s2.begin(); it != s2.end(); it++)
		cout << *it << " ";

	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：利用仿函数可以指定set容器的排序规则



**示例二** set存放自定义数据类型

```C++
#include <set>
#include <string>

class Person
{
public:
	Person(string name, int age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}

	string m_Name;
	int m_Age;
};
class comparePerson
{
public:
	bool operator()(const Person& p1, const Person &p2)
	{
		//按照年龄进行排序  降序
		return p1.m_Age > p2.m_Age;
	}
};

void test01()
{
	set<Person, comparePerson> s;

	Person p1("刘备", 23);
	Person p2("关羽", 27);
	Person p3("张飞", 25);
	Person p4("赵云", 21);

	s.insert(p1);
	s.insert(p2);
	s.insert(p3);
	s.insert(p4);

	for (set<Person, comparePerson>::iterator it = s.begin(); it != s.end(); it++)
	{
		cout << "姓名： " << it->m_Name << " 年龄： " << it->m_Age << endl;
	}
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：对于自定义数据类型，set必须指定排序规则才可以插入数据







#### 3.8.9 **set/multiset使用区分**

*   插入元素：

    -   `set` 容器使用 `insert()` 方法插入元素，如果元素已经存在，则插入操作不会成功

    -   `multiset` 容器使用 `insert()` 方法插入元素，如果元素已经存在，则插入操作会成功，并增加该元素的计数



*   删除元素：

    -   `set` 容器使用 `erase()` 方法删除元素，如果元素存在，则删除该元素

    -   `multiset` 容器使用 `erase()` 方法删除元素，可以指定删除单个元素，也可以删除所有值为指定值的元素



*   查找元素：

    -   `set` 容器使用 `find()` 方法查找元素，如果元素存在，则返回指向该元素的迭代器，否则返回 `end()` 迭代器

    -   `multiset` 容器使用 `find()` 方法查找元素，如果元素存在，则返回指向第一个匹配元素的迭代器，否则返回 `end()` 迭代器



*   计数元素：

    -   `set` 容器使用 `count()` 方法计数元素，如果元素存在，则返回 1，否则返回 0

    -   `multiset` 容器使用 `count()` 方法计数元素，返回值为指定值的元素数量



*   其他方法：

    -   `set` 和 `multiset` 容器都提供了 `lower_bound()`、`upper_bound()` 和 `equal_range()` 方法，用于查找特定范围内的元素

    -   `set` 和 `multiset` 容器都提供了 `begin()`、`end()`、`size()` 和 `empty()` 方法，用于访问容器的元素和信息



**例：**

```c++
int main()
{
    std::set<int> mySet;				//创建一个 set 容器
    std::multiset<int> myMultiset;		//创建一个 multiset 容器

    //向 set 容器插入元素
    mySet.insert(10);
    mySet.insert(20);
    mySet.insert(10); 		//不会插入，因为元素已经存在
    mySet.insert(30);

    //向 multiset 容器插入元素
    myMultiset.insert(10);
    myMultiset.insert(20);
    myMultiset.insert(10); 	//会插入，因为允许重复元素
    myMultiset.insert(30);

    //遍历 set 容器
    std::cout << "set: ";
    for (auto it = mySet.begin(); it != mySet.end(); ++it)
        std::cout << *it << " ";
    std::cout << std::endl; //10 20 30

    // 遍历 multiset 容器
    std::cout << "multiset: ";
    for (auto it = myMultiset.begin(); it != myMultiset.end(); ++it)
        std::cout << *it << " ";
    std::cout << std::endl; //10 10 20 30

    // 查找元素
    std::cout << "查找元素 10：" << std::endl;
    std::cout << "set: " << mySet.count(10) << std::endl; 			//set 中元素 10 的数量为 1
    std::cout << "multiset: " << myMultiset.count(10) << std::endl; //multiset 中元素 10 的数量为 2

    // 删除元素
    std::cout << "删除元素 10：" << std::endl;
    mySet.erase(10); 		//删除 set 中的元素 10
    auto it=myMultiset.find(10);
	if(it!=myMultiset.end())
		myMultiset.erase(it); //只会删一个
    myMultiset.erase(10); 	//删除 multiset 中所有值为 10 的元素
    
    // 再次遍历
    std::cout << "set: ";
    for (auto it = mySet.begin(); it != mySet.end(); ++it)
        std::cout << *it << " ";
    std::cout << std::endl;	//20 30

    std::cout << "multiset: ";
    for (auto it = myMultiset.begin(); it != myMultiset.end(); ++it)
        std::cout << *it << " ";
    std::cout << std::endl;	//20 30

    return 0;
}
```















### 3.9 map/ multimap 容器

#### 3.9.1 map基本概念

**简介：**

* `map` 中所有元素都是 `pair`
* `pair` 中第一个元素为 `key`（键值），起到索引作用，第二个元素为 `value`（实值）
* 所有元素都会根据元素的键值自动排序



**本质：**`map/multimap` 属于**关联式容器**，底层结构是用二叉树实现



**优点：**可以根据 `key` 值快速找到 `value` 值



`map` 和 `multimap` 区别：

- `map` 不允许容器中有重复 `key` 值元素
- `multimap` 允许容器中有重复 `key` 值元素



#### 3.9.2  map构造和赋值

**功能描述：**

* 对 `map` 容器进行构造和赋值操作

**函数原型：**

```cpp
//构造
map<T1, T2> mp;						//map默认构造函数
map(const map &mp);     			//拷贝构造函数

//赋值
map& operator=(const map &mp);		//重载等号操作符
```



**示例：**

```C++
#include <map>

void printMap(map<int,int>&m)
{
	for (map<int, int>::iterator it = m.begin(); it != m.end(); it++)
	{
		cout << "key = " << it->first << " value = " << it->second << endl;
	}
	cout << endl;
}

void test01()
{
	map<int,int>m; 			//默认构造
	m.insert(pair<int, int>(1, 10));
	m.insert(pair<int, int>(2, 20));
	m.insert(pair<int, int>(3, 30));
	printMap(m);

	map<int, int>m2(m); 	//拷贝构造
	printMap(m2);

	map<int, int>m3;
	m3 = m2; 				//赋值
	printMap(m3);
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：`map` 中所有元素都是成对出现，插入数据时候要使用对组











#### 3.9.3 map大小和交换

**功能描述：**

* 统计 `map` 容器大小以及交换 `map` 容器





函数原型：

- `size();`          //返回容器中元素的数目
- `empty();`        //判断容器是否为空
- `swap(st);`      //交换两个集合容器





**示例：**

```C++
#include <map>

void printMap(map<int,int>&m)
{
	for (map<int, int>::iterator it = m.begin(); it != m.end(); it++)
	{
		cout << "key = " << it->first << " value = " << it->second << endl;
	}
	cout << endl;
}

void test01()
{
	map<int, int>m;
	m.insert(pair<int, int>(1, 10));
	m.insert(pair<int, int>(2, 20));
	m.insert(pair<int, int>(3, 30));

	if (m.empty())
	{
		cout << "m为空" << endl;
	}
	else
	{
		cout << "m不为空" << endl;
		cout << "m的大小为： " << m.size() << endl;
	}
}


//交换
void test02()
{
	map<int, int>m;
	m.insert(pair<int, int>(1, 10));
	m.insert(pair<int, int>(2, 20));
	m.insert(pair<int, int>(3, 30));

	map<int, int>m2;
	m2.insert(pair<int, int>(4, 100));
	m2.insert(pair<int, int>(5, 200));
	m2.insert(pair<int, int>(6, 300));

	cout << "交换前" << endl;
	printMap(m);
	printMap(m2);

	cout << "交换后" << endl;
	m.swap(m2);
	printMap(m);
	printMap(m2);
}

int main() {

	test01();

	test02();

	system("pause");

	return 0;
}
```

总结：

- 统计大小   --- size
- 判断是否为空   --- empty
- 交换容器   --- swap











#### 3.9.4 map插入和删除

**功能描述：**

- map容器进行插入数据和删除数据





**函数原型：**

```cpp
insert(elem);           	//在容器中插入元素。
clear();                    //清除所有元素
erase(pos);              	//删除pos迭代器所指的元素，返回下一个元素的迭代器
erase(beg, end);    		//删除区间[beg,end)的所有元素 ，返回下一个元素的迭代器
erase(key);            		//删除容器中值为key的元素
```



**示例：**

```C++
#include <map>

void printMap(map<int,int>&m)
{
	for (map<int, int>::iterator it = m.begin(); it != m.end(); it++)
	{
		cout << "key = " << it->first << " value = " << it->second << endl;
	}
	cout << endl;
}

void test01()
{
	//插入
	map<int, int> m;
	//第一种插入方式
	m.insert(pair<int, int>(1, 10));
	//第二种插入方式
	m.insert(make_pair(2, 20));
	//第三种插入方式
	m.insert(map<int, int>::value_type(3, 30));
	//第四种插入方式
	m[4] = 40; 
	printMap(m);

	//删除
	m.erase(m.begin());
	printMap(m);

	m.erase(3);
	printMap(m);

	//清空
	m.erase(m.begin(),m.end());
	m.clear();
	printMap(m);
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

* map插入方式很多，记住其一即可

- 插入   --- insert 
- 删除   --- erase
- 清空   --- clear













#### 3.9.5 map查找和统计

**功能描述：**

- 对map容器进行查找数据以及统计数据



**函数原型：**

```cpp
find(key);                  //查找key是否存在,若存在，返回该键的元素的迭代器；若不存在，返回map.end();
count(key);                	//统计key的元素个数
```



**示例：**

```C++
#include <map>

//查找和统计
void test01()
{
	map<int, int>m; 
	m.insert(pair<int, int>(1, 10));
	m.insert(pair<int, int>(2, 20));
	m.insert(pair<int, int>(3, 30));

	//查找
	map<int, int>::iterator pos = m.find(3);

	if (pos != m.end())
	{
		cout << "找到了元素 key = " << (*pos).first << " value = " << (*pos).second << endl;
	}
	else
	{
		cout << "未找到元素" << endl;
	}

	//统计
	int num = m.count(3);
	cout << "num = " << num << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：

- 查找   ---  find    （返回的是迭代器）
- 统计   ---  count  （对于map，结果为0或者1）















#### 3.9.6 map容器排序

**学习目标：**

- map 容器默认排序规则为 按照 key 值进行 从小到大排序，掌握如何改变排序规则





**主要技术点:**

- 利用仿函数，可以改变排序规则





**示例：**

```C++
#include <map>

class MyCompare
{
public:
	bool operator()(int v1, int v2)
    {
		return v1 > v2;
	}
};

void test01() 
{
	//默认从小到大排序
	//利用仿函数实现从大到小排序
	map<int, int, MyCompare> m;

	m.insert(make_pair(1, 10));
	m.insert(make_pair(2, 20));
	m.insert(make_pair(3, 30));
	m.insert(make_pair(4, 40));
	m.insert(make_pair(5, 50));

	for (map<int, int, MyCompare>::iterator it = m.begin(); it != m.end(); it++)
    {
		cout << "key:" << it->first << " value:" << it->second << endl;
	}
}
int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

* 利用仿函数可以指定map容器的排序规则
* 对于自定义数据类型，map必须要指定排序规则,同set容器











#### 3.9.7 unordered_map 哈希表

**简介：**`unordered_map` 是标准模板库（STL）的一部分，提供了一种 **基于哈希表的键值对容器**

*   与 map 不同，unordered_map 不保证元素的排序，但通常提供更快的查找速度
*   它是一个关联容器，它存储了键值对（key-value pairs），其中每个键（key）都是唯一的
*   unordered_map 使用哈希表来存储元素，这使得它在查找、插入和删除操作中具有平均常数时间复杂度



**基本语法：**`unordered_map<key_type, value_type> map_name;`

-   key_type	是键的类型
-   value_type	 是值的类型



**构造函数：**

```c++
//默认构造
unordered_map<int, string> myMap;

//初始化构造
unordered_map<int, string> myMap = {{1, "one"}, {2, "two"}};

//指定初始容量构造
unordered_map<int, string> myMap(10);

//拷贝构造
unordered_map<int, string> anotherMap = myMap;
```







**基本操作：**

```c++
//插入元素
myMap.insert({3, "three"});
myMap[4]="fourth";				//可以通过[]来访问或插入没有的键值,且初始元素为0(或者'/0',false,没有试过不确定)

//访问元素
string value = myMap[1]; 		//获取键为1的值

//删除元素
myMap.erase(1); 				//删除键为1的元素

//查找元素
auto it = myMap.find(2); 		//查找键为2的元素
if (it != myMap.end())			//end是容器的最后一个元素的后一个
    cout << "Found: " << it->second << endl;
```







**示例：**

```c++
int main()
{
    //创建一个 unordered_map,键为 int,值为 string
    unordered_map<int, string> myMap;

    //插入一些键值对
    myMap[1] = "one";
    myMap[2] = "two";
    myMap[3] = "three";

    //打印所有元素
    for (const auto& pair : myMap)
        cout << "Key: " << pair.first << ", Value: " << pair.second << endl;
    //由插入重后往前遍历

    //访问特定键的值
    cout << "Value for key 2: " << myMap[2] << endl;

    //删除键为1的元素
    myMap.erase(1);

    //再次打印所有元素
    cout << "After erasing key 1:" << endl;
    for (const auto& pair : myMap)
        cout << "Key: " << pair.first << ", Value: " << pair.second << endl;

    return 0;
}
```







### 3.a bitset 容器

#### 3.a.1 基本概念

`bitset` 是 C++ 标准库提供的一种模板类，用于处理固定大小的位集合（`bitset` 是 `"bit set"` 的缩写）或进行整型的二进制输出



**特点：**

1. **固定大小**：`bitset` 的大小必须在编译时确定，例如 `std::bitset<8>` 表示包含 8 位的位集合
2. **位操作支持**：可以通过设置、清除、翻转等操作，灵活地操作位
3. **类型安全**：与普通的整型位操作相比，`bitset` 提供更高的可读性和类型安全性
4. **支持字符串转换**：能够轻松将位集合转为字符串或从字符串初始化





#### 3.a.2 定义与初始化

`bitset` 初始化需要指定位数

```cpp
#include <bitset>
#include <iostream>
using namespace std;

int main()
{
    bitset<8> b1;                      // 默认全为 0：00000000
    bitset<8> b2(42);                  // 从整数初始化：00101010
    bitset<8> b3(string("1101"));      // 从字符串初始化：00001101
    cout << b1 << endl;                // 输出：00000000
    cout << b2 << endl;                // 输出：00101010
    cout << b3 << endl;                // 输出：00001101
    return 0;
}
```





#### 3.a.3 常用函数

```cpp
#include <bitset>
#include <iostream>
using namespace std;

int main()
{
    bitset<8> b(42);			 // 00101010

    cout << b.count() << endl;   // 返回 1 的个数：3
    cout << b.any() << endl;     // 是否至少有一个 1：1 (true)
    cout << b.none() << endl;    // 是否全为 0：0 (false)
    cout << b.test(1) << endl;   // 第 1 位是否为 1：1 (true)

    b.set();                     // 全部置 1：11111111
    cout << b << endl;

    b.reset();                   // 全部清零：00000000
    cout << b << endl;

    b.flip();                    // 翻转位：11111111
    cout << b << endl;

    return 0;
}
```





#### 3.a.4 访问和修改特定位

```cpp
#include <bitset>
#include <iostream>
using namespace std;

int main()
{
    bitset<8> b(42); 		// 00101010

    cout << b[1] << endl;   // 访问第 1 位的值：1
    b[1] = 0;               // 修改第 1 位为 0
    cout << b << endl;      // 输出：00101000

    b.set(3);               // 将第 3 位设置为 1
    cout << b << endl;      // 输出：00111000

    b.reset(3);             // 将第 3 位清零
    cout << b << endl;      // 输出：00101000

    b.flip(2);              // 翻转第 2 位
    cout << b << endl;      // 输出：00101100

    return 0;
}
```





#### 3.a.5 格式转换

```cpp
#include <bitset>
#include <iostream>
using namespace std;

int main()
{
    bitset<8> b(42); // 00101010

    // 转换为字符串
    string binaryString = b.to_string(); 
    cout << binaryString << endl; // 输出：00101010

    // 转换为整数
    unsigned long num = b.to_ulong();
    cout << num << endl; // 输出：42

    return 0;
}
```











## 4 STL- 函数对象

### 4.1 函数对象

#### 4.1.1 函数对象概念

**概念：**

* 重载**函数调用操作符**的类，其对象常称为**函数对象**
* **函数对象**使用重载的()时，行为类似函数调用，也叫**仿函数**



**本质：**

函数对象(仿函数)是一个**类**，不是一个函数



#### 4.1.2  函数对象使用

**特点：**

* 函数对象在使用时，可以像普通函数那样调用, 可以有参数，可以有返回值
* 函数对象超出普通函数的概念，函数对象可以有自己的状态
* 函数对象可以作为参数传递





**示例:**

```C++
#include <string>

//1、函数对象在使用时，可以像普通函数那样调用, 可以有参数，可以有返回值
class MyAdd
{
public :
	int operator()(int v1,int v2)
	{
		return v1 + v2;
	}
};

void test01()
{
	MyAdd myAdd;
	cout << myAdd(10, 10) << endl;
}

//2、函数对象可以有自己的状态
class MyPrint
{
public:
	MyPrint()
	{
		count = 0;
	}
	void operator()(string test)
	{
		cout << test << endl;
		count++; //统计使用次数
	}

	int count; //内部自己的状态
};
void test02()
{
	MyPrint myPrint;
	myPrint("hello world");
	myPrint("hello world");
	myPrint("hello world");
	cout << "myPrint调用次数为： " << myPrint.count << endl;
}

//3、函数对象可以作为参数传递
void doPrint(MyPrint &mp , string test)
{
	mp(test);
}

void test03()
{
	MyPrint myPrint;
	doPrint(myPrint, "Hello C++");
}

int main()
{
	//test01();
	//test02();
	test03();

	system("pause");

	return 0;
}
```

总结：

* 仿函数写法非常灵活，可以作为参数进行传递。













### 4.2  谓词

#### 4.2.1 谓词概念



**概念：**

* 返回bool类型的仿函数称为**谓词**
* 如果operator()接受一个参数，那么叫做一元谓词
* 如果operator()接受两个参数，那么叫做二元谓词





#### 4.2.2 一元谓词

**示例：**

```C++
#include <vector>
#include <algorithm>

//1.一元谓词
struct GreaterFive
{
	bool operator()(int val)
    {
		return val > 5;
	}
};

void test01()
{
	vector<int> v;
	for (int i = 0; i < 10; i++)
		v.push_back(i);

	vector<int>::iterator it = find_if(v.begin(), v.end(), GreaterFive());
	if (it == v.end())
		cout << "没找到!" << endl;
	else
		cout << "找到:" << *it << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：参数只有一个的谓词，称为一元谓词











#### 4.2.3 二元谓词

**示例：**

```C++
#include <vector>
#include <algorithm>
//二元谓词
class MyCompare
{
public:
	bool operator()(int num1, int num2)
	{
		return num1 > num2;
	}
};

void test01()
{
	vector<int> v;
	v.push_back(10);
	v.push_back(40);
	v.push_back(20);
	v.push_back(30);
	v.push_back(50);

	//默认从小到大
	sort(v.begin(), v.end());
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
	cout << "----------------------------" << endl;

	//使用函数对象改变算法策略，排序从大到小
	sort(v.begin(), v.end(), MyCompare());
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：参数只有两个的谓词，称为二元谓词

















### 4.3 内建函数对象

#### 4.3.1 内建函数对象意义

**概念：**

* STL内建了一些函数对象



**分类:**

* 算术仿函数

* 关系仿函数

* 逻辑仿函数

**用法：**

* 这些仿函数所产生的对象，用法和一般函数完全相同
* 使用内建函数对象，需要引入头文件 `#include<functional>`







#### 4.3.2 算术仿函数

**功能描述：**

* 实现四则运算
* 其中negate是一元运算，其他都是二元运算



**仿函数原型：**

* `template<class T> T plus<T>`                //加法仿函数
* `template<class T> T minus<T>`              //减法仿函数
* `template<class T> T multiplies<T>`    //乘法仿函数
* `template<class T> T divides<T>`         //除法仿函数
* `template<class T> T modulus<T>`         //取模仿函数
* `template<class T> T negate<T>`           //取反仿函数



**示例：**

```C++
#include <functional>
//negate
void test01()
{
	negate<int> n;
	cout << n(50) << endl;
}

//plus
void test02()
{
	plus<int> p;
	cout << p(10, 20) << endl;
}

int main()
{
	test01();
	test02();

	system("pause");

	return 0;
}
```

总结：使用内建函数对象时，需要引入头文件 `#include <functional>`









#### 4.3.3 关系仿函数

**功能描述：**

- 实现关系对比



**仿函数原型：**

* `template<class T> bool equal_to<T>`                    //等于
* `template<class T> bool not_equal_to<T>`            //不等于
* `template<class T> bool greater<T>`                      //大于
* `template<class T> bool greater_equal<T>`          //大于等于
* `template<class T> bool less<T>`                           //小于
* `template<class T> bool less_equal<T>`               //小于等于



**示例：**

```C++
#include <functional>
#include <vector>
#include <algorithm>

class MyCompare
{
public:
	bool operator()(int v1,int v2)
	{
		return v1 > v2;
	}
};

void test01()
{
	vector<int> v;

	v.push_back(10);
	v.push_back(30);
	v.push_back(50);
	v.push_back(40);
	v.push_back(20);

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
    {
		cout << *it << " ";
	}
	cout << endl;

	//自己实现仿函数
	//sort(v.begin(), v.end(), MyCompare());
	//STL内建仿函数  大于仿函数
	sort(v.begin(), v.end(), greater<int>());

	for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
    {
		cout << *it << " ";
	}
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：关系仿函数中最常用的就是greater<>大于











#### 4.3.4 逻辑仿函数

**功能描述：**

- 实现逻辑运算



**函数原型：**

* `template<class T> bool logical_and<T>`              //逻辑与
* `template<class T> bool logical_or<T>`                //逻辑或
* `template<class T> bool logical_not<T>`              //逻辑非



**示例：**

```C++
#include <vector>
#include <functional>
#include <algorithm>
void test01()
{
	vector<bool> v;
	v.push_back(true);
	v.push_back(false);
	v.push_back(true);
	v.push_back(false);

	for (vector<bool>::iterator it = v.begin();it!= v.end();it++)
		cout << *it << " ";
	cout << endl;

	//逻辑非  将v容器搬运到v2中，并执行逻辑非运算
	vector<bool> v2;
	v2.resize(v.size());
	transform(v.begin(), v.end(),  v2.begin(), logical_not<bool>());
	for (vector<bool>::iterator it = v2.begin(); it != v2.end(); it++)
		cout << *it << " ";

	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

总结：逻辑仿函数实际应用较少，了解即可







## 5 STL- 常用算法



**概述**:

* 算法主要是由头文件`<algorithm>` `<functional>` `<numeric>`组成。



* `<algorithm>`是所有STL头文件中最大的一个，范围涉及到比较、 交换、查找、遍历操作、复制、修改等等
* `<numeric>`体积很小，只包括几个在序列上面进行简单数学运算的模板函数
* `<functional>`定义了一些模板类,用以声明函数对象。





### 5.1 常用遍历算法

**学习目标：**

* 掌握常用的遍历算法



**算法简介：**

```cpp
for_each  	// 遍历容器
transform	// 搬运容器到另一个容器中
```







#### 5.1.1 for_each

**功能描述：**

* 实现遍历容器

**函数原型：**

```cpp
for_each(iterator beg, iterator end, _func);
// 遍历算法 遍历容器元素
// beg 开始迭代器
// end 结束迭代器
// _func 函数或者函数对象
```





**示例：**

```C++
#include <algorithm>
#include <vector>

//普通函数
void print01(int val) 
{
	cout << val << " ";
}

//函数对象
class print02 
{
 public:
	void operator()(int val) 
	{
		cout << val << " ";
	}
};

//for_each算法基本用法
void test01()
{
	vector<int> v;
	for (int i = 0; i < 10; i++) 
		v.push_back(i);

	//遍历算法
	for_each(v.begin(), v.end(), print01);
	cout << endl;

	for_each(v.begin(), v.end(), print02());
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：**for_each在实际开发中是最常用遍历算法，需要熟练掌握









#### 5.1.2 transform

**功能描述：**

* 搬运容器到另一个容器中

**函数原型：**

```cpp
transform(iterator beg1, iterator end1, iterator beg2, _func);
//beg1 源容器开始迭代器
//end1 源容器结束迭代器
//beg2 目标容器开始迭代器
//_func 函数或者函数对象
```





**示例：**

```C++
#include<vector>
#include<algorithm>

//常用遍历算法  搬运 transform

class TransForm
{
public:
	int operator()(int val)
	{
		return val;
	}

};

class MyPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int>v;
	for (int i = 0; i < 10; i++)
		v.push_back(i);

	vector<int>vTarget; //目标容器

	vTarget.resize(v.size()); // 目标容器需要提前开辟空间

	transform(v.begin(), v.end(), vTarget.begin(), TransForm());

	for_each(vTarget.begin(), vTarget.end(), MyPrint());
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：** 搬运的目标容器必须要提前开辟空间，否则无法正常搬运







### 5.2 常用查找算法

学习目标：

- 掌握常用的查找算法





**算法简介：**

```cpp
find             // 查找元素
find_if          // 按条件查找元素
adjacent_find    // 查找相邻重复元素
binary_search    // 二分查找法
count            // 统计元素个数
count_if         // 按条件统计元素个数
```






#### 5.2.1 find

**功能描述：**

* 查找指定元素，找到返回指定元素的迭代器，找不到返回结束迭代器end()



**函数原型：**

```cpp
find(iterator beg, iterator end, value);
// 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置
// beg 开始迭代器
// end 结束迭代器
// value 查找的元素
```







**示例：**

```C++
#include <algorithm>
#include <vector>
#include <string>
void test01()
{
	vector<int> v;
	for (int i = 0; i < 10; i++)
		v.push_back(i + 1);

	//查找容器中是否有 5 这个元素
	vector<int>::iterator it = find(v.begin(), v.end(), 5);
	if (it == v.end()) 
		cout << "没有找到!" << endl;
	else 
		cout << "找到:" << *it << endl;
}

class Person
{
public:
	Person(string name, int age) 
	{
		this->m_Name = name;
		this->m_Age = age;
	}
	//重载==
	bool operator==(const Person& p) 
	{
		if (this->m_Name == p.m_Name && this->m_Age == p.m_Age) 
			return true;

		return false;
	}

public:
	string m_Name;
	int m_Age;
};

void test02()
{
	vector<Person> v;

	//创建数据
	Person p1("aaa", 10);
	Person p2("bbb", 20);
	Person p3("ccc", 30);
	Person p4("ddd", 40);

	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);

	vector<Person>::iterator it = find(v.begin(), v.end(), p2);
	if (it == v.end()) 
		cout << "没有找到!" << endl;
	else 
		cout << "找到姓名:" << it->m_Name << " 年龄: " << it->m_Age << endl;
}
```

总结： 利用find可以在容器中找指定的元素，返回值是**迭代器**













#### 5.2.2 find_if

**功能描述：**

* 按条件查找元素

**函数原型：**

```cpp
find_if(iterator beg, iterator end, _Pred);
// 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置
// beg 开始迭代器
// end 结束迭代器
// _Pred 函数或者谓词（返回bool类型的仿函数）
```





**示例：**

```C++
#include <algorithm>
#include <vector>
#include <string>

//内置数据类型
class GreaterFive
{
public:
	bool operator()(int val)
	{
		return val > 5;
	}
};

void test01()
{
	vector<int> v;
	for (int i = 0; i < 10; i++)
		v.push_back(i + 1);

	vector<int>::iterator it = find_if(v.begin(), v.end(), GreaterFive());
	if (it == v.end())
		cout << "没有找到!" << endl;
	else
		cout << "找到大于5的数字:" << *it << endl;
}

//自定义数据类型
class Person
{
public:
	Person(string name, int age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}
public:
	string m_Name;
	int m_Age;
};

class Greater20
{
public:
	bool operator()(Person &p)
	{
		return p.m_Age > 20;
	}
};

void test02()
{
	vector<Person> v;

	//创建数据
	Person p1("aaa", 10);
	Person p2("bbb", 20);
	Person p3("ccc", 30);
	Person p4("ddd", 40);

	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);

	vector<Person>::iterator it = find_if(v.begin(), v.end(), Greater20());
	if (it == v.end())
		cout << "没有找到!" << endl;
	else
		cout << "找到姓名:" << it->m_Name << " 年龄: " << it->m_Age << endl;
}

int main()
{
	//test01();

	test02();

	system("pause");

	return 0;
}
```

总结：find_if按条件查找使查找更加灵活，提供的仿函数可以改变不同的策略















#### 5.2.3 adjacent_find

**功能描述：**

* 查找相邻重复元素



**函数原型：**

```cpp
adjacent_find(iterator beg, iterator end);
// 查找相邻重复元素,返回相邻元素的第一个位置的迭代器
// beg 开始迭代器
// end 结束迭代器
```







**示例：**

```C++
#include <algorithm>
#include <vector>

void test01()
{
	vector<int> v;
	v.push_back(1);
	v.push_back(2);
	v.push_back(5);
	v.push_back(2);
	v.push_back(4);
	v.push_back(4);
	v.push_back(3);

	//查找相邻重复元素
	vector<int>::iterator it = adjacent_find(v.begin(), v.end());
	if (it == v.end())
		cout << "找不到!" << endl;
	else
		cout << "找到相邻重复元素为:" << *it << endl;
}
```

总结：面试题中如果出现查找相邻重复元素，记得用STL中的adjacent_find算法









#### 5.2.4 binary_search

**功能描述：**

* 查找指定元素是否存在



**函数原型：**

```cpp
bool binary_search(iterator beg, iterator end, value);

// 查找指定的元素，查到 返回true  否则false
// 注意   在无序序列中不可用
// beg   开始迭代器
// end   结束迭代器
// value  查找的元素
```







**示例：**

```C++
#include <algorithm>
#include <vector>

void test01()
{
	vector<int>v;

	for (int i = 0; i < 10; i++)
		v.push_back(i);

	//二分查找
	bool ret = binary_search(v.begin(), v.end(),2);
	if (ret)
		cout << "找到了" << endl;
	else
		cout << "未找到" << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：**二分查找法查找效率很高，值得注意的是查找的容器中元素必须的有序序列









#### 5.2.5 count

**功能描述：**

* 统计元素个数



**函数原型：**

```cpp
count(iterator beg, iterator end, value);

// 统计元素出现次数
// beg 开始迭代器
// end 结束迭代器
// value 统计的元素
```







**示例：**

```C++
#include <algorithm>
#include <vector>

//内置数据类型
void test01()
{
	vector<int> v;
	v.push_back(1);
	v.push_back(2);
	v.push_back(4);
	v.push_back(5);
	v.push_back(3);
	v.push_back(4);
	v.push_back(4);

	int num = count(v.begin(), v.end(), 4);

	cout << "4的个数为： " << num << endl;
}

//自定义数据类型
class Person
{
public:
	Person(string name, int age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}
	bool operator==(const Person & p)
	{
		if (this->m_Age == p.m_Age)
			return true;
		else
			return false;

	}
	string m_Name;
	int m_Age;
};

void test02()
{
	vector<Person> v;

	Person p1("刘备", 35);
	Person p2("关羽", 35);
	Person p3("张飞", 35);
	Person p4("赵云", 30);
	Person p5("曹操", 25);

	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	v.push_back(p5);
    
    Person p("诸葛亮",35);

	int num = count(v.begin(), v.end(), p);
	cout << "num = " << num << endl;
}

int main()
{
	//test01();

	test02();

	system("pause");

	return 0;
}
```

**总结：** 统计自定义数据类型时候，需要配合重载 `operator==`

















#### 5.2.6 count_if

**功能描述：**

* 按条件统计元素个数

**函数原型：**

```cpp
count_if(iterator beg, iterator end, _Pred);

// 按条件统计元素出现次数
// beg 开始迭代器
// end 结束迭代器
// _Pred 谓词
```





**示例：**

```C++
#include <algorithm>
#include <vector>

class Greater4
{
public:
	bool operator()(int val)
	{
		return val >= 4;
	}
};

//内置数据类型
void test01()
{
	vector<int> v;
	v.push_back(1);
	v.push_back(2);
	v.push_back(4);
	v.push_back(5);
	v.push_back(3);
	v.push_back(4);
	v.push_back(4);

	int num = count_if(v.begin(), v.end(), Greater4());

	cout << "大于4的个数为： " << num << endl;
}

//自定义数据类型
class Person
{
public:
	Person(string name, int age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}

	string m_Name;
	int m_Age;
};

class AgeLess35
{
public:
	bool operator()(const Person &p)
	{
		return p.m_Age < 35;
	}
};

void test02()
{
	vector<Person> v;

	Person p1("刘备", 35);
	Person p2("关羽", 35);
	Person p3("张飞", 35);
	Person p4("赵云", 30);
	Person p5("曹操", 25);

	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	v.push_back(p5);

	int num = count_if(v.begin(), v.end(), AgeLess35());
	cout << "小于35岁的个数：" << num << endl;
}


int main()
{
	//test01();

	test02();

	system("pause");

	return 0;
}
```

**总结：**按值统计用count，按条件统计用count_if













### 5.3 常用排序算法

**学习目标：**

- 掌握常用的排序算法

**算法简介：**

```cpp
sort             // 对容器内元素进行排序
random_shuffle   // 洗牌   指定范围内的元素随机调整次序
merge            // 容器元素合并, 并存储到另一容器中
reverse          // 反转指定范围的元素
```







#### 5.3.1 sort

**功能描述：**

* 对容器内元素进行排序





**函数原型：**

```cpp
sort(iterator beg, iterator end, _Pred);

// 按值查找元素, 找到返回指定位置迭代器, 找不到返回结束迭代器位置
//  beg    开始迭代器
//  end    结束迭代器
// _Pred   谓词
```







**示例：**

```c++
#include <algorithm>
#include <vector>

void myPrint(int val)
{
	cout << val << " ";
}

void test01()
{
	vector<int> v;
	v.push_back(10);
	v.push_back(30);
	v.push_back(50);
	v.push_back(20);
	v.push_back(40);

	//sort默认从小到大排序
	sort(v.begin(), v.end());
	for_each(v.begin(), v.end(), myPrint);
	cout << endl;

	//从大到小排序, 使用小根堆进行排序
	sort(v.begin(), v.end(), greater<int>());
	for_each(v.begin(), v.end(), myPrint);
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：**sort属于开发中最常用的算法之一，需熟练掌握













#### 5.3.2 random_shuffle

**功能描述：**

* 洗牌   指定范围内的元素随机调整次序



**函数原型：**

```cpp
random_shuffle(iterator beg, iterator end);
// 指定范围内的元素随机调整次序
// beg 开始迭代器
// end 结束迭代器
```





**示例：**

```c++
#include <algorithm>
#include <vector>
#include <ctime>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	srand((unsigned int)time(NULL));
	vector<int> v;
	for(int i = 0 ; i < 10;i++)
		v.push_back(i);

	for_each(v.begin(), v.end(), myPrint());
	cout << endl;

	//打乱顺序
	random_shuffle(v.begin(), v.end());
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：**random_shuffle洗牌算法比较实用，使用时记得加随机数种子















#### 5.3.3 merge

**功能描述：**

* 两个容器元素合并，并存储到另一容器中



**函数原型：**

```cpp
merge(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);
// 容器元素合并，并存储到另一容器中
// 注意    两个容器必须是有序的
// beg1   容器1开始迭代器
// end1   容器1结束迭代器
// beg2   容器2开始迭代器
// end2   容器2结束迭代器
// dest   目标容器开始迭代器
```





**示例：**

```c++
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10 ; i++) 
    {
		v1.push_back(i);
		v2.push_back(i + 1);
	}

	vector<int> vtarget;
	//目标容器需要提前开辟空间
	vtarget.resize(v1.size() + v2.size());
	//合并  需要两个有序序列
	merge(v1.begin(), v1.end(), v2.begin(), v2.end(), vtarget.begin());
	for_each(vtarget.begin(), vtarget.end(), myPrint());
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：**merge合并的两个容器必须的有序序列











#### 5.3.4 reverse

**功能描述：**

* 将容器内元素进行反转



**函数原型：**

```cpp
reverse(iterator beg, iterator end);
// 反转指定范围的元素
// beg 开始迭代器
// end 结束迭代器
```





**示例：**

```c++
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v;
	v.push_back(10);
	v.push_back(30);
	v.push_back(50);
	v.push_back(20);
	v.push_back(40);

	cout << "反转前： " << endl;
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;

	cout << "反转后： " << endl;

	reverse(v.begin(), v.end());
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：**reverse反转区间内元素，面试题可能涉及到









### 5.4 常用拷贝和替换算法

**学习目标：**

- 掌握常用的拷贝和替换算法

**算法简介：**

```cpp
copy        // 容器内指定范围的元素拷贝到另一容器中
replace     // 将容器内指定范围的旧元素修改为新元素
replace_if	// 容器内指定范围满足条件的元素替换为新元素
swap        // 互换两个容器的元素
```






#### 5.4.1 copy

**功能描述：**

* 容器内指定范围的元素拷贝到另一容器中



**函数原型：**

```cpp
copy(iterator beg, iterator end, iterator dest);
// 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置
// beg  开始迭代器
// end  结束迭代器
// dest 目标起始迭代器
```





**示例：**

```c++
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	for (int i = 0; i < 10; i++)
		v1.push_back(i + 1);

	vector<int> v2;
	v2.resize(v1.size());
	copy(v1.begin(), v1.end(), v2.begin());

	for_each(v2.begin(), v2.end(), myPrint());
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：**利用copy算法在拷贝时，目标容器记得提前开辟空间















#### 5.4.2 replace

**功能描述：**

* 将容器内指定范围的旧元素修改为新元素



**函数原型：**

```cpp
replace(iterator beg, iterator end, oldvalue, newvalue);
// 将区间内旧元素 替换成 新元素
// beg 开始迭代器
// end 结束迭代器
// oldvalue 旧元素
// newvalue 新元素
```





**示例：**

```c++
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v;
	v.push_back(20);
	v.push_back(30);
	v.push_back(20);
	v.push_back(40);
	v.push_back(50);
	v.push_back(10);
	v.push_back(20);

	cout << "替换前：" << endl;
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;

	//将容器中的20 替换成 2000
	cout << "替换后：" << endl;
	replace(v.begin(), v.end(), 20,2000);
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：**replace会替换区间内满足条件的元素













#### 5.4.3 replace_if

**功能描述:**  

* 将区间内满足条件的元素，替换成指定元素



**函数原型：**

```cpp
replace_if(iterator beg, iterator end, _pred, newvalue);
// 按条件替换元素，满足条件的替换成指定元素
// beg 开始迭代器
// end 结束迭代器
// _pred 谓词
// newvalue 替换的新元素
```





**示例：**

```c++
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

class ReplaceGreater30
{
public:
	bool operator()(int val)
	{
		return val >= 30;
	}

};

void test01()
{
	vector<int> v;
	v.push_back(20);
	v.push_back(30);
	v.push_back(20);
	v.push_back(40);
	v.push_back(50);
	v.push_back(10);
	v.push_back(20);

	cout << "替换前：" << endl;
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;

	//将容器中大于等于的30 替换成 3000
	cout << "替换后：" << endl;
	replace_if(v.begin(), v.end(), ReplaceGreater30(), 3000);
	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：**replace_if按条件查找，可以利用仿函数灵活筛选满足的条件







#### 5.4.4 swap

**功能描述：**

* 互换两个容器的元素



**函数原型：**

```cpp
swap(container c1, container c2);
// 互换两个容器的元素
// c1容器1
// c2容器2
```





**示例：**

```c++
#include <algorithm>
#include <vector>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10; i++)
    {
		v1.push_back(i);
		v2.push_back(i+100);
	}

	cout << "交换前： " << endl;
	for_each(v1.begin(), v1.end(), myPrint());
	cout << endl;
	for_each(v2.begin(), v2.end(), myPrint());
	cout << endl;

	cout << "交换后： " << endl;
	swap(v1, v2);
	for_each(v1.begin(), v1.end(), myPrint());
	cout << endl;
	for_each(v2.begin(), v2.end(), myPrint());
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：**swap交换容器时，注意交换的容器要同种类型













### 5.5 常用算术生成算法

**学习目标：**

- 掌握常用的算术生成算法



**注意：**

* 算术生成算法属于小型算法，使用时包含的头文件为 `#include <numeric>`



**算法简介：**

```cpp
accumulate		// 计算容器元素累计总和
fill            // 向容器中添加元素
```





#### 5.5.1 accumulate

**功能描述：**

*  计算区间内 容器元素累计总和



**函数原型：**

```cpp
accumulate(iterator beg, iterator end, value);
// 计算容器元素累计总和
// beg 开始迭代器
// end 结束迭代器
// value 起始值
```





**示例：**

```c++
#include <numeric>
#include <vector>
void test01()
{
	vector<int> v;
	for (int i = 0; i <= 100; i++)
		v.push_back(i);

	int total = accumulate(v.begin(), v.end(), 0);

	cout << "total = " << total << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：**accumulate使用时头文件注意是 numeric，这个算法很实用



#### 5.5.2 fill

**功能描述：**

* 向容器中填充指定的元素



**函数原型：**

```cpp
fill(iterator beg, iterator end, value);
// 向容器中填充元素
// beg 开始迭代器
// end 结束迭代器
// value 填充的值
```





**示例：**

```c++
#include <numeric>
#include <vector>
#include <algorithm>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{

	vector<int> v;
	v.resize(10);
	//填充
	fill(v.begin(), v.end(), 100);

	for_each(v.begin(), v.end(), myPrint());
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：**利用fill可以将容器区间内元素填充为 指定的值





### 5.6 常用集合算法

**学习目标：**

- 掌握常用的集合算法



**算法简介：**

```cpp
set_intersection	// 求两个容器的交集
set_union           // 求两个容器的并集
set_difference      // 求两个容器的差集
```







#### 5.6.1 set_intersection

**功能描述：**

* 求两个容器的交集



**函数原型：**

```cpp
set_intersection(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);
// 求两个集合的交集
// 注意  两个集合必须是有序序列
// beg1 容器1开始迭代器
// end1 容器1结束迭代器
// beg2 容器2开始迭代器
// end2 容器2结束迭代器
// dest 目标容器开始迭代器
```





**示例：**

```C++
#include <vector>
#include <algorithm>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10; i++)
    {
		v1.push_back(i);
		v2.push_back(i+5);
	}

	vector<int> vTarget;
	//取两个里面较小的值给目标容器开辟空间
	vTarget.resize(min(v1.size(), v2.size()));

	//返回目标容器的最后一个元素的迭代器地址
	vector<int>::iterator itEnd = 
        set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());

	for_each(vTarget.begin(), itEnd, myPrint());
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：** 

* 求交集的两个集合必须的有序序列
* 目标容器开辟空间需要从**两个容器中取小值**
* set_intersection返回值既是交集中最后一个元素的位置













#### 5.6.2 set_union

**功能描述：**

* 求两个集合的并集



**函数原型：**

```cpp
set_union(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);
// 求两个集合的并集
// 注意 两个集合必须是有序序列
// beg1 容器1开始迭代器
// end1 容器1结束迭代器
// beg2 容器2开始迭代器
// end2 容器2结束迭代器
// dest 目标容器开始迭代器
```





**示例：**

```C++
#include <vector>
#include <algorithm>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10; i++)
    {
		v1.push_back(i);
		v2.push_back(i+5);
	}

	vector<int> vTarget;
	//取两个容器的和给目标容器开辟空间
	vTarget.resize(v1.size() + v2.size());

	//返回目标容器的最后一个元素的迭代器地址
	vector<int>::iterator itEnd = 
        set_union(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());

	for_each(vTarget.begin(), itEnd, myPrint());
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：** 

- 求并集的两个集合必须的有序序列
- 目标容器开辟空间需要**两个容器相加**
- set_union返回值既是并集中最后一个元素的位置








#### 5.6.3  set_difference

**功能描述：**

* 求两个集合的差集



**函数原型：**

```cpp
set_difference(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);
// 求两个集合的差集
// 注意 两个集合必须是有序序列
// beg1 容器1开始迭代器
// end1 容器1结束迭代器
// beg2 容器2开始迭代器
// end2 容器2结束迭代器
// dest 目标容器开始迭代器
```





**示例：**

```C++
#include <vector>
#include <algorithm>

class myPrint
{
public:
	void operator()(int val)
	{
		cout << val << " ";
	}
};

void test01()
{
	vector<int> v1;
	vector<int> v2;
	for (int i = 0; i < 10; i++)
    {
		v1.push_back(i);
		v2.push_back(i+5);
	}

	vector<int> vTarget;
	//取两个里面较大的值给目标容器开辟空间
	vTarget.resize( max(v1.size() , v2.size()));

	//返回目标容器的最后一个元素的迭代器地址
	cout << "v1与v2的差集为： " << endl;
	vector<int>::iterator itEnd = 
        set_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());
	for_each(vTarget.begin(), itEnd, myPrint());
	cout << endl;


	cout << "v2与v1的差集为： " << endl;
	itEnd = set_difference(v2.begin(), v2.end(), v1.begin(), v1.end(), vTarget.begin());
	for_each(vTarget.begin(), itEnd, myPrint());
	cout << endl;
}

int main()
{
	test01();

	system("pause");

	return 0;
}
```

**总结：** 

- 求差集的两个集合必须的有序序列
- 目标容器开辟空间需要从**两个容器取较大值**
- set_difference返回值既是差集中最后一个元素的位置







### 5.7 其它算法

#### 5.7.1 unique

`unique` 的作用是去除数组中相邻的重复元素，使得序列中相邻的元素保持唯一，可以和 `sort` 排序函数联合使用

`unique` **不会真正删除容器中的元素**，它只是将不重复的元素移动到前面，并返回一个新的 **结束迭代器**，表示去重后的范围

要真正删除重复元素，通常需要配合 `erase` 使用



**示例：**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main()
{
    std::vector<int> vec = { 1, 1, 2, 2, 2, 3, 4, 4, 5 };

    // unique 只会移动数据, 不会真正删除元素
    auto end1 = std::unique(vec.begin(), vec.end());
    for (int num : vec)
        std::cout << num << " ";
    // 1 2 3 4 5 3 4 4 5

    std::cout << std::endl;

    // 通过 erase 彻底删除多余的元素
    vec.erase(end1, vec.end());
    for (int num : vec)
        std::cout << num << " ";
    // 1 2 3 4 5

    std::cout << std::endl;

    int arr[9] = {1, 1, 2, 2, 2, 3, 4, 4, 5};
    int end2 = std::unique(arr, arr + 9) - arr;
    for (int i = 0; i < end2; ++i)
        std::cout << arr[i] << " ";
    // 1 2 3 4 5
    std::cout << std::endl;

    return 0;
}
```







#### 5.7.2 lower_bound

`lower_bound` 是一个二分查找函数，用于在==有序序列==（先排序）中查找==第一个==大于或等于目标值的位置



`lower_bound` 会返回一个迭代器，指向第一个不小于（≥）给定值的元素
 如果所有元素都小于目标值，则返回 `last`（即超出范围的迭代器）



**示例：**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main()
{
    std::vector<int> vec = { 1, 3, 5, 7, 9 };

    auto it = std::lower_bound(vec.begin(), vec.end(), 6);
    int index = std::lower_bound(vec.begin(), vec.end(), 6) - vec.begin() + 1;

    if (it != vec.end())
        std::cout << "二分结果为：" << *it << ", 其下标为：" << index << std::endl;
    else
        std::cout << "所有元素都小于 6" << std::endl;
    // 二分结果为：7, 其下标为：4

    return 0;
}
```



`lower_bound` 和 `upper_bound` 之间的区别

| 目标值 `x` | `lower_bound(x)`              | `upper_bound(x)` 返回      |
| ---------- | ----------------------------- | -------------------------- |
| 存在 `x`   | 返回第一个==等于== `x` 的位置 | 返回第一个大于 `x` 的位置  |
| 不存在 `x` | 返回第一个大于 `x` 的位置     | 返回第一个大于 `>x` 的位置 |







#### 5.7.3 gcd 和 lcm

`gcd` 函数来计算两个数的最大公约数（Greatest Common Divisor，GCD），来自于标准库 `<numeric>` 中

接受两个整数作为参数，并返回它们的最大公约数



`lcm` 函数计算两个数的最小公倍数（Least Common Multiple，LCM），与 `gcd ` 函数类似，也是通过 `<numeric>` 头文件提供的

但我们可以不使用该函数，可以通过 `a*b/gcd(a,b)` 来得到 `lcm` 的值



```cpp
#include <iostream>
#include <numeric>
using namspace std;

int main()
{
    int a = 56;
    int b = 98;
    cout << "56 与 98 的最大公约数为:" << gcd(a, b) << endl;
    cout << "56 与 98 的最小公倍数为:" << a * b / gcd(a,b) << endl;
    return 0;
}
```







#### 5.7.4 move

`move` 位于 `<utility>` 头文件中

主要作用是将一个 **左值** 转换为 **右值引用**，从而 **启用移动语义**（不会真的移动对象），避免不必要的深拷贝（不重新分配内存），提高程序性能



函数声明：

```cpp
template <class T>
typename std::remove_reference<T>::type&& move(T&& t) noexcept;
```



**例：**

```cpp
#include <iostream>
#include <vector>
#include <utility>
using namespace std;

int main()
{
    vector<int> a(10);
    for (int i = 0; i < 10; ++i)
        a[i] = i;

    vector<int> b = move(a);    // 将 a 的值转移给 b
    for (int i = 0; i < 10; ++i)
        cout << b[i] << " ";
    cout << endl;
    // 0 1 2 3 4 5 6 7 8 9

    // 转移后不可访问
    // for (int i = 0; i < 10; ++i)
    //     cout << a[i] << " ";
    // cout << endl;

    // 转移后可以对源对象重新赋值, 且不影响转移后的数据
    a = vector<int>(10, 0);
    for (int i = 0; i < 10; ++i)
        cout << a[i] << " ";
    cout << endl;
    // 0 0 0 0 0 0 0 0 0 0
}
```



注意事项：

1. 仅转换，不会移动数据

```cpp
int a = 10;
int b = move(a); // 只是拷贝 a, a 仍然是 10
```

- `move(a)` **不会真正移动 a**，因为 `int` 是 **基本数据类型**，不会触发移动语义



2. 移动后，源对象仍然存在，但不可再使用

```cpp
MyClass a(10);
MyClass b = move(a);
cout << "a 的数据大小: " << a.data.size() << endl; // 不可预测
```

- `a` 仍然存在，但 **其内部资源已被移动**，**不能再访问**

3. 可以，但不要对已经移动过的对象重复使用

```cpp
MyClass a(10);
MyClass b = std::move(a);
a = MyClass(5); // a 之前的资源已经转移, 不会影响 b
```









#### 5.7.5 ______builtin_clzll 和______builtin_ctzll

`__builtin_clzll` 和 `__builtin_ctzll` 是==GCC 和 Clang 提供的内建函数==（VS 不能使用）

用于计算一个 64 位无符号整数的前导零（leading zeros）和尾部零（trailing zeros）个数



函数原型：

```cpp
int __builtin_clzll(unsigned long long x);	// 前导0
int __builtin_ctzll(unsigned long long x);	// 尾部0
```

- 参数：一个 无符号 64 位整数（`unsigned long long`）
- 时间复杂度：`O(1)`，非常高效
- 返回值：
    * `clz`：返回 `x` 的二进制表示中，前导零的个数 （`x == 0`，返回的值为未定义行为，不同编译器可能有不同实现）
    * `ctz`：返回 `x` 的二进制表示中，从最低位到第一个 `1` 之前的零的个数（`x == 0` 也未定义）



```cpp
#include <iostream>

using namespace std;

int main()
{
    unsigned long long x = 8;
    // 二进制: 0000...00001000 (前导零60个)
    cout << "__builtin_clzll(8) = " << __builtin_clzll(x) << endl;

    x = 1;
    // 二进制: 0000...00000001 (前导零63个)
    cout << "__builtin_clzll(1) = " << __builtin_clzll(x) << endl;

    x = 1024;
    // 二进制: 0000...10000000000 (前导零53个)
    cout << "__builtin_clzll(1024) = " << __builtin_clzll(x) << endl;
    
    x = 0;
    // 二进制: 0000...0000 (前导零64个)
	cout << __builtin_clzll(x) << endl;  // 可能崩溃或返回64（不同平台可能不同）

    return 0;
}
```



应用：

* 计算最高位：`MSB = 63 - __builtin_clzll(x)`
* 判断 `x` 是否是 2 的幂：` clz(x) + ctz(x) == 63`











# c++知识补充

## 1 编译预处理

### 1.1 预处理使用

#### 1.1.1 #inclued

用法：用于包含一个文件的内容，通常是头文件

```c++
#include <bits_stdc++.h>			//包含所有头文件的总头文件,编译器自带的头文件用 < > 来表示
#include "myheader.h"				//自己定义的头文件,自定义的头文件用 " " 来表示
```



#### 1.1.2 #define

用法：用于定义宏，可以是对象宏（常量）或函数宏（代码块）

```c++
#define PI 3.1415926				//定义常量π
#define SQUARE(x) ((x) * (x))		//定义平方函数宏
```





>   相关用法：运算符 # 和 运算符 ##	( 令牌  是指宏定义中的宏被替换成的文本 replacement-tex )
>
>   *   `#`   运算符将令牌转换为用引号引起来的字符串
>   *   `##` 运算符用于连接两个令牌



例：

```c++
#define MKSTR( x ) #x
#define CONCAT( x, y )  x ## y

void test()
{
    string hello = "hello ";
    cout << CONCAT(he, llo) << MKSTR(world!) << endl;
}
//hello world!
```





#### 1.1.3 #undef

用法：用于取消之前定义的宏

```c++
#define PI 3.1415926				//定义常量π
#undef PI							//取消对于π的定义
```



#### 1.1.4 #ifdef，#ifndef，#endif

*   #ifdef：用于条件编译，如果指定的宏**被定义**，则编译其后的代码块
*   #ifndef：用于条件编译，如果指定的宏**没有被定义**，则编译其后的代码块
*   #endif：用于结束一个条件编译指令块（ #if，#ifdef，#ifndef ）

```c++
#ifndef PI								//如果没有定义π
#define PI 3.1415926					//定义常量π
#endif									//结束 #ifndef 的编译

#ifdef PI								//如果π被定义
printf("π is defined\n");				//输出结果
#endif									//结束 #ifdef 的编译
```



#### 1.1.5 #if，#else，#elif

*   #if：用于条件编译，根据给定的表达式是否为真来决定是否编译其后的代码块
*   #elif：用于条件编译，相当于 else if，可以检查另一个条件
*   #else：与 #if、#ifdef 或 #ifndef 配合使用，如果条件为假则编译 #else 后的代码块

```c++
#define VERSION 2						//定义一个常量
#if VERSION == 1						//类似于 if 语句判断
printf("Version 1\n");
#elif VERSION == 2						//类似于 else if 语句判断
printf("Version 2\n");
#else									//类似于 else 语句判断
printf("Other version\n");
#endif									//结束 #if 的编译
```



#### 1.1.6 #error

用法：用于在编译时生成错误信息并停止编译

```c++
#if __STDC_VERSION__ < 199901L			//强制要求代码在特定版本的编译器下编译
#error "This code requires at least C99 standard."
#endif									//结束 #if 的编译
```



#### 1.1.7 #pragma

用法：提供给编译器的特殊指令，通常用于启用或禁用特定的编译器功能

```c++
#pragma once                       		//防止头文件被多次包含

//#pragma warning 用于控制编译器警告的行为
#pragma warning(disable: warning_code)	//禁用特定的警告
#pragma warning(error: warning_code)	//将警告视为错误

#pragma comment (lib,"Winmm.lib")		//告诉编译器将 Winmm.lib 库文件链接到程序中
//Winmm.lib 是 Windows 多媒体 API 的静态库文件,包含了许多与音频,视频,MIDI等多媒体功能相关的函数
```

注意：不同编译器可能支持不同的 #pragma 指令，具体用法请参考编译器文档







### 1.2 预定义宏

预定义宏：这些宏由编译器自动定义，提供编译环境的信息



#### 1.2.1 `__FILE__`

含义：当前文件名

```c++
printf("This file is %s\n", __FILE__);			//输出当前文件名,带后缀名
```



#### 1.2.2 `__LINE__`

含义：当前行号

```c++
printf("This is line %d\n", __LINE__);			//输出当前行号,一个整形
```



#### 1.2.3 `__DATE__`

含义：编译的日期

```c++
printf("Compilation date: %s\n", __DATE__);		//输出编译日期,以 mon dd yyyy 的形式
```



#### 1.2.4 `__TIME__`

含义：编译的时间

```c++
printf("Compilation time: %s\n", __TIME__);		//输出当编译时间,以 hh:mm:ss 的形式
```



#### 1.2.5 `__cplusplus`

含义：如果正在使用C++编译器，则此宏被定义



>   相关用法：extern "C"
>
>   定义：`extern "C"` 是一个用于C++代码与C代码进行互操作的关键字，它告诉C++编译器使用C语言的链接规范来编译指定的代码块
>
>   
>
>   * 在C++中，函数名和变量名在编译过程中会被修改（name mangling），以支持函数重载等特性
>
>       而C语言没有这些特性，因此不会对名称进行修改
>
>   * 如果试图直接从C++调用C语言的函数，或者反过来，不使用  extern "C"  可能会导致链接错误，因为符号名称不匹配
>
>       通过使用  extern "C" ，可以避免这些问题



例：

```c++
#ifdef __cplusplus		//如果是c++版本,加入一个{
extern "C" {
#endif

void my_function(int x);

#ifdef __cplusplus		//如果是c++版本,加入一个}
}
#endif
```



#### 1.2.6 `__STDC_VERSION__`

含义：表示C标准的版本号



常见的 `__STDC_VERSION__` 值：

-   C90：未定义	( 也称为ANSI C ，该版本定义了 `__STDC__` 为1 )
-   C94：未定义	( ISO C90的修订版 )
-   C99：定义为  199901L 
-   C11：定义为  201112L 
-   C18：定义为  201710L 

```c++
#ifdef __STDC_VERSION__						
	#if __STDC_VERSION__ >= 201112L
		printf("C11 or later\n");
	#elif __STDC_VERSION__ >= 199901L
		printf("C99\n");
	#else
		printf("C90 or earlier\n");
	#endif
#else
	printf("C90 or earlier\n");
#endif
```

在这个示例中，程序会根据 `__STDC_VERSION__` 的值打印对应的C标准版本：

-   如果编译器支持C11或更高版本，会打印  C11 or later
-   如果编译器支持C99，但未达到C11，会打印  C99
-   如果编译器不支持C99或更高版本，并且 `__STDC_VERSION__` 未定义，会打印  C90 or earlier











## 2 命名空间

### 2.1 namespace

>   概念：
>
>   在C/C++中，变量、函数和后面要学到的类都是大量存在的
>
>   这些变量、函数和类的名称将都存在于全局作用域中，可能会导致很多冲突
>
>   使用命名空间的目的是对标识符的名称进行本地化，以避免命名冲突或名字污染



定义：`namespace + 命名空间名称 + { 命名空间成员 }`



例：

```c++
//第一个命名空间
namespace first_space
{
    void func()
    {
        cout << "命名空间 first_space" << endl;
    }
}

//第二个命名空间
namespace second_space
{
    void func()
    {
        cout << "命名空间 second_space" << endl;
    }
}

//无命名空间
void func()
{
    cout << "命名空间 无" << endl;
}

void test()
{
    first_space::func();		//命名空间 first_space

    second_space::func();		//命名空间 second_space

    func();						//命名空间 无
}
```







### 2.2 using

#### 2.2.1 设置命名空间

可以使用 using namespace 指令，这样在使用命名空间时就可以不用在前面加上命名空间的名称



用法：`using namespace + 命名空间名称`

作用：告诉编译器，后续的代码将使用指定的命名空间中的名称



例：

```c++
//第一个命名空间
namespace first_space
{
    void func()
    {
        cout << "命名空间 first_space" << endl;
    }
}

//第二个命名空间
namespace second_space
{
    void func()
    {
        cout << "命名空间 second_space" << endl;
    }
}

using namespace first_space;
//不能再创建无命名空间的 func 函数,会导致函数重载冲突

void test()
{
    first_space::func();		//命名空间 first_space

    second_space::func();		//命名空间 second_space

    func();						//命名空间 first_space
}
```







#### 2.2.2 起别名

`using` 有着等价于 `typedef` 的作用，可以给变量类型起别名，但 `using` 语法更直观，而且可以用于模板（`typedef` 不能）



例：

```cpp
// 算法模板
#include <bits/stdc++.h>
using namespace std;

using ll = long long;
using ull = unsigned long long;

signed main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    
    /* code */

    return 0;
}
```









### 2.3 std命名空间

std命名空间是C++标准库（Standard Library）中定义的一个命名空间，用于包含一系列标准库提供的类、函数和对象

命名空间提供了一种组织和封装代码的机制，以避免命名冲突，并将相关的实体进行逻辑上的分组



以下是std命名空间中一些常用的函数：

​	1. 输入输出流函数：	包含在 `<iostream>` 头文件中

-   `cout`：用于标准输出，可以输出各种类型的数据
-   `cin`：用于标准输入，可以接收各种类型的数据
-   `endl`：用于输出换行符并刷新输出缓冲区



​	2. 字符串处理函数：	包含在 `<cstring>` 头文件中

-   `strlen`：用于计算字符串的长度
-   `strcpy`：用于将一个字符串复制到另一个字符串
-   `strcat`：用于将一个字符串连接到另一个字符串的末尾
-   `strcmp`：用于比较两个字符串



​	3. 数学函数：	包含在 `<cmath>` 头文件中

-   `abs`：用于计算整数的绝对值
-   `sqrt`：用于计算一个数的平方根
-   `pow`：用于计算一个数的幂
-   `sin`：用于计算正弦值
-   `cos`：用于计算余弦值



​	4. 容器和算法函数：	包含在 `<vector>` 等 stl 头文件中

-   vector：用于创建和操作动态数组
-   list：用于创建和操作双向链表
-   sort：用于对容器中的元素进行排序
-   find：用于在容器中查找指定元素
-   erase：用于从容器中删除指定元素



`using namespace std` 是一条特殊的语句，用于指示编译器使用标准命名空间 ( std )



注意：过度使用 `using namespace std` 可能会导致命名冲突或者不明确的引用

因此，在大型项目中，最好只在需要的地方使用 `using` 语句，或者使用命名空间前缀来明确指定标识符的来源



参考资料：https://en.cppreference.com/w/cpp/header











##  3 内存管理库

`<memory>` 是 C++ 标准库中的一个头文件，它包含了用于动态内存管理的模板和函数

其中常用且重要的有 **智能指针** 和 **分配器**，可以更安全地管理动态内存

* 动态内存管理允许程序在运行时分配和释放内存，这在处理不确定大小的数据结构时非常有用

    然而，不正确的内存管理可能导致内存泄漏、野指针等问题



### 3.1 智能指针

原始指针和智能指针的区别：

>   原始指针：需要使用 new/malloc 分配内存，使用 delete/free 释放内存，容易出错但更为方便
>
>   智能指针：封装后的原始指针，是一个对象类，优点是可以自动分配内存，不需要担心潜在内存泄漏



智能指针的主要类型有：

-   `std::unique_ptr`：独占所有权的智能指针，同一时间只能有一个 `unique_ptr` 指向特定内存
-   `std::shared_ptr`：共享所有权的智能指针，多个 `shared_ptr` 可以指向同一内存，内存在最后一个 `shared_ptr` 被销毁时释放
-   `std::weak_ptr`：弱引用智能指针，用于与 `shared_ptr` 配合使用，避免循环引用导致的内存泄漏



####　3.1.1 独占指针 unique_ptr

指针特点：

* 对象独占动态分配的内存，同一时间只能有一个，即其他指针不能和unique_ptr同时指向同一块动态分配的内存

* 当指针超出作用域时，内存将自动释放，且可以接受自定义的删除器函数或函数对象来进行删除

* 该类型指针不可复制操作，只可以移动操作，通过 `move` 函数

    ` unique_ptr<int> ptr2 = move(ptr1)`



创建方式：

*   通过已有裸指针创建
*   通过new来创建
*   通过`make_unique创建` ( 推荐 )



获取地址：

*   通过 `get()` 获取地址
*   通过 `*` 对地址解引用
*   通过 `->` 调用成员函数和访问成员变量

```c++
//创建指针
// 1.通过已有裸指针创建
int* rawPtr = new int(42);
unique_ptr<int> uniquePtr1(rawPtr);
// 2.通过new来创建
unique_ptr<int> uniquePtr2(new int(42));
// 3.通过std::make_unique创建
unique_ptr<int> uniquePtr3 = make_unique<int>(42);

//自定义的删除器
void custom_deleter(int* p)		//删除函数
{
    cout << "Using custom deleter" << endl;
    free(p);
}
unique_ptr<int, decltype(&custom_deleter)> uniquePtr4(new int(42), &custom_deleter);
```



函数调用时的注意事项：

*   不要以引用传递 `unique_ptr`
    *   `unique_ptr` 是独占式的，不应该通过引用的方式传递给函数
    *   因为这会导致多个 `unique_ptr` 指向同一块内存，违反了 `unique_ptr` 的设计初衷
    *   正确的做法是通过值传递 `unique_ptr`。

```c++
void foo(unique_ptr<int> ptr)	//void foo(unique_ptr<int>& ptr) 不能通过引用的方式传递
{
    /* code */
}

int main()
{
    unique_ptr<int> uniquePtr = make_unique<int>(42);
    foo(move(uniquePtr)); //将 uniquePtr 移动到函数中,之后只能在函数中使用该指针所指的地址
    //cout<<*uniquePtr<<endl;
    //uniquePtr 在此处已经被移动,不再拥有原始指针,现在 uniquePtr 为空指针
}
```



*   返回 `unique_ptr` 时需要小心
    *   当函数返回 `unique_ptr` 时，调用者会获得该指针的所有权
    *   如果在函数内部使用 `new` 创建了动态内存，需要确保在返回之前正确地将其转移给返回的 `unique_ptr`

```c++
unique_ptr<int> createInt()
{
    unique_ptr<int> ptr(new int(42));
    return ptr; //所有权被转移给调用者
}

int main()
{
    unique_ptr<int> uniquePtr = createInt();
    // uniquePtr 现在拥有从 createInt() 返回的指针
}
```



*   移动而不是复制 `unique_ptr`
    *   `unique_ptr` 是不可复制的，但是可以被移动
    *   当需要将 `unique_ptr` 传递给函数或在赋值时，应该使用 `move` 来移动所有权,而不是试图复制它

```c++
int main()
{
    unique_ptr<int> uniquePtr1 = make_unique<int>(42);
    unique_ptr<int> uniquePtr2 = move(uniquePtr1);
    //uniquePtr1 不再拥有原始指针,uniquePtr2 现在拥有该指针
}
```







####　3.1.2 计数指针 shared_ptr

`shared_ptr` 又叫共享指针

指针特点：

* 对象之间可以共享数据的，可以将地址复制给其他对象，如果数据是对象，只会调用一次析构函数

* 创建了一个计数器与对象所指的内存相关联，代表了指向该地址的指针数量

    复制则计数器加一，销毁则计数器减一，计数器的 api 为 `use_count()`

* 不适用于管理动态数组,因为它只能管理单个对象

    对于动态数组，应该使用 `unique_ptr` 或自定义的删除器



创建方式：( 和 unique_ptr 指针一样，仅函数名不同 )

*   通过已有裸指针创建
*   通过new来创建
*   通过 `make_shared创建` ( 推荐 )



数据销毁：

* 通过 `reset()` 函数或让指针指向 `nullptr`

    销毁和指向空的指针，计算器为 0



```cpp
int main()
{
    shared_ptr<int> ptr1 = make_shared<int>(42);
    shared_ptr<int> ptr2 = ptr1;
    shared_ptr<int> ptr3 = ptr1;
    ptr3 = nullptr;

    cout << "ptr1 use count: " << ptr1.use_count() << endl;	//3
    cout << "ptr2 use count: " << ptr2.use_count() << endl; //3
    ptr3 = nullptr;
    cout << "ptr1 use count: " << ptr1.use_count() << endl;	//2
    cout << "ptr3 use count: " << ptr3.use_count() << endl;	//0

    *ptr1 = 24;
    cout << "*ptr1: " << *ptr1 << endl;	//24
    cout << "*ptr2: " << *ptr2 << endl;	//24

    return 0;
}
```



函数调用时的注意事项：

*   传递 `shared_ptr` 作为参数
    *   当将 `shared_ptr` 传递给函数作为参数时，通常应该以引用的方式传递，而不是值传递
    *   这是因为值传递会增加引用计数，而函数结束时又会减少引用计数，这可能会导致不必要的内存分配和释放

*   返回 `shared_ptr`
    *   当从函数返回 `shared_ptr` 时，最好使用 `move()` 将智能指针转移给调用者，而不是直接返回
    *   这可以避免不必要的引用计数增加和减少操作

```c++
//传递 shared_ptr 作为参数
void processData(shared_ptr<Data>& ptr)
{
   /* code */
}

int main()
{
   shared_ptr<int> sharedPtr = make_shared<int>(42);
   processData(sharedPtr); // 以引用方式传递
}

/*-------------------------------------------------------------------------------------------------------------------------*/

//返回 shared_ptr
shared_ptr<int> createSharedInt(int value)
{
    shared_ptr<int> ptr = make_shared<int>(value);
    return move(ptr);
}

int main()
{
    shared_ptr<int> sharedPtr = createSharedInt(42);	//让 sharedPtr 指针指向返回的地址
    cout << "*sharedPtr = " << *sharedPtr << endl;
    return 0;
}
```



智能指针之间的转换：

*   `unique_ptr` 转换为 `shared_ptr`

```cpp
unique_ptr<int> up(new int(42));
shared_ptr<int> sp(move(up)); //up 现在为空

//这种转换会导致 unique_ptr 放弃对资源的所有权,转移给 shared_ptr
//unique_ptr 在转换后将不再拥有任何资源
```



*   `shared_ptr` 转换为 `unique_ptr`

```c++
shared_ptr<int> sp(new int(42));
unique_ptr<int> up(sp.get()); //sp 仍然拥有资源

//这种转换只是创建了一个新的 unique_ptr 指向相同的资源
//而 shared_ptr 仍然拥有该资源,shared_ptr 的引用计数不会改变

//如果 shared_ptr 在转换后被销毁,而 unique_ptr 仍然存在,则会导致双重释放问题,因此需要谨慎使用
//双重释放是指 shared_ptr 和 unique_ptr 在结束调用时都会对该地址释放空间,让该空间释放两次
```



####　3.1.3 弱引用指针 weak_ptr

指针特点：

* `weak_ptr` 并不拥有内存的所有权，并不能调用 `->` 和 解引用 `*`

* `weak_ptr` 通常用于打破 `shared_ptr` 之间的循环引用

    循环引用会导致内存泄漏，因为即使没有外部引用，对象也无法被正确销毁

* `weak_ptr` 提供了 `lock()` 函数，可以创建一个 `shared_ptr` 来检查其指向的对象是否仍然存在

    如果对象已经被销毁，`lock()` 函数会返回一个空的 `shared_ptr`

```cpp
int main()
{
	//创建 weak_ptr 和 使用 lock 函数
	shared_ptr<int> sp = make_shared<int>(42);
	weak_ptr<int> wp1(sp);		//从 shared_ptr 创建
	weak_ptr<int> wp2;			//默认构造
	weak_ptr<int> wp3(wp1);		//拷贝构造
	weak_ptr<int> wp4 = wp1;	//赋值构造
	//且不能通过 -> 和 * 访问内存上的数据,如 *wp1 ,是错误语法

	if (shared_ptr<int> lp = wp1.lock())	//使用 lock() 函数检查 weakPtr 是否指向一个有效的对象
		cout << "The value is: " << *lockedPtr << endl;
	else
		cout << "The object pointed to by weakPtr no longer exists." << endl;

	sp.reset();	// 释放 sp,这会导致所指向的对象被销毁

	if (shared_ptr<int> lp = wp1.lock())	//再次使用 lock() 函数检查 weakPtr
		cout << "The value is: " << *lockedPtr << endl;
	else
		cout << "The object pointed to by weakPtr no longer exists." << endl;
    
    return 0;
}
/*-------------------------------------------------------------------------------------------------------------------------*/

//打破 shared_ptr 之间的循环引用	没懂, 用的少所以不重要
class A;
class B;

class A
{
public:
    void setB(shared_ptr<B> b)
    {
        m_b = b;
    }

private:
    shared_ptr<B> m_b;
};

class B
{
public:
    void setA(shared_ptr<A> a)
    {
        m_a = a;
    }

private:
    weak_ptr<A> m_a;
};

int main()
{
    auto a = make_shared<A>();
    auto b = make_shared<B>();
    a->setB(b);
    b->setA(a);
    return 0;
}
```







###　3.2 分配器

分配器 ( Allocator ) 是 C++ 中用于管理动态内存的一种抽象机制

它负责分配、释放和管理程序所需的内存空间



分配器的主要作用包括：

*   内存分配：提供分配和释放内存的接口，通常包括 `allocate()` 和 `deallocate()` 等函数
*   内存管理：负责维护内存的状态，跟踪已分配和未分配的内存块
*   错误处理：处理内存分配失败的情况，比如抛出异常或返回错误码
*   定制化：允许用户根据需求定制内存分配的策略，如使用内存池、对齐内存等



分配器通常通过以下方式被使用：

*   作为 C++ 标准库容器的模板参数，如 `vector<T, Allocator>` 
*   作为自定义数据结构的组件，由用户自行实现
*   作为函数参数，传递给需要动态分配内存的函数



标准分配器：`allocator` 是标准分配器，提供了基本的内存分配和释放功能

```cpp
int main()
{
  allocator<int> alloc;
  int* p = alloc.allocate(1);	// 分配内存
  alloc.construct(p, 42);		// 构造对象

  cout << *p << endl;

  alloc.destroy(p);				// 销毁对象
  alloc.deallocate(p, 1);		// 释放内存

  return 0;
}
```
