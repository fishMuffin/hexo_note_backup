---
title: java中volatile关键字的作用
date: 2020-07-15 10:21:37
tags: [java,volatile,多线程,并发编程]
---

## Volatile关键字的内存语义

`happens-before`对`volatile`的定义:volatile变量的写,先发生于后续对这个变量的读

## Volatile关键字的验证

Volatile关键字保证所有线程看到这个变量的值是一致的,如果没有该关键字修饰,可能会出现脏读的现象.例如下面的代码:

```java
class move {
    public boolean flag = false;

    public void stop() {
        flag = true;
        System.out.println("stop");
    }

    public void forward() {
        if (!flag) {
            System.out.println("forward");
        }
    }
}

public class VolatileTest {

    public static void main(String[] args) {
        move move = new move();

        new Thread(move::forward).start();
        new Thread(move::forward).start();
        new Thread(move::stop).start();
        new Thread(move::forward).start();
        new Thread(move::forward).start();
    }

}
```



![image-20200715102307074](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20200715102307074.png)

```tex
在上面的例子中可以看到,第三个线程中flag的值发生变化,但是其他的线程并没有立马感知到,所以控制台多打印了一行"forward",这就是"脏读"的现象.将flag变量使用volatile修饰,即可避免脏读.
```

![image-20200715105134770](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20200715105134770.png)