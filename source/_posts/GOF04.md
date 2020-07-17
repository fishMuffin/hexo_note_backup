---
title: 设计模式入门四(单例模式)
date: 2020-07-15 08:44:45
tags: [java,GoF]
---

## 单例模式的定义与特点

单例模式(singleton)的定义:指一个类只有一个实例,且该类能自行创建这个实例的一种模式.

在计算机系统中,Windows的回收站,操作系统中的文件系统,多线程中的线程池,显卡的驱动程序对象,打印机的后台处理服务,应用程序的日志对象,数据库的连接池,网站的计数器等.

单例模式有3个特点:

:one:单例类只有一个实例对象

:two:该单例对象必须由单例类自行创建

:three:单例类对外提供一个访问该单例的全局访问点

## 单例模式的实现

### 饿汉式单例

这个模式的特点是类一旦加载就创建一个单例,保证在调用getInstance方法之前单例就已经存在了.

```java
public class SingletonTest {
    private SingletonTest() {
    }

    private static SingletonTest instance = new SingletonTest();

    public static SingletonTest getInstance() {
        return instance;
    }
}

```

### 懒汉式单例

该模式的特点是类加载时没有生成单例,只有当第一次调用getInstance方法时才会去创建这个单例.

```java
public class SingletonTest {
    private static volatile SingletonTest instance = null;

    private SingletonTest() {
    }

    public static synchronized SingletonTest getInstance() {
        if (instance == null) {
            instance = new SingletonTest();
        }
        return instance;
    }
}
```

```tex
注意:如果是在多线程环境中的单例,不能删除上例代码中的关键字volatile和synchronized,否则会存在线程安全的问题.但是这两个关键字,使得每次访问都要同步,会影响性能,且消耗更多的资源.
```

## 单例模式的应用实例

```java
class Person {
    private static volatile Person instance = null;

    private Person() {
    }

    public static synchronized Person getInstance() {
        if (instance == null) {
            instance = new Person();
        }
        return instance;
    }

    @Override
    public String toString() {
        return super.toString();
    }
}

public class SingletonApp {
    public static void main(String[] args) {
        Person p1 = Person.getInstance();
        Person p2 = Person.getInstance();
        System.out.println("p1-->"+p1);
        System.out.println("p2-->"+p2);
        if(p1==p2){
            System.out.println("是同一个人!");
        }
    }
}

//运行结果:

    //p1-->GoF.Person@14ae5a5
    //p2-->GoF.Person@14ae5a5
    //是同一个人!

```

```tex
以上的例子可以发现,以上的p1和p2两个对象通过单例模式获取的,所以他们俩的地址值相等,是同一个对象
```

## 单例模式的应用场景

:one:某个类只要求生成一个对象的时候,如一个班的班长

:two:当对象需要被共享的场合.由于单例模式只允许创建一个对象,共享该对象可以节省内存,并加快对象访问速度.如web中的配置对象,数据库的连接池等.

:three:当某类需要频繁实例化,而创建的对象又频繁被销毁的时候,如多线程的线程池,网络连接池等.





















