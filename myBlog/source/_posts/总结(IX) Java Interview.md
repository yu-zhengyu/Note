---
title: 总结(IX) Java Interview --- java面试二三事
date: 2016-01-06 09:22:30
tags:
- leetcode
- 刷题
categories:
- 总结
---

自己本来是个python的程序员,然而来了CMU后都被带成java程序员了,没办法了,现在面试也以Java为主,不过其实各个语言也是相通的.那么接下来我就收集一些JAVA面试时候会问的奇奇怪怪的问题吧.
<!--more-->
##[基础知识篇]

#####1. 什么是JVM
```
JVM 就是java虚拟机.JVM 会把Java code编译为byte code, 然后执行这些byte code.所有系统都能够安装JAVA虚拟机,这也体现了Java程序的多平台执行优势.
```

***

#####2. OOPS 的重要概念.
```
OOPS就是面向对象编程,主要包括以下4个概念:
1. Abstraction 抽象性
2. Polymorphism 多态
3. Inheritance 继承
4. Encapsulation 封装
```

***

#####3. Overloadding VS Overriding
```
* Overloadding 发生在compile阶段,Overriding 发生在runtime阶段.
* Overloading是在同一个类中写同一个函数,只是参数的类型不同.而overriding一般会在子类中出现,并且重写的时候函数名字和参数类型是一样的.
* 返回类型,overloading 是可以自由定义,但是Overriding就必须和继承的父类保持一致.
* 你可以overloading一个static 类, 但是不能overriding一个static类.
```

***

#####4. static VS instance
```
static method是一个类级别的方法,你可以直接调用它.但是instance方法是一个对象级别的方法,你必须首先创建一个引用指向它,然后再进行调用.
```

***

#####5. Stringbuilder, Stringbuffer, string 的区别
```
* String是immutable,不可改变的.Stringbuilder 和 Stringbuffer 都是mutable的.
* Stringbuffer 是 synchronized,Stringbuilder 不是
* Stringbuilder 比 Stringbuffer 快.
* StringBuffer线程安全,且保证同步.Stringbuilder不保证线程同步.
* String也是线程安全的,因为它不能够改变.
```

***

#####6. Java是值传递还是引用传递
```
当写函数的过程中,如果参数是基本类,如int或者double,那么我们会发现java是值传递.但是,在Java中对象作为参数传递时,是把对象在内存中的地址拷贝了一份传给了参数.所以,总体来说,都是pass by value.
```

***

#####7. Searialziation 和 Desearialziation.
```
Searialziation 是将对象转化为数据流,方便对象能够在网络中进行传递.Desearialziation当然就是讲流转为对象了.
```

***

#####8. final, finally, and finalize
```
* Final 能够修饰变量,方法和类.修饰变量,说明该变量不能再改变.修饰method,说明该方法不能被overridden.修饰class说明该类不能够被继承.

* finally是用在异常处理的情况.代码如果被写入了finally的块中,不管该段代码是否抛出异常,到最后finally块中的代码都必须执行.这个修饰一般用在关闭网络流,数据库或者输入输出流一类的处理.

* finalize是用在垃圾回收的过程.finalize会被垃圾回收调用,会在对象被抛弃之前清理所有的活动.java提供finalize()方法,垃圾回收器准备释放内存的时候,会先调用finalize().
```

***

#####9. java如何创建多线程
```
一般在java中,创建多线程有两种方法:
* 继承Thread类;
重写run() 函数,然后调用start()函数.

public class simpleThread extends Thread {
	public void run() {
		// 写上你的task
		System.out.println("Hello");
	}
}

public class simple {
	public static void main(String[] agrs) {
		simpleThread t = new simpleThread();
		t.start();
	}
}

* 实现Runnable接口.
方法和上面差不多,不过是实现接口,重写的函数还是run方法.

public class simpleThread implements Runnable {
	public void run() {
		// 写上你的task
		System.out.println("Hello");
	}
}

public class simple {
	public static void main(String[] agrs) {
		Thread t = new Thread( new simpleThread() );
		t.start();
	}
}


```

***

##### 10. java HashMap, 有什么特点

是基于Map接口的实现，存储键值对时，它可以接收null的键值，是非同步的，HashMap存储着Entry(hash, key, value, next)对象。

##### 11. HashMap 的 get 和 put 函数是如何工作的

当我们使用 put 函数的时候，我们会把 Key 和 value 传入，hashmap 会使用 hashcood 函数计算出 key 的哈希值，通过计算出的哈希值找到对应的 bucket，如果当前的 bucket 已经满了，HashMap会根据情况对容量进行扩容，一般是当前 capacity 的两倍。使用 get 函数的时候，传入 key 的值，计算出对应的哈希值，找到相应的 bucket，然后在 bucket 中使用 equals 函数找到相应的key。如果出现碰撞的情况，一般会使用链表来解决冲突。

##### 12. Hashcode 函数一般是怎么计算的

在 java1.8中，一般是将key 的高16位异或上低16位获得的。

##### 13. 为什么要创建私有的构造函数

私有构造函数和一般的私有函数没有区别，也是只能够类内部进行调用，外部无法进行访问。创建私有构造函数的原因可能有两个：一，是你不想让除类之外的其他类构造这个类。二是你只希望该对象在类的内部构造。

另外，私有构造函数一般用在singleton design pattern中。这也是开发人员使用私有构造函数的又一个原因。

###### 关于 singleton design

设计 singleton 的关键就是不能让其他类随便构造该类对象。那么，我们唯一能做的就是控制该类的构造函数。将构造函数变为私有是一个关键的步骤。 下面是一个 singleton 设计的例子。

####code[java]
```
// 以 singleton 的模式实现该类
public class Logging {
	// 创建一个实例，有且只有一个
	private static final Logging singletonInstance = new Logging();

	// 私有构造函数
	private Logging() {}

	// 返回实例，保证了有且只有一个实例
	public static Logging getSingleton() {
		return singletonInstance;
	}
}

```

###### 假设我们希望上面的 singleton 实例只有在被需要的时候再创建从而节省一些资源，我们应该怎么做。

这题还是承接上面的题目来说的。其实只要对 getSingleton 函数稍微修改一下即可

####code[java]
```

private static Logging singletonInstance = null;
public static Logging getSingleton() {
	if(singletonInstance == null)
		singletonInstance = new Loging();
	reutrn singletonInstance;
}

```

###### 在设计 singleton 的时候，如何保证线程安全？？
这个题也是承接上面的两个问题的。首先明白，为什么要保证线程安全。首先，上面的例子, 我们这样看视乎并没有什么问题。这是因为都是基于单线程来看的。假设我们现在有多个线程，并且这些线程同时调用上面的 getsingleton 函数来对实例进行初始化，因为同一时间满足了条件，结果我们也创建了两个或以上的实例。那么怎么解决这个问题。其实，java 也已经替我们想好了，只要在函数前面加上 synchronized 的关键字就可以了。

####code[java]
```

private static Logging singletonInstance = null;
public synchronized static Logging getSingleton() {
	if(singletonInstance == null)
		singletonInstance = new Loging();
	reutrn singletonInstance;
}

```

synchronized 会保证该函数只有一个线程能够进行访问，从而避免了上述冲突情况的出现。


##### 14. finally 中的代码会在 return 之后执行么？
这个题目也是有趣。一般来说，一个函数如果调用了 return，基本上说明该函数已经停止了，在这之后的代码也不会执行，编译器其实也会提示错误，然而下面的例子能够说明一些问题：

####code[java]
```

class SomeClass
{
    public static void main(String args[]) 
    { 
        // call the proveIt method and print the return value
    	System.out.println(SomeClass.proveIt()); 
    }

    public static int proveIt()
    {
    	try {  
            	return 1;  
    	}  
    	finally {  
    	    System.out.println("finally block is run 
            before method returns.");
    	}
    }
}

```

最后，该段代码的输出为

```
finally block is run before method returns.

```

我们发现了，return 后的代码不但执行了，还是在 return 之前。

‘’‘当然有例外，假如是调用 System.exit(), 那么final 块的代码就不会调用’‘’


##[code 篇]

### 用最少的code写一个函数,要求会把栈用光.
当时第一次遇到这种问题,我本身吓了一跳.不过其实这题是考你基础知识,就是栈和堆的问题.一般来收,所有的对象都是存放到堆中的,而指向对象的reference是存在栈里面的,函数调用,一开始也会放进栈.那么,用光栈最快的方法就是进行递归调用.下面上代码

####code[java]
```
public class Useupstack {
	public static void main(String[] args) {
		help();
	}
	
	public static help() {
		help();
	}
}
```

***

#### follow up: 如果是用完堆里面的函数呢？？
这个也是基础知识的考察,那么,因为java里面不方便对内存直接调用,所以这里我们用C++来进行解答.
我们知道,C++可以任意对内存进行控制,那么我们就可以用C++的malloc函数来进行内存操作.

####code[C++]
```

#include <stdlib.h>
#include <unistd.h>

void main(int argc, char** argv)
{
    while(1)
    {
        malloc(1);
    }
}
```

