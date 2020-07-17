---
title: 设计模式入门五(工厂方法模式)
date: 2020-07-15 13:25:57
tags: [java,GoF]
---

# 工厂方法模式

## 工厂方法模式的定义

工厂(FactoryMethod)方法模式的定义:定义一个创建产品对象的工厂接口,将产品对象的实际创作工作推迟到具体子工厂类中.这满足创建型模式中要求的"创建与使用相分离"的特点.

我们把被创建的对象称为"产品",把创建产品的对象称为"工厂".如果创建的产品不多,只要一个工厂类就可以完成,这种模式叫"简单工厂模式",它不属于GoF23中经典设计模式,它的缺点是增加新产品时会违背"开闭原则".

## 工厂方法模式的优缺点

### 优点

:one:用户只需要知道具体工厂的名称就可得到所要的产品,无须知道产品的具体创建过程.

:two:在系统增加新的产品时只需要添加具体产品类和对应的具体工厂类,无须对原工厂进行任何修改,满足开闭原则.

### 缺点

:one:每增加一个产品就要增加一个具体产品类和一个对应的具体工厂类,这增加系统的复杂度



## 工厂方法模式的应用实例



```java
interface Phone {
    public void call();
}

class HuaWeiPhone implements Phone {
    @Override
    public void call() {
        System.out.println("华为手机call");
    }
}

class ApplePhone implements Phone {

    @Override
    public void call() {
        System.out.println("苹果手机call");
    }
}

interface PhoneFactory {
    public Phone createPhone();
}

class HuaWeiPhoneFactory implements PhoneFactory {

    @Override
    public Phone createPhone() {
        System.out.println("生产了一台华为手机");
        return new HuaWeiPhone();
    }
}

class ApplePhoneFactory implements PhoneFactory {

    @Override
    public Phone createPhone() {
        System.out.println("生产了一台苹果手机");
        return new ApplePhone();
    }
}


public class FactoryMethodTest {
    public static void main(String[] args) {
        PhoneFactory factory;
        factory=new ApplePhoneFactory();
        factory.createPhone();
        factory=new HuaWeiPhoneFactory();
        factory.createPhone();
    }
}

//运行结果:
    //生产了一台苹果手机
    //生产了一台华为手机

```

```tex
上述的代码符合开闭原则,无论要生产什么品牌的手机,只需要添加对应产品的工厂类即可.但是这还是有缺点的,如果产品过多,就会产生过多的工厂类.因此还有一种抽象工厂模式是工厂方法模式的升级版.
```



## 工厂方法模式和抽象工厂模式的区别

工厂方法模式中考虑的是一类产品的生产,如畜牧场只羊动物,电视机厂只生产电视机.

同种类称为同等级,也就是说:工厂方法模式只考虑生产同等级的产品,但是在实现生活中许多工厂是综合型的工厂,能生产多等级(种类)的产品,如农场里既养动物又种植物,电器厂既生产电视机又生产洗衣机或空调.

抽象工厂模式将考虑多等级产品的生产,将同一个具体工厂所生产的位于不同等级的一组产品称为一个产品族,图1所示的是苹果工厂和华为工厂所生产的手机与电脑的对应关系.

![image-20200715140738223](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20200715140738223.png)

# 抽象工厂模式



## 抽象工厂模式的定义

抽象工厂(AbstractFactory)模式的定义:是一种为访问类提供一个创建一组相关或依赖对象的接口,且访问类无须指定所要产品的具体类就能得到同族不同等级的产品的模式结构.

抽象工厂模式是工厂方法模式的升级版本,工厂方法模式只生产一个等级的产品,而抽象工厂模式可生产多个等级的产品.

使用抽象工厂模式一般要满足以下条件:

:one:系统中有多个产品族,每个具体工厂创建同一族但属于不同等级结构的产品

:two:系统一次只可能消费其中某一族产品,即同族的产品一起使用



## 抽象工厂模式的优缺点

抽象工厂模式除了具有工厂方法模式的优点外,其他主要优点如下:

:one:可以在类 的内部对产品族中相关联的多等级产品共同管理,而不必专门引入多个新的类来进行管理

:two:当增加一个新的产品族时不需要修改源代码,满足开闭原则

缺点是:

:one:当产品族中需要增加一个新的产品时,所有的工厂类都需要进行修改



```java
interface Phone {
    public void call();
}

class HuaWeiPhone implements Phone {
    @Override
    public void call() {
        System.out.println("华为手机call");
    }
}

class ApplePhone implements Phone {

    @Override
    public void call() {
        System.out.println("苹果手机call");
    }
}

interface Computer {
    public void calc();
}

class HuaWeiComputer implements Computer {

    @Override
    public void calc() {
        System.out.println("华为电脑计算");
    }
}

class AppleComputer implements Computer {

    @Override
    public void calc() {
        System.out.println("苹果电脑计算");
    }
}

interface factory {
    public Phone createPhone();

    public Computer createComputer();
}

class factoryHuaWei implements factory {

    @Override
    public Phone createPhone() {
        System.out.println("生产了一台华为手机");
        return new HuaWeiPhone();
    }

    @Override
    public Computer createComputer() {
        System.out.println("生产了一台华为电脑");
        return new HuaWeiComputer();
    }
}

class factoryApple implements factory {

    @Override
    public Phone createPhone() {
        System.out.println("生产了一台苹果手机");
        return new ApplePhone();
    }

    @Override
    public Computer createComputer() {
        System.out.println("生产了一台苹果电脑");
        return new AppleComputer();
    }
}

public class AbstractFactoryTest {
    public static void main(String[] args) {
        factory f;
        f = new factoryHuaWei();
        f.createPhone();
        f.createComputer();
        f = new factoryApple();
        f.createPhone();
        f.createComputer();

    }
}
//运行结果:
    //生产了一台华为手机
    //生产了一台华为电脑
    //生产了一台苹果手机
    //生产了一台苹果电脑
```





































