我们只介绍C#中独有的，不同于C++的部分，因此这篇文章要求拥有C++编程的基础

# 对函数传入任意参数

可以通过一个方法传递给一个函数任意个参数，只需使用 params typename[] 关键字即可

```cs
int f(params int[] data) {

}
```

传入的参数会储存在data这一数组中

# 参数传递

我们在这里着重讨论类的参数传递，在C#中，类是一个引用类型，如果向一个具有类参数的函数传递参数，即使不像在C++中那样显式指出，传递的也是对象的引用而非拷贝构造而来的副本

```cpp
void f(A a) {
	a.data = 1;//会影响外部!
}
```

由于我们得到的是引用，我们可以在函数中修改外部的内容

如果我们在此基础上在加上引用关键字ref *(在C#中不使用&)*，我们就会得到引用的引用，因此我们不但可以改变外部变量指向的内存本身，还可以改变外部变量指向那一块内存

> *外部变量，也就是A类对象a，是一个引用对象，其本身就相当于一个指针，这与C++极为不同*

```cs
void f(ref A a) {
	a.data = 1;
	a = new A();
}
```

在这一例子中，函数中的第二条语句是会改变外部的类变量指向的内存本身的

> *C#拥有自动回收内存的特性，因此不必担心这样做会导致内存溢出*

C#中在传递参数时还可以加入out关键字，对于加了这一关键字的变量，其与ref相同，传递的是引用的引用，并且这一参数在传入前无需有初始值，并且甚至无需被事先声明 *(这被称作内联声明)*，并且会被视作一个空白的变量，也就是你无法使用在函数内调用传入时，这个参数在外部的值，并且这个变量在函数return前必须被赋值，否则会编译报错

> *注意在调用函数的时候，对于含out关键字的参数，要显式写出out关键字*

这一关键字的作用在于使得函数可以事实上有多个返回值，即使其本身只能return一个值，因为其要求必须给一个外部变量赋值

```cs
int f(int val, out int num) {
	num = val;
	...
}

int a = 1;
int b;
f(a, out int c);
f(a, out b);
Console.WriteLine(b);
Console.WriteLine(c);
```
# 类的初始化

在C#中，由于类是一个引用类型，其初始化需要使用new来分配内存，可以像C++一样调用带自定参数的构造函数，也可以使用对象初始化器

```cpp
A a = new A(arg1, arg2,...);
A a = new A{member1 = x, member2 = y, ...};
```

其中构造函数的用法我们不再介绍。在对象初始化器中，我们显式指出要给那个成员赋值，无需含有对象，而且其本质是语法糖，在编译时会变为

```cpp
A a = new A;
a.member1 = x;
a.member2= y;
```

这样的形式，因此对象初始化器只能作用于public成员

# 类的属性

“属性”是一种方法的封装，在编写类的时候，我们通常希望数据成员是私有的，又希望有方法来改变，得到这个数据成员的值，C#将其封装为属性，并且给出了 get，set方法，属性可以被作为变量使用，并且对其进行一些操作会自动调用这两个方法

```cs
class MyClass {
	private int data;
	public int Data {
		get {
			return Data;
		}
		set {
			data = value;
		}
	}
}

MyClass a = new MyClass;

int a = A.Data; //调用get
A.Data = 1; //调用set
```

其中，每一个属性对应一个数据成员，并且属性的名字应当为数据成员的大写，并且如果赋值可以简单地通过等于号进行，那么get，set方法可以简写，如下

```cs
class MyClass {
	private int data;
	public int Data {get; set;}
}
```

# 类的继承

与C++不同，C#中的类**不允许**多继承，然而多层继承 *(A->B->C->...)* 不受影响

在C#中，任何一个类都有一个最终的基类System.Object，任何被定义出来的类最终都是其子类，接口都继承自System.ValueType，而System.ValueType最终也是从System.Object继承而来

```C#
public class Base {
	...
}

public class Derived: Base {
	...
}
```

在C#中，不需要且不能指出使用哪种继承方式，并且只以public方式继承，并且，C#规定子类的可访问性不可以高于基类

在C#中也有virtual函数，如果希望一个函数是虚函数，需要显式给出virtual关键字，并且需要在子类中显式给出override关键字，并且virtual方法也具有动态特征

## 多态

先前我们提到过，类是**引用类型**，正因如此，对于有继承关系的类来说，多态性是很常见的

```C#
Base object = new Derived();
```

这样就会触发动态绑定了

## 抽象类，抽象方法 *(纯虚函数)*

要使一个类是抽象类，需要显式给出abstract关键字

只有抽象类可以拥有抽象函数，要使一个函数是抽象函数 *(纯虚函数)*，也需要显式给出abstract关键字，并且函数在拥有abstract关键字之后自动也是虚函数，因此不需要也不可以给出virtual关键字，否则会报错

抽象类的派生类**必须**实现每个抽象函数，否则会报错，并且也要加入override关键字

> *抽象类不能被实例化为对象*

```c#
public abstract class Shape {
	public abstract void A(); //不给出实现
	public void B() {
		...//抽象类中可以有一般的方法
	}
}

public class Circle: Shape {
	public override void A() {
		...
	}
}
```

## 密封类，密封方法

如果不希望一个类被继承，就应该设计成密封类；如果不希望一个方法被重写，就应该设计成密封方法

要实现这点，只需加入sealed关键字

```c#
public sealed class Circle: Shape {
	...
}
```

# 接口

之前提到过一个类只能有一个基类，但是在C#中，一个类可以继承很多个接口，接口使用interface关键字声明，接口是引用类型

接口存在的作用就是提供规范，其与抽象类有类似之处，其定义了一组方法、属性、事件或索引器

接口中的成员默认是public的

接口中有方法，这些方法**都**不能被实现，但是不用给出abstract关键字

接口本身不能被实例化

```c#
public interface IMovement {
	void Method1(string msg); //没有abstract关键字，也不给出实现
}
```

> *习惯上，接口的名字以大写I开头*

接口可以继承自另一个接口

```c#
public interface IJump {
	void Jump();
}

public interface IMovement: Ijump {
	void Move();
}
```

类要继承一个接口，需要实现这个接口及其所有父接口的方法

```c#
public class Animal: IMovement {
	void Move() {
		...
	}
	void Jump() {
		...
	}
}
```

## 显式的接口实现

有时候两个接口规定了同名的方法，那么同时继承这两个接口的类就需要在实现这些方法时带上接口名字和点运算符，这个类实例化的对象不能直接调用这个其中的任一方法，而需要先转换成对应的接口类型

```c#
public interface Iinter() {
	void Print();
}

public interface Iexter() {
	void Print();
}

public class MyClass: Iinter, Iexter {
	void Iinter.Print() {
		...
	}
	void Iexter.Print() {
		...
	}
}

MyClass myobject = new MyClass();
//不能myobject.Print()!
Iinter inter = myobject;
inter.Print(); //使用Iinter中的Print

Iexter exter = myobject;
exter.Print(); //使用Iexter中的Print
```

# internal访问控制

如果类、接口、方法、属性或字段是internal的，那么只有当前程序集 *(Assembly)* 当中的代码才能访问它

如果另一个项目通过using来引用当前项目，其将不能访问internal修饰的内容

> *一个程序集就是要求代码在同一个项目中，在.NET中，一个程序集对应一个编译后得到的dll文件*

# is和as运算符

is和as运算符适用于类型转换的，有时候强制的类型转换可能会不安全，譬如我们把一个Unity Object对象转换为Shape对象，但是这个对象可能本来不是Shape对象，就会出现InvalidCastException错误，这时候就有了这两个运算符

as会检查参数是否能转换为指定类型，如果可以，就返回转换之后的对象，如果不能，就返回一个null，用法就是

```c#
obj as typename;
```

例如

```c#
Shape a = s as Shape;
```

is会判断当前对象能否转换为指定类型，返回true或者false

```c#
if (a is Shape) {
	...
}
```

这一用法可能带来歧义，要牢记这里是判断能否**转换为指定对象**，而非是否**就是指定对象**

# Unity生命周期

![LifeSpan](https://u3d-connect-cdn-public-prd.cdn.unity.cn/h1/20220728/p/images/208453e6-cdf9-4963-b55a-47e3d19ed6f7___.png)

这就是Unity的生命周期，并且给出了什么类型的代码应该在什么时候运行的常见规范