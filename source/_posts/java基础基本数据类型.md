---
title: java基础基本数据类型
toc: true # 是否生成目录
indent: true # 是否首行缩进
archive: true # 是否显示在归档
cover: false # 是否显示封面
mathjax: false # 是否渲染公式
pin: false # 是否首页置顶
top_meta: false # 是否显示顶部信息
bottom_meta: false # 是否显示尾部信息
sidebar: [toc]
tag:
  - Java数据类型
categories: Java
keywords: java数据类型
date: { { date } }
description: java 基础数据类型
icons: [fas fa-fire red, fas fa-star green]
---

## 基本数据类型

- boolean 1

- char 16
- byte 8
- short 16
- int 32
- float 32
- long 32

### 缓存池

Integer 是 int 的包装类。拥有-128-127 的缓存池。除非 `new Integer（number）` 否则会使用缓存池里的对象。

### 缓存池大小

boolean true &false

all byte

short value -128 - 127

int -128 - 127

char \u0000 - \uoo7F

## String

String 被声明为 final 不可变

好处：

- 缓存 hash 值，只需要计算一次
- String Pool 需要
- 安全性
- 线程安全

### String, StringBuffer and StringBuilder

**1. 可变性**

- String 不可变
- StringBuffer 和 StringBuilder 可变

**2. 线程安全**

- String 不可变，因此是线程安全的
- StringBuilder 不是线程安全的
- StringBuffer 是线程安全的，内部使用 synchronized 进行同步

### String.intern()

保证引用的是同一常量池中的对象

`s3=s1.intern()` 首先把 s1 的对象放入 String Pool （字符串常量池）中，再返回引用

## java 参数传递

java 是值传递而不是引用传递

java 在传递基本数据的参数时会对实参创建一个副本。将副本传递给形参。因此对副本的改变不会影响到原实参。

在传递引用类型是会传递实参的一个副本给形参。（两个引用指向的都是同一对象）。此时改变对象的内容会体现在原引用上。而对形参更改引用只会让形参指向新的对象，而不会更改原实参指向的对象。

### 类型转换

java 不允许隐式向下转型，比如：

~~float f=1.1~~ 这样是将 double 类型转换为 float

​ 必须使用 `float f=1.1f`

### 隐式类型转换

使用+=运算符可以执行隐式类型转换。使用如下：

```java
short s1=1；
s1+=1；
//相当于
s1=(short) (s1+1)
```

因为 1 是 int 类型，直接~~s1=s1+1~~ 不允许隐式向下转型。

## 继承

### 访问权限

- private 只能被该类的对象和方法访问，子类不允许访问
- protected 属性和方法只能被类本身的方法和子类访问，子类在不同的包也可以访问
- public 允许跨类访问和跨包访问
- default 不带修饰符的 包访问权限（默认访问权限）

### 抽象类和接口

抽象类中不一定有抽像方法，抽象方法一定在抽象类中

抽象类不能直接被实现，需要子类继承抽象类才能实例化子类

#### 接口

接口的成员（方法和字段）都必须是 public，~~private 和 protected~~不允许

接口的字段只能是 static 和 final

#### 区别

一个类可以实现多个接口，但是只能继承一个父类

接口的成员必须是 public 而抽象类则灵活

接口的字段都必须是 static 和 final 接口更灵活

一般接口优先于抽象类。

使用接口:

- 需要让不相关的类都实现一个方法，例如不相关的类都可以实现 Compareable 接口中的 compareTo() 方法；
- 需要使用多重继承。

使用抽象类:

- 需要在几个相关的类中共享代码。
- 需要能控制继承来的成员的访问权限，而不是都为 public。
- 需要继承非静态和非常量字段。

### super 关键字

使用`super()`可以访问父类的构造函数

使用如果子类重写了父类的方法，可以使用 super 引用父类中的实现

### 重写和重载

#### 重写(override)

子类的方法声明与父类完全相同

**里氏替换原则**: 子类重写的方法的访问权限必须大于等于父类的方法; 子类方法返回类型必须是父类返回的类型或是子类型

#### 重载(overload)

在同一类中,方法的名称相同,但是 参数类型和个数,和顺序至少有一个不同

仅返回值不同~~不算重载~~

## Object

```java
public final native Class<?> getClass()

public native int hashCode()

public boolean equals(Object obj)

protected native Object clone() throws CloneNotSupportedException

public String toString()

public final native void notify()

public final native void notifyAll()

public final native void wait(long timeout) throws InterruptedException

public final void wait(long timeout, int nanos) throws InterruptedException

public final void wait() throws InterruptedException

protected void finalize() throws Throwable {}

```

#### equals()实现

- 检查是否为同一个对象的引用,如果是直接返回 true
- 检查是否为同一个类型 如果不是直接返回 false
- 将 object 对象转型
- 检查每个关键域是否相等

#### hashcode()实现

使用素数 31 对类中的每个成员变量(x,y,z)作如下操作

```java
@Override
public int hashCode() {
    int result = 17;
    result = 31 * result + x;
    result = 31 * result + y;
    result = 31 * result + z;
    return result;
}
```

#### toString()

默认返回 Classname@hashcode 如`ToStringExample@4554617c`

@后面为散列码的无符号十六进制表示

#### clone()

`clone()`是 Object 的 protected 方法. 因此一个类不显式的实现 clone(),其他类就不能调用这个类的 clone()

重写 clone()的时候必须实现`cloneable`接口 否则会抛出`CloneNotSupportException`

##### 浅拷贝与深拷贝

浅拷贝拷贝出的引用与原引用指向同一个对象.(更改新引用指向的对象会同时改变原引用指向的对象)

深拷贝拷贝的引用指向的对象与原引用指向的对象不同

最好不要使用 clone(),可以使用拷贝构造函数和拷贝工厂来拷贝一个对象

## final 与 static

### final

对数据可以声明为常量,不可更改,可以使编译是常量,也可以是运行时常量

对于基本类型,final 使其不能改变.对于引用类型,final 使其不能改变引用,但是引用本身的对象可以修改

对于方法可以使其不能被子类重写

对于类,该类不能被继承

### static

- #### 静态变量 属于类的变量,由所有示例共享

- 静态方法 在类加载的时刻就已经存在.不依赖于任何示例,所以必须有实现,不能是抽象方法

静态内部类不依赖于任何示例就可以实现.普通内部类必须有外部类的实例

#### 初始化顺序

静态语句块和变量的初始化顺序取决于在代码中的位置

存在继承的情况下，初始化顺序为:

- 父类(静态变量、静态语句块)
- 子类(静态变量、静态语句块)
- 父类(实例变量、普通语句块)
- 父类(构造函数)
- 子类(实例变量、普通语句块)
- 子类(构造函数)
