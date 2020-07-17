---
title: C#学习一(入门)
date: 2020-07-15 16:05:54
tags: [C#]
---

# c#基础

## 嵌套类

相当于java中的内部类

```c#			
    class Program
    {

        public class InnerClass
        {
            public string CardId { get; set; }

            public void printMsg()
            {
                Console.WriteLine("卡号为:" + CardId);
            }
        }

        static void Main(string[] args)
        {
            TestInnerClass();

        }

        public static void TestInnerClass()
        {
            var innerClass = new Program.InnerClass();
            innerClass.CardId = "2432432432";
            innerClass.printMsg();
        }
    }

//运行结果
	//卡号为:2432432432
```

## patial关键字(部分类)

这里的Person类使用partial关键字修饰,部分类主要用于当一个类中的内容较多时,将一些内容拆分到不同的类中,但是这些类的名称要相同.

下面的Person类分为两个部分,一部分包含属性,一部分包含方法.

```C#
    class Program
    {

        public partial class Person
        {
            public int Id { get; set; }
            public string Name { get; set; }
        }

        public partial class Person
        {
            public void ShowMsg()
            {
                Console.WriteLine("姓名:" + Name);
                Console.WriteLine("编号:" + Id);
            }
        }


        static void Main(string[] args)
        {
            TestPartial();


        }

        public static void TestPartial()
        {
            var person = new Person();
            person.Name = "张三";
            person.Id = 1;
            person.ShowMsg();
        }
}
```

从该实例可以看出,在不同的部分类中可以直接互相访问其成员,相当于所有的代码都写到一个类中.



## string字符串类

包含一些c#常用字符串方法

```C#
static void Main(string[] args)
        {
            TestString01();
        }

        public static void TestString01()
        {
            string str = "hello C#";
            Console.WriteLine("字符串长度为:" + str.Length);
            Console.WriteLine("字符串中第一个字符为:" + str[0]);
            Console.WriteLine("字符串中最后一个字符为:" + str[str.Length - 1]);
        }

//运行结果
    //字符串长度为:8
    //字符串中第一个字符为:h
    //字符串中最后一个字符为:#
```



### indexOf和lastIndexOf

```C#
     static void Main(string[] args)
        {
            IndexOfTest();
        }


        public static void IndexOfTest()
        {
            string str = "fishmuffin@aliyun.com";
            if (str.IndexOf("@") != -1)
            {
                Console.WriteLine("字符串中含有@,其出现的位置是{0}",str.IndexOf("@") + 1);
            }
            else
            {
                Console.WriteLine("字符串中不含有@");
            }

        }
//运行结果
	//字符串中含有@,其出现的位置是11
```

### replace字符串替换

```c#
static void Main(string[] args)
        {
            ReplaceTest();
        }


        public static void ReplaceTest()
        {
            string str = "fish_muffin_apple_pie";
            if (str.IndexOf("_") != -1)
            {
                str = str.Replace("_", "#");
            }
            Console.WriteLine("替换后的字符串:" + str);
        }

//运行结果
	//替换后的字符串:fish#muffin#apple#pie
```



### Substring字符串截取

基本上和java类似

```C#
static void Main(string[] args)
        {
            SubstringTest();
        }
public static void SubstringTest()
        {
            string str = "fish_muffin";
            var substring = str.Substring(0,str.IndexOf("_"));
            Console.WriteLine(substring);
        }
//运行结果
	//fish
```



### Insert字符串插入

```C#
static void Main(string[] args)
        {
            InsertTest();
        }
public static void InsertTest()
        {
            string str = "fish_muffin";
            str = str.Insert(str.IndexOf("_"), "###");
            Console.WriteLine(str);
        }

//运行结果
	//fish###_muffin

```



### Split将字符串拆分成数组

```C#
static void Main(string[] args)
        {
            SplitTest();
        }
    public static void SplitTest()
        {
            string str = "fish_muffin_apple_pie";
            var strings = str.Split("_",StringSplitOptions.None);
            foreach (var s in strings)
            {
                Console.WriteLine(s);
            }
        }
//运行结果
    //fish
    //muffin
    //apple
    //pie
```



## 数组

```C#
static void Main(string[] args)
        {
            ArrayTest();
        }
public static void ArrayTest()
        {
            string[] strs = { "java", "c#", "golang" };
            for (var i = 0; i < strs.Length; i++)
            {
                Console.WriteLine(strs[i]);
            }
        }
//运行结果
    //java
    //c#
    //golang
```



## 枚举

```C#
static void Main(string[] args)
        {
            EnumTest();
        }
public static void EnumTest()
        {
            Console.WriteLine(LNGEnum.Name.Java+":"+(int)LNGEnum.Name.Java);
            Console.WriteLine(LNGEnum.Name.Python + ":" + (int)LNGEnum.Name.Python);
            Console.WriteLine(LNGEnum.Name.Golang + ":" + (int)LNGEnum.Name.Golang);
        }
        class LNGEnum
        {
            public enum Name:int
            {
                Java,
                Python,
                Golang

            }
        }

//运行结果
	//Java:0
	//Python:1
	//Golang:2
```



## Object类

Object类是C#语言中最原始,最重要的类,是所有类的父类.

### Equals

比较两个对象是否相等

```C#
Equals(object o1,object o2);	//静态方法
Equals(object o)	//非静态方法
```

```C#
static void Main(string[] args)
        {
            EqualsTest();
        }

class Car{};

        public static void EqualsTest()
        {
            Car c1=new Car();
            Car c2 = new Car();
            var flag1 = Equals(c2,c1);
            var flag2 = c1.Equals(c2);
            Console.WriteLine("两个对象是否相等:"+flag1);
        }
//运行结果
	//两个对象是否相等:False
```

### GetType

获取对象type类型

```C#
static void Main(string[] args)
        {
            GetTypeTest();
        } 
public static void GetTypeTest()
        {
            int i = 1;
            string str = "123";
            var time = new DateTime();
            Console.WriteLine(i.GetTypeCode());
            Console.WriteLine(str.GetTypeCode());
            Console.WriteLine(time.GetTypeCode());
        }
//运行结果
    //Int32
    //String
    //DateTime

```

### ToString

返回一个对象实例的字符串.

```C#
static void Main(string[] args)
        {
            ToStringTest();
        } 
class Car
        {
            public override string ToString()
            {
                return "This is a Car Class";
            }
        };
        public static void ToStringTest()
        {
            var car = new Car();
            Console.WriteLine(car.ToString());
        }

//运行结果
	//This is a Car Class
```



## Visual关键字

visual是虚拟的含义,在C#语言中,默认情况下类中的成员都是非虚拟的,通常将类中的成员定义成虚拟的,表示这些成员将会在继承后重写其中的内容.

visual关键字能修饰方法,属性,索引器,以及事件等,用到父类的成员中.

需要注意的是,visual关键字不能修饰使用static修饰的成员.



### 方法隐藏和重写的区别

```C#
        static void Main(string[] args)
        {
            VisualTest();
        } 
		public static void VisualTest()
        {
            A a1=new B();
            a1.Print();
            A a2=new C();
            a2.Print();
        }

        class A
        {
            public virtual void Print()
            {
                Console.WriteLine("A");
            }
        }

        class B:A
        {
            public new void Print()
            {
                Console.WriteLine("B");
            }
        }

        class C:A
        {
            public override void Print()
            {
                Console.WriteLine("C");
            }
        }
//运行结果
	//A
	//C
```

```tex
从上面的执行效果可以看出,使用方法隐藏方式的结果是父类A中Print方法中的内容,而使用方法重写方式的结果是子类C中Print方法的内容.
方法隐藏相当于在子类中定义新方法,而方法重写则是重新定义父类中方法的内容.
```



## Abstract关键字

定义抽象类或者抽象方法

```C#
static void Main(string[] args)
        {
            AbstractTest();
        } 
public static void AbstractTest()
        {
            var student = new Student();
            student.Name = "张三";
            var teacher = new Teacher();
            teacher.Name = "张老师";
            student.sayHello();
            teacher.sayHello();
        }

abstract class Person1
        {
            public string Name { get; set; }
            public int Age { get; set; }
            public abstract void sayHello();
        }

        class Student:Person1
        {
            public override void sayHello()
            {
                Console.WriteLine("student "+Name+" say hello");
            }
        }

        class Teacher:Person1
        {
            public override void sayHello()
            {
                Console.WriteLine("teacher " + Name + " say hello");
            }
        }
//运行结果
    //student 张三 say hello
    //teacher 张老师 say hello

```

## sealed

sealed关键字的含义是密封的,使用该关键字能修饰类或者类中的方法,修饰的类被称为密封类,修饰的方法被称为密封方法.

但是密封方法必须出现在子类中,并且是子类重写的父类方法,即sealed关键字必须与override关键一起使用.

密封类不能被继承,密封方法不能被重写.

```C#
bstract class GraphAbstract
        {
            public abstract void Area();
        }

        class Rectangle:GraphAbstract
        {
            public double Width { get; set; }
            public double Length { get; set; }
            public sealed override void Area()
            {
                Console.WriteLine("矩形的面积是:"+Width*Length);
            }
        }

        sealed class Circle:GraphAbstract
        {
            public double r { get; set; }

            public override void Area()
            {
                Console.WriteLine("圆的面积是:"+r*3.14*3.14);    
            }
        }


```

上例中,Circle类不能被继承,Reactangle类中的Area方法不能被重写



## 集合

### ArrayList

```C#
static void Main(string[] args)
        {
            ArrayListTest();
        }   
public static void ArrayListTest()
        {
            var arrayList = new ArrayList(){"a1","a2","a3", "ssss" };
            foreach (var o in arrayList)
            {
                Console.WriteLine(o);
            }
        }
//运行结果
	//a1
    //a2
    //a3
    //ssss
```

### Queue

Queue队列是常见的数据结构之一,队列是一种先进先出的结构,即元素从队列尾部插入,从队列头部移除.



```C#
static void Main(string[] args)
        {
            QueueTest();
        }    
public static void QueueTest()
        {
            var queue = new Queue();
            queue.Enqueue("aaaa");
            queue.Enqueue("bbbb");
            queue.Enqueue("cccc");
            while (queue.Count != 0)
            {
                Console.WriteLine(queue.Dequeue());
            }

            queue.Enqueue("111");
            queue.Enqueue("222");
            queue.Enqueue("333");
            //使用ToArray可以将Queue转换为数组
            var objects = queue.ToArray();
            foreach (var o in objects)
            {
                Console.WriteLine(o);       
            }

            
            //使用GetEnumerator()遍历
            var enumerator = queue.GetEnumerator();
            while (enumerator.MoveNext())
            {
                Console.WriteLine(enumerator.Current);
            }
        }


//运行结果
	//aaaa
    //bbbb
    //cccc
    //111
    //222
    //333
    //111
    //222
    //333
```



### Stack

数据结构栈,先进后出

```C#
static void Main(string[] args)
        {
            StatckTest();
        }    
   public static void StatckTest()
        {
            var stack = new Stack();
            stack.Push("一号");
            stack.Push("二号");
            stack.Push("三号");
            while (stack.Count!=0)
            {
                Console.WriteLine(stack.Pop());
            }
        }
//运行结果
    //三号
    //二号
    //一号
```

### Hashtable

C#中Hashtable类实现了IDictionary接口,集合中的值都是以键值对的形式存取的.

```C#
static void Main(string[] args)
        {
            HashtableTest();
        }     
public static void HashtableTest()
        {
            var hashtable = new Hashtable();
            hashtable.Add(1,"java");
            hashtable.Add(2,"C#");
            hashtable.Add(3,"goLang");
            hashtable.Add(4,"python");
            foreach (DictionaryEntry o in hashtable)
            {
                Console.WriteLine("编号:{0},内容:{1}",o.Key,o.Value);
            }
            Console.WriteLine("编号3的内容是:"+hashtable[3]);
        }

//运行结果
    //编号:4,内容:python
    //编号:3,内容:goLang
    //编号:2,内容:C#
    //编号:1,内容:java
    //编号3的内容是:goLang
```



### SortedList

SortedLIst类实现了IDictionary接口,集合中的值是以键值对形式存取的.

```C#
static void Main(string[] args)
        {
            SortedListTest();
        }      
public static void SortedListTest()
        {
            var sortedList = new SortedList();
            sortedList.Add(1,"张三");
            sortedList.Add(2,"李四");
            sortedList.Add(3,"王五");
            sortedList.Add(4,"赵六");
            foreach (DictionaryEntry o in sortedList)
            {
                Console.WriteLine("编号:{0},名称:{1}",o.Key,o.Value);
            }
        }
//运行结果
    //编号:1,名称:张三
    //编号:2,名称:李四
    //编号:3,名称:王五
    //编号:4,名称:赵六

```

## 泛型

```C#
static void Main(string[] args)
        {
            FanTest();
        }       
public static void FanTest()
        {
            MyAdd<double>(2.33,4.3);
            MyAdd<int>(2,3);
        }

        public static void MyAdd<T>(T a, T b)
        {
            double res = double.Parse(a.ToString()) + double.Parse(b.ToString());
            Console.WriteLine(res);

        }
//运行结果
    //6.63
    //5
```







### Nullable

对于引用类型的变量来说,如果为对其赋值,在默认情况下是NUll值,对于值类型的变量,如果未赋值,整型变量的默认值为0.

但通过0判断该变量是否赋值是不太准确的,在C#语言中提供了一种泛型类型(System.Nullable<T>)来解决值类型的变量在未赋值的情况下允许为NULL的情况.

```C#
static void Main(string[] args)
        {
            NullalbleTest();
        }      
  public static void NullalbleTest()
        {
            int? a = null;
            double? d = 2.33;
            if (a.HasValue)
            {
                Console.WriteLine("a的值为{0}",a);
            }
            else
            {
                Console.WriteLine("i的值为空!");
            }

            if (d.HasValue)
            {
                Console.WriteLine("d的值为{0}",d);
            }
            else
            {
                Console.WriteLine("d的值为空!");
            }
        }
//运行结果
    //i的值为空!
    //d的值为2.33
```

## IComparable和IComparer

IComparer和IComparable接口比较集合中的对象值,主要用于对集合中元素排序.

```C#
static void Main(string[] args)
        {
            CompareTest();
        }      
public static void CompareTest()
        {
            var dog = new Dog(1,"wang");
            var dog1 = new Dog(2,"wangcai");
            var compareTo = dog.CompareTo(dog1);
            Console.WriteLine(compareTo);
        }


        class Dog:IComparable<Dog>
        {
            public Dog(int id, string name)
            {
                this.id = id;
                this.name = name;
            }
            public int id { get; set; }
            public string name { get; set; }


            public int CompareTo(Dog other)
            {
                if (this.id > other.id)
                {
                    return -1;
                }

                return 1;
            }

            public override string ToString()
            {
                return id+":"+name;
            }
        }

//运行结果
	//1
```

## 委托和事件



C#语言中的委托和事件是其一大特色,委托和事件在windows窗体应用程序,ASP.NET应用程序,WPF应用程序等应用中是最为普遍的应用.

### 委托(Delegate)

C#中的委托类似于C或C++中函数的指针.委托是存有对某个方法的引用的一种引用类型变量.引用可以在运行时改变.

委托从字面上理解就是一种代理.



#### 命名方法委托

命名方法委托是最常用的一种委托,其定义的语法形式如下

```C#
修饰符 delegate 返回值类型 委托名(参数列表);
例如:
public delegate void MyDelegate();
```

定义好委托之后,需要实例化委托才能使用.

实例化委托的语法形式如下:

```C#
委托名 委托对象名 =new 委托名(方法名)
```

委托中传递的方法名既可以是静态方法的名称,也可以是实例方法的名称.

在实例化委托后即可调用委托,语法如下

```C#
委托对象名(参数列表);
```



```C#
static void Main(string[] args)
        {
            DelegateTest();
        }   
        public static void DelegateTest()
        {
            BB.getDele();
            BB.getDele1();
        }

        class AA
        {
            public static void showMsg()
            {
                Console.WriteLine("静态方法委托");
            }

            public void showMsg1()
            {
                Console.WriteLine("实例方法委托");
            }
        }

        class BB
        {
            public delegate void Dele();

            public static void getDele()
            {
                var dele = new Dele(AA.showMsg);
                dele();
            }

            public static void getDele1()
            {
                var dele = new Dele(new AA().showMsg1);
                dele();
            }
        }

//运行结果
    //静态方法委托
    //实例方法委托
```

#### 多播委托

多播委托指在一个委托中注册多个方法,在注册方法时可以在委托中使用加号运算符或者减号运算符来实现添加或撤销方法.

委托相当于点餐平台,每一个类型的商品都可以理解为在委托上注册了一个方法.

```C#
static void Main(string[] args)
        {
            HallTest();
        }   
public static void HallTest()
        {
            Hall.dele1();
        }

        class Hall
        {
            public delegate void CookDelegate();

            public static void dele1()
            {
                //实例化委托
                var cookDelegate = new CookDelegate(Cook.CookCookies);
                //向委托中注册方法
                cookDelegate += Cook.CookBurger;
                cookDelegate += Cook.CookNoodle;
                //从委托中撤销方法
                cookDelegate -= Cook.CookCookies;
                //委托调用
                cookDelegate();
            }
        }

        class Cook
        {
            public static void CookCookies()
            {
                Console.WriteLine("烹饪饼干");
            }

            public static void CookBurger()
            {
                Console.WriteLine("烹饪汉堡");
            }

            public static void CookNoodle()
            {
                Console.WriteLine("煮面条");
            }
        }

//运行结果
    //烹饪汉堡
    //煮面条
```



### 事件(Event)

事件是一种引用委托,实际上也是一种特殊的委托.

通常,每一个事件的发生都会产生发送方和接收方,发送方是指引发事件的对象,接收方则是指获取,处理事件.事件要与委托一起使用.

事件的定义语法如下:

```C#
访问修饰符 event 委托名 事件名;
```

```C#
static void Main(string[] args)
        {
            EventTest();
        }       
public static void EventTest()
        {
            var cook02 = new Cook02();
            //实例化事件,使用委托指向处理方法
            cook02.cookd += new Cook02.CookDelegate(Cook02.CookCookies);
            cook02.cookd+=new Cook02.CookDelegate(Cook02.CookNoodle);
            cook02.cookd+=new Cook02.CookDelegate(Cook02.CookBurger);
    		//调用触发事件的方法
            cook02.InvokeEvent();
        }


        class Cook02
        {

            //定义委托
            public delegate void CookDelegate();
            //定义事件
            public event CookDelegate cookd;

            //定义委托中使用的方法
            public static void CookCookies()
            {
                Console.WriteLine("烹饪饼干");
            }

            public static void CookBurger()
            {
                Console.WriteLine("烹饪汉堡");
            }

            public static void CookNoodle()
            {
                Console.WriteLine("煮面条");
            }
            //创建触发事件的方法
            public void InvokeEvent()
            {
                //触发事件,必须和事件是同名方法
                cookd();
            }
        }
//运行结果
    //烹饪饼干
    //煮面条
    //烹饪汉堡

```



注意:在使用事件时如果事件的定义和调用不在同一个类中,实例化的事件只能出现在`+=`或`-=`操作符的左侧.



## 反射

反射是指程序可以访问,检测和修改它本身状态或行为的一种能力

程序集包含模块,而模块包含类型,类型又包含成员.反射则提供了封装程序集,模块和类型的对象.

可以使用反射动态地创建类型的实例,将类型绑定到现有对象,或从现有对象中获取类型.然后,可以调用类型的方法或访问其字段和属性.

### 反射的优缺点

#### 优点

:one:反射提高了程序的灵活性和扩展性

:two:降低耦合性,提高自适应能力.

:three:它允许程序创建和控制任何类的对象,无需提前硬编码目标类

#### 缺点

:one:会有性能问题.使用反射基本上是一种解释操作,用于字段和方法接入时要远远慢于直接代码.因此反射机制主要应用在对灵活性和扩展性要求很高的系统机构上.

:two:使用反射会模糊程序内部逻辑;反射的代码比直接代码更加复杂,维护成本更高.



```C#

namespace test02
{
   public class Person
    {
        public Person()
        {

        }
        public string Name { get; set; }
        public int Age { get; set; }
        public char Gender { get; set; }
        public string Address { get; set; }

        public void Eat()
        {
            Console.WriteLine("I like rice");
        }

        private string Game()
        {
            return "I like game";
        }
    }
}


```



```C#
namespace test02
{
    class Program
    {
        static void Main(string[] args)
        {
            //获取程序集信息
            var loadFrom = Assembly.LoadFrom("D:\\c#vs_workplace\\class01\\test02\\bin\\Debug\\netcoreapp3.1\\test02.dll");
            Console.WriteLine("程序集名称:"+loadFrom.FullName);
            Console.WriteLine("程序集位置:"+loadFrom.Location);
            Console.WriteLine("运行程序集需要的CLR版本:"+loadFrom.ImageRuntimeVersion);
            Console.WriteLine();
            //获取模块信息
            var modules = loadFrom.GetModules();
            foreach (var module in modules)
            {
                Console.WriteLine("模块名称:"+module.Name);
                Console.WriteLine("模块版本ID:"+module.ModuleVersionId);
            }
            Console.WriteLine();
            //获取类,通过模块和程序集都可以
            var types = loadFrom.GetTypes();
            foreach (var type in types)
            {
                Console.WriteLine("类型的名称:"+type.Name);
            }
            //获取主要类的成员信息
            var type1 = loadFrom.GetType("test02.Person");
            var memberInfos = type1.GetMembers();
            foreach (var memberInfo in memberInfos)
            {
                Console.WriteLine("成员名称:"+memberInfo.Name);
                Console.WriteLine("成员类别:"+memberInfo.MemberType);

            }
        }
    }
}
```



![image-20200717141024808](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20200717141024808.png)



## 特性(Attribute)

特性是用于在运行时传递程序中各种元素(比如类,方法,结构,枚举,组件等)的行为信息的声明性标签.一个声明性标签是通过放置在它所应用的元素前面的方括号`[]`来描述的

特性用于添加元数据,如编译器指令和注释,描述,方法,类等信息.

.net提供了两种类型的特性:预定义特性和自定义特性



## 进程和线程

### 进程

是指一个内存中运行的应用程序,它包含着一个运行程序所需要的资源,一个应用程序可以同时运行多个进程;进程也是程序的一次执行过程,是系统运行程序的基本单位;



```C#
static void Main(string[] args)
        {
            ProcessTest();
        }     
public static void ProcessTest()
        {
            var processes = Process.GetProcesses();
            foreach (var process in processes)
            {
                var processProcessName = process.ProcessName;
                if (processProcessName.Equals("System"))
                {
                    Console.WriteLine(processProcessName);
                }
                
            }
        }
//运行结果
	//System
```



### 线程

线程是进程中的基本执行单元,是操作系统分配CPU时间的基本单位,负责当前进程中程序的执行,一个进程中至少有一个线程,一个进程中有多个线程,这个程序可以称为多线程程序.



