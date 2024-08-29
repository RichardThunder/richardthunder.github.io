---
title: java基础泛型
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
  - Java泛型
categories: Java
keywords: 泛型
date: { { date } }
description: java 泛型基础
icons: [fas fa-fire red, fas fa-star green]
---

# 泛型

- 泛型类
- 泛型接口
- 泛型方法

定义泛型方法时，必须在返回值前边加一个`<T>`，来声明这是一个泛型方法，持有一个泛型`T`，然后才可以用泛型 T 作为方法的返回值。

作用 减少强制类型转换来确保类型安全.

## 泛型的上下限

限制传入的参数为某种类型的子类或父类

上限

`class info<T extend number>{}` 此处的泛型只能是数字类型

下限

`class info<? super String>{}` 此处传入的参数只能是 String 或 Object 的泛型

PECS 原则

上下界通配符的使用应当遵循 PECS 原则：Producer Extends，Consumer Super。

限定通配符总是包括自己

上界类型通配符：add 方法受限

下界类型通配符：get 方法受限

如果你想从一个数据类型里获取数据，使用 ? extends 通配符

如果你想把对象写入一个数据结构里，使用 ? super 通配符

## 伪泛型

类型擦除 在编译阶段所有尖括号内的内容被替换为具体内容

### 类型擦除的原则

- 消除类型参数声明,删除<>包围的内容
- 根据类型参数上下界推断并替换所有类型参数为原生态类型:1.如果使用的是无限制通配符<?> 或者没有上下界限定,替换为 Object 2.如果存在上下界限定,根据子类替换原则去类型参数的最左边限定类型(父类)
- 为了保证安全,必要时插入强制类型转换
- 自动产生桥接方法,以保证擦除类型后的代码依然有泛型的多态性

### 泛型的编译期检查

泛型的类型检测是在编译前进行的.检查时只针对引用进行类型检测,而对实际引用的对象类型并不关心.例如:

```java
ArrayList<String> list1 = new ArrayList();
        list1.add("1"); //编译通过
        list1.add(1); //编译错误

ArrayList list2 = new ArrayList<String>();//泛型的编译检查不关心实际引用的对象.
        list2.add("1"); //编译通过
        list2.add(1); //编译通过
```

泛型中不存在继承关系,传递引用给不同的父类或子类(上转型和下转型)会导致编译错误如:

```java
ArrayList<String> list1 = new ArrayList<Object>(); //编译错误
ArrayList<Object> list2 = new ArrayList<String>(); //编译错误
```

## 泛型的桥接方法

类型擦除会造成多态冲突,JVM 使用桥接方法解决这个问题

继承泛型类的子类的方法中定义了泛型的具体类型.但是在父类中使用相同的具体类型,编译期的类型擦除会导致子类的方法与父类的参数类型不一致,进而导致重写方法变成了重载.

```java
class Pair<T> {
    private T value;
    public T getValue() {
        return value;
    }
    public void setValue(T value) {
        this.value = value;
    }
}

```

```java
class DateInter extends Pair<Date> {
    @Override
    public void setValue(Date value) {
        super.setValue(value);
    }
    @Override
    public Date getValue() {
        return super.getValue();
    }
}
```

<u>**此时子类中的方法不是重写而是重载**.</u>因为父类方法中的参数是 Object,子类是 Date(这也成为协变返回类型,子类重写方法的返回值不必与父类一直,可以是更狭窄的范围) 出现在语义上重写而在 jvm 看来是重写的问题(只有参数类型与返回类型一致时 jvm 才会看做重写)

因此 JVM 会使用桥接方法来处理这一问题,编译器会自动生成桥接方法来保证重写的语义.

在生成的桥接方法中会调用我们重写的方法.

## 泛型不允许实例化

由于类型擦除的存在编译器无法确定泛型参数类型.

### 泛型数组不能用具体类型进行实例化

Java 中是不能创建一个确切的泛型类型的数组的，除非是采用通配符的方式且要做显式类型转换才可以。

```java
List<String>[] list11 = new ArrayList<String>[10]; //编译错误，非法创建
List<String>[] list12 = new ArrayList<?>[10]; //编译错误，需要强转类型
List<String>[] list13 = (List<String>[]) new ArrayList<?>[10]; //OK，但是会有警告
List<?>[] list14 = new ArrayList<String>[10]; //编译错误，非法创建
List<?>[] list15 = new ArrayList<?>[10]; //OK
List<String>[] list6 = new ArrayList[10]; //OK，但是会有警告

```

## 异常中使用泛型

在异常中使用泛型不能通过编译.因为异常出现在运行期.在编译期由于类型擦除,所有异常中的泛型会被替换为 Object .所以不能在 catch 子句中使用泛型变量,但是可以使用类型变量.
