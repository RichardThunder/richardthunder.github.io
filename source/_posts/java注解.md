---
title: java注解
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
  - Java注解
categories: Java
keywords: 注解
date: { { date } }
description: java 注解基础
icons: [fas fa-fire red, fas fa-star green]
---

# 注解

- java 自带注解 @Override @Deprecated @SupressWarning
- 元注解 定义注解的注解
- 自定义注解 由元注解自定义的注解

## 作用

- 生成文档 标识元数据生成 javadoc 文档
- 编译检查 标识元数据进行编译检查
- 编译时动态处理 动态生成代码
- 运行时动态处理 使用反射注入实例

## java 内置注解

- `@Override`：表示当前的方法定义将覆盖父类中的方法
- `@Deprecated`：表示代码被弃用，如果使用了被@Deprecated 注解的代码则编译器将发出警告
- `@SuppressWarnings`：表示关闭编译器警告信息

### `@Override`

```java
@Target(ElementType.METHOD)
@Rentention(RetentionPolicy.SOURCE)
public @interface Override{}
```

用于修饰方法 编译时有效(源文件保留,编译后字节码不存在)

### `@Deprecated`

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE})
public @interface Deprecated {
}
```

被文档化，能够保留到运行时，能够修饰构造方法、属性、局部变量、方法、包、参数、类型。这个注解的作用是告诉编译器被修饰的程序元素已被“废弃”，不再建议用户使用。

### `@SuppressWarnings`

```java
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
@Retention(RetentionPolicy.SOURCE)
public @interface SuppressWarnings {
    String[] value();
}

```

修饰的程序元素包括类型、属性、方法、参数、构造器、局部变量，只能存活在源码时，取值为 String[]。

告诉编译器忽略指定的警告信息

取值范围

| 参数                     | 作用                                                   | 原描述                                                                                   |
| ------------------------ | ------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| all                      | 抑制所有警告                                           | to suppress all warnings                                                                 |
| boxing                   | 抑制装箱、拆箱操作时候的警告                           | to suppress warnings relative to boxing/unboxing operations                              |
| cast                     | 抑制映射相关的警告                                     | to suppress warnings relative to cast operations                                         |
| dep-ann                  | 抑制启用注释的警告                                     | to suppress warnings relative to deprecated annotation                                   |
| deprecation              | 抑制过期方法警告                                       | to suppress warnings relative to deprecation                                             |
| fallthrough              | 抑制确在 switch 中缺失 breaks 的警告                   | to suppress warnings relative to missing breaks in switch statements                     |
| finally                  | 抑制 finally 模块没有返回的警告                        | to suppress warnings relative to finally block that don’t return                         |
| hiding                   | 抑制与隐藏变数的区域变数相关的警告                     | to suppress warnings relative to locals that hide variable（）                           |
| incomplete-switch        | 忽略没有完整的 switch 语句                             | to suppress warnings relative to missing entries in a switch statement (enum case)       |
| nls                      | 忽略非 nls 格式的字符                                  | to suppress warnings relative to non-nls string literals                                 |
| null                     | 忽略对 null 的操作                                     | to suppress warnings relative to null analysis                                           |
| rawtype                  | 使用 generics 时忽略没有指定相应的类型                 | to suppress warnings relative to un-specific types when using                            |
| restriction              | 抑制与使用不建议或禁止参照相关的警告                   | to suppress warnings relative to usage of discouraged or                                 |
| serial                   | 忽略在 serializable 类中没有声明 serialVersionUID 变量 | to suppress warnings relative to missing serialVersionUID field for a serializable class |
| static-access            | 抑制不正确的静态访问方式警告                           | to suppress warnings relative to incorrect static access                                 |
| synthetic-access         | 抑制子类没有按最优方法访问内部类的警告                 | to suppress warnings relative to unoptimized access from inner classes                   |
| unchecked                | 抑制没有进行类型检查操作的警告                         | to suppress warnings relative to unchecked operations                                    |
| unqualified-field-access | 抑制没有权限访问的域的警告                             | to suppress warnings relative to field access unqualified                                |
| unused                   | 抑制没被使用过的代码的警告                             | to suppress warnings relative to unused code                                             |

## 元注解

### @Target

描述注解作用的范围:用于描述修饰的注解可以用在什么类型上

```java
public enum ElementType {

    TYPE, // 类、接口、枚举类

    FIELD, // 成员变量（包括：枚举常量）

    METHOD, // 成员方法

    PARAMETER, // 方法参数

    CONSTRUCTOR, // 构造方法

    LOCAL_VARIABLE, // 局部变量

    ANNOTATION_TYPE, // 注解类

    PACKAGE, // 可用于修饰：包

    TYPE_PARAMETER, // 类型参数，JDK 1.8 新增

    TYPE_USE // 使用类型的任何地方，JDK 1.8 新增

}

```

### @Retention & @RetentionTarget

Reteniton 注解用来限定那些被它所注解的注解类在注解到其他类上以后，可被保留到何时，一共有三种策略，定义在 RetentionPolicy 枚举中。

```java
public enum RetentionPolicy {

    SOURCE,    // 源文件保留
    CLASS,       // 编译期保留，默认值
    RUNTIME   // 运行期保留，可通过反射去获取注解信息
}

```

### @Documented

描述在使用 javadoc 工具为类生成帮助文档时是否要保留其注解信息。

### @Inherited

被它修饰的 Annotation 将具有继承性。如果某个类使用了被@Inherited 修饰的 Annotation，则其子类将自动具有该注解。

## 注解与反射接口

反射包 java.lang.reflect 下的 AnnitatedElement 接口提供这些方法.这里注意,只有注解被被定义为 RUNTIME 时才能运行时可见

AnntatedElemented 是所有

程序元素(Class,Method,Constructor)的父接口.程序通过反射获取某个类的 AnnotatedElement 对象就可以通过反射调用该对象的 Annotation 信息

- `boolean isAnnotationPresent(Class<?extends Annotstion> annotationClass)`

​ 判断该程序元素上是否包含指定类型的注解 存在返回 True 否则返回 False，此方法会忽略注解对应的注解容器。

- `<T ectends Annotstion> T getAnnotation(Class<T> annotationClass)`

​ 返回该程序元素上存在的,制定类型的的注解,如果指定类型的注解不存在,返回 null

- `Annotation[] getAnnotions()`

​ 返回该程序上的所有注解,没有则返回长度为 0 的数组

- `<T extends Annotation> T[] getAnnotationsByType(Class<T> annotationClass)`

  ​ 返回该程序指定类型的注解数组,如果不存在,则返回长度为 0 的数组

  ​ `getAnnotationsByType`方法与 `getAnnotation`的区别在于，`getAnnotationsByType`会检测注解对应的重复注解容器。若程序元素为类，当前类上找不到注解，且该注解为可继承的，则会去父类上检测对应的注解。

  - `<T extends Annotation> T getDeclaredAnnotation(Class<T> annotationClass)`

    返回存此元素上的所有注解. 忽略继承的注解

- `<T extends Annotation> T[] getDeclaredAnnotationsByType(Class<T> annotationClass)`

​ 返回存此元素上的指定类型的注解. 忽略继承的注解

- `Annotation[] getDeclareAnnotations()`

  ​ 返回直接存在于此元素上的所有注解及注解对应的重复注解容器。与此接口中的其他方法不同，该方法将忽略继承的注解。如果没有注释直接存在于此元素上，则返回长度为零的一个数组。该方法的调用者可以随意修改返回的数组，而不会对其他调用者返回的数组产生任何影响。

# 注解不支持继承

可以使用@inherited 注解使子类具有该注解
