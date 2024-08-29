---
title: java异常机制
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
  - Java异常
categories: Java
keywords: 异常
date: { { date } }
description: java 异常基础
icons: [fas fa-fire red, fas fa-star green]
---

# 异常机制

## 异常的层次结构

Java 异常都是对象,是 Throwable 的子类的实例

![java-basic-exception-1](/Users/richard/Downloads/java-basic-exception-1.png)

- 运行时异常 RuntimeException

  java 编译器不会检查它.即使没有 try-catch 捕获,或者 throws 子句声明抛出,依然编译通过

  - IndexOutOfBoundsException
  - IllegalArgument
  - NullPointerException
  - ClassCastException

- 编译时异常(非运行时异常)

  - 必须处理的异常,否则无法编译通过

  - IOException

  - SQLException

  - ClassNotFound

- 不可查异常(unchecked exceptions)

  运行时异常 runtimeException 和错误 error

- 可查异常(checked exceptions)

  除 runtimeException 和其他子类

## 异常关键字

try 将被监听的代码放入 try 语句块中.try 语句块中出现异常,异常就被抛出

catch 用于捕获 try 语句块中发生的异常

finally 无论是否发生异常,finally 中的语句都会被执行 主要用于回收 try 语句块中打开的资源

​ 只有 finally 执行完之后才会执行 catch 或 try 中的 return 或 throw 语句.

​ 如果 finally 中使用了 return 或 throws 语句,将会直接停止而不是执行 try 或 catch 中的语句.

throw 用于抛出异常

throws 用于方法签名中,声明该方法可能抛出的异常

## 异常的申明(throws)

如果方法中存在异常,不对其进行捕获,那么需要在方法头显式声明异常 使用 throws 接上声明的异常,多个异常使用" , "隔开.

```java
public static void method() throws IOException,FileNotFoundException{
//.....
}
```

如果父类方法没有声明该异常,那么子类继承父类后也不能声明异常

通常应该捕获知道如何处理的异常,对于不知道如何处理的异常,应该传递下去,在方法的签名处使用 throws 关键字声明可能抛出的异常

```java
private static void readFile(String filePath) throws IOException{
File file =new File(filePath);
String reult;
BufferedReader reader = new BufferedReader(new FileReader(file));
while((result =reader.readLine())! = null){
System.out.println(result);
}reader.close();
}
```

- 对于不可查异常,(error 和 runtimeException 和其子类)不可以使用 throws

- 必须声明该方法可以抛出的任何可查异常 checkException

- 调用方法必须遵循任何可查异常的处理和声明规则. 如果覆盖一个方法不能声明与覆盖方法不同的异常,声明的异常必须是被覆盖方法的同类或子类

## 异常的抛出(throw)

如果代码可能会引发某种错误,可以创建一个异常类实例并抛出他.

```java
public static double method(int value){
  if(value==0){
    throw new ArithmeticException("not 0");
  }
  return 5.0/value;
}
```

大多情况不需要手动抛出异常,java 大部分方法已经处理异常或声明异常,只需要捕获异常或者往上抛

## 自定义异常

定义一个异常类应包含两个构造函数,一个无参构造函数和一个带有详细描述信息的构造函数

```java
public class MyException extends Exception{
  public MyException(){}
  public MyException(String msg){
    super(msg);
  }
}
```

## 异常的捕获

### try-catch

可以捕获多个异常并作出处理

```java
ptivate static void readFile(String filePath){
try{}
catch (FileNotFoundException e){}
catch(IOException e){}
  //OR like this
  //捕获多个异常
  catch(FileNotFoundException | UnknowHostException e){}
  catch(IOException e){}
}
```

### Try-catch-finally

```java
try{}
catch(Exception e){}
finally{必定执行的代码}
```

- 当 try 没有捕获到异常时,try 语句块将被一一执行,catch 语句块将被跳过.finally 语句块继续执行
- try 捕获到异常但是 catch 没有相应的处理语句 异常抛给 JVM,finally 语句最后执行,**但是 finally 语句块后的语句不会执行**
- try 捕获到异常,catch 有相应的处理,会执行此处理.然后继续执行 finally 语句块.如果 try 或 catch 中有 return 语句,则会先执行 finally 语句在执行 return

### try-finally

可以直接使用 try-finally. try 捕获到异常,执行完 finally 语句不再继续执行.

可以用在不需要捕获异常的代码,用于关闭正在使用的资源 例如 IO 流执行完相关操作后关闭相应资源 数据库连接代码的关闭

```java
ReentrantLock lock = new ReentrantLock();
try{}
finally{
  lock.unlock();
}
```

在以下情况下 finally 不会执行

- 前面的代码中使用了 System.exit() 退出了程序

- finally 语句块中出现异常

- 程序所在的想线程死亡

- 关闭 CPU

### Try-with-resource

_java7 引入的 try-with-resource_

自动释放的资源需要是实现了 AutoCloseable 接口的类。

```java
//例如Scanner就默认实现了Closeable接口
private static void tryWithResource(){
	try(Scanner scanner = new Scanner(new FileInputStream("c:/abc"),"UTF-8")){}
	catch(IOException e){}
}
```

## 常用的异常

### RuntimeException

- `java.lang.ArrayIndexOutOfBoundsException` 数组索引越界异常。当对数组的索引值为负数或大于等于数组大小时抛出。
- `java.lang.ArithmeticException` 算术条件异常。譬如：整数除零等。
- `java.lang.NullPointerException` 空指针异常。当应用试图在要求使用对象的地方使用了 null 时，抛出该异常。譬如：调用 null 对象的实例方法、访问 null 对象的属性、计算 null 对象的长度、使用 throw 语句抛出 null 等等
- `java.lang.ClassNotFoundException` 找不到类异常。当应用试图根据字符串形式的类名构造类，而在遍历 CLASSPAH 之后找不到对应名称的 class 文件时，抛出该异常。
- `java.lang.NegativeArraySizeException` 数组长度为负异常
- `java.lang.ArrayStoreException` 数组中包含不兼容的值抛出的异常
- `java.lang.SecurityException` 安全性异常
- `java.lang.IllegalArgumentException` 非法参数异常

### IOException

- `IOException`：操作输入流和输出流时可能出现的异常。
- `EOFException` 文件已结束异常
- `FileNotFoundException` 文件未找到异常

### 其他

- `ClassCastException` 类型转换异常类

- `ArrayStoreException` 数组中包含不兼容的值抛出的异常
- `SQLException` 操作数据库异常类
- `NoSuchFieldException` 字段未找到异常
- `NoSuchMethodException` 方法未找到抛出的异常
- `NumberFormatException` 字符串转换为数字抛出的异常
- `StringIndexOutOfBoundsException` 字符串索引超出范围抛出的异常
- `IllegalAccessException` 不允许访问某类异常
- `InstantiationException` 当应用程序试图使用 Class 类中的 newInstance()方法创建一个类的实例，而指定的类对象无法被实例化时，抛出该异常

## 异常的使用

### 只针对不正常的情况使用异常

NullPointerException IndexOutOfBoundException 不应该使用异常捕获处理

### 在 finally 块中清理资源或使用 try-with-resource 语句

### 尽量使用标准异常

常用的标准异常

- IllegalArgumentException 参数的值不合适
- IllegalStateException 参数的状态不合适
- NullPointerException 在 null 被禁止的情况下参数值为 null
- IndexOutOfBoundsException 下标越界
- ConcurrentModificationException 在禁止并发修改的情况下，对象检测到并发修改
- UnsupportedOperationException 对象不支持客户请求的方法

### 对异常进行文档说明

### 优先捕获最具体的异常

第一个 catch 块处理所有 NumberFormatException 异常，第二个处理所有非 NumberFormatException 异常的 IllegalArgumentException 异常。

```java
public void catchMostSpecificExceptionFirst() {
    try {
        doSomething("A message");
    } catch (NumberFormatException e) {
        log.error(e);
    } catch (IllegalArgumentException e) {
        log.error(e)
    }
}
```

### 不要捕获 Throwable 类

Throwable 是所有异常和错误的超类.会同时捕获所有错误和异常

```java
public void doNotCatchThrowable() {
    try {
        // do something
    } catch (Throwable t) {
        // don't do this!
    }
}

```

### 不要忽略异常

捕获异常后不要无视异常,至少记录异常的信息

### 不要记录并抛出异常

```java
try {
    new Long("xyz");
} catch (NumberFormatException e) {
    log.error(e);
    throw e;
}

```

会给一个异常多个输出日志

### 包装异常时不要抛弃原始异常

### 不要在 finally 语句块中使用 return

在 finally 语句块中使用 return 会直接返回并会丢弃 try 中的 return

## JVM 处理异常的实现

### Exception table 异常表

异常表中包含了一个或多个异常处理者(exception Handler)的信息,其中包括:

- from 可能发生异常的起始点
- to 可能发生异常的结束点
- target 发生异常后异常处理者的位置
- type 异常处理者处理异常的类信息

### JVM 处理异常的机制

- 1.JVM 在当前出现异常的方法中查找异常表,是否有合适的处理者来处理
- 2.如果当前方法异常表不为空,并且符合处理者的 from 和 to 节点,并且 type 也匹配, JVM 调用位于 target 的调用者来处理
- 3.如果上一条未找到合理的处理者,则继续查找异常表中的剩余项目.
- 4.如果当前方法的异常表无法处理,则继续向上查找(弹栈处理)刚刚调用该方法的调用处,并重复上面操作
- 5.如果所有的栈帧被弹出仍然没有处理,则抛给当前的 Thread,Thread 会终止
- 6.如果当前 Thread 为最后一个非守护进程,且未处理异常,则会导致 JVM 终止运行

## 异常耗时

异常建立对象,抛出接住异常都非常耗时

建立一个异常对象是一个普通 Object 耗时的 20 倍

参考文章:

https://www.iteye.com/blog/icyfenix-857722
