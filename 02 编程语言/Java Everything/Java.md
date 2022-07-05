- [数据类型](#数据类型)
- [程序流程控制](#程序流程控制)
- [方法](#方法)
- [面向对象](#面向对象)
  - [封装](#封装)
  - [static](#static)
  - [继承extends](#继承extends)
  - [Package](#package)
  - [权限修饰符](#权限修饰符)
  - [final](#final)
  - [枚举enum](#枚举enum)
  - [抽象类abstract](#抽象类abstract)
  - [接口Interface](#接口interface)
  - [多态](#多态)
  - [内部类](#内部类)
- [常用api](#常用api)
  - [String](#string)
  - [Object](#object)
- [容器](#容器)
  - [ArrayList](#arraylist)
- [多线程并发](#多线程并发)
- [Java 8 新特性](#java-8-新特性)
  - [Lambda 表达式](#lambda-表达式)
  - [方法引用](#方法引用)
  - [Stream](#stream)
  - [Optional 类](#optional-类)

## 数据类型

**基本类型** ：

byte、int、short、long、double、float、boolean、char

​	每个基本类型都有对应的**包装类型**：

​				Byte、Integer、Short、Long、Double、Float、Character、Boolean;

​	使用包装类型合理的场景有： 

- 作为集合中的元素、键和值 

- 泛型，必须使用包装类型，如 List list ， OptionalInt rpcResult()

- 反射方法调用需使用包装类型，例如在Method.invoke,MethodHandle.invoke中 

- POJO类的字段、RPC方法的返回值和参数等可能要序列化的且可能缺失值的场景中


**引用类型**



## 程序流程控制



## 方法



## 面向对象

### 封装



### static

**static修饰成员变量**

成员变量： 

- 静态成员变量 **static修饰** 只有一份，**被所有对象共享**  **类.变量** 访问
- 实例成员变量  无static修饰，属于每个对象，必须用 **对象名.变量** 访问

内存原理：

​	 静态成员变量放在堆内存的类静态变量区，而实例成员变量放在new出来的对象的堆内存中，不同对象的实例成员变量不存储在同一片空间。

**static修饰成员方法**

成员方法：

- ​	静态成员方法 static修饰  归属于类，可以被共享访问 ，建议类名访问。（同一个类中访问静态方法可以省略类名）
- ​    实例成员方法 归属于对象，**只能**用对象触发访问  对象. 实例方法 

使用场景：

​	 表示对象自己的行为，且方法中需要访问成员变量，必须申请为实例方法；如果方法是执行一个共用功能为目的，可以申请成静态方法。



### 继承extends  

extends  继承后直接使用父类的属性和方法

```java
class 父类 {
	//子类们的相同特征（共性属性、共性方法）放在父类中定义
}
class 子类 extends 父类 {
	//子类独有的属性和方法定义在子类自己里面
    //子类不能继承父类的构造器
    //子类可以共享父类的静态成员
    //子类访问成员就近原则：先找子类，再找父类，都没有报错
}
```

- Java 只支持单继承，即一个子类只能继承一个父类 （为了防止歧义和混乱）。
- Object类是所有Java类的祖宗类。

**方法重写**：子类和父类中出现了一样的方法声明，称子类的方法是重写

- ​	使用场景： 子类需要父类的功能，但是父类这个功能不完全满足子类的需求
- ​	@Override 重写注释
- ​	重写的方法要和父类的方法的名称、形参列表一样
- ​	私有方法不能被重写



### Package

- 相同包的类可以直接访问，不同包的类要导包
- 类名冲突，可以选一个用全名访问



### 权限修饰符

- private - > 缺省  - > protected  - > public 
- 成员变量一般私有，方法一般公开



### final

绝育修饰符

- 修饰类，类是最终类，不能被继承
- 修饰方法，方法不能被重写
- 修饰变量，代表该变量被第一次赋值后不能再次被赋值



### 枚举enum

特殊类，用来表示一组常量

```java
enum Color 
{ 
    RED, GREEN, BLUE; 
} 
```

**values(), ordinal() 和 valueOf()** 方法位于 java.lang.Enum 类中：

- values() 返回枚举类中所有的值。
- ordinal()方法可以找到每个枚举常量的索引，就像数组索引一样。
- valueOf()方法返回指定字符串值的枚举常量。



### 抽象类abstract 

用abstract 修饰，是父类的一个不完全的设计图，父类知道子类一定会做的行为，但是每个子类的行为实现不同，父类把这种行为定义为抽象形式，具体实现交给子类自己完成。

```java
public abstract class Employee
{
   private String name;
   private String address;
   private int number;
   
   public abstract double computePay();

}
```

设计模式中的**模板方法模式**就是经典应用：

![模板模式的 UML 图](https://www.runoob.com/wp-content/uploads/2014/08/template_pattern_uml_diagram.jpg)



### 接口Interface

```java
//声明：
[可见度] interface 接口名称 [extends 其他的接口名] {
        // 声明变量
        // 抽象方法
}
//实现： 接口是用来被类实现（implements）的
修饰符 class 实现类名 implements 接口名称1，接口名称2，...{
    
}
```

本质是一种规范，约定必须要做某件事情

接口中的成员变量只能是 **public static final** 类型的

**Java8 新增的接口特性**

- 默认方法 ： 在方法名前加**default**关键字实现默认方法

- 静态默认：Java 8 的另一个特性是**接口可以声明（并且可以提供实现）静态方法**

```java
public interface Vehicle {
    //默认方法
    default void print(){
      System.out.println("我是一辆车!");
   }
    // 静态方法
   static void blowHorn(){
      System.out.println("按喇叭!!!");
   }
}
//接口的静态方法必须提部分本身的接口名来调用
```



### 多态

当继承、重写、父类引用指向子类对象：**Parent p = new Child();** 三个情况存在时会出现多态。

多态中成员访问特点：

- ​	方法调用：编译看左边，运行看右边；
- ​	变量调用：编译看左边，运行也看左边；



### 内部类

类中类，内部类寄生在宿主外部类

```java
public class 外部类名{
	//内部类
	public class 内部类名{
		//内部类可以访问外部类成员，包括私有成员
	}
} 
```

- 静态内部类

- 成员内部类

- 局部内部类

- **匿名内部类**

  不提供类名直接实例化，实现一个类中包含另一个类，使代码更加简洁。（类和类之间）

  ```java
  //定义一个匿名类
  object1 a = new object1(){
      public void work(){
          //
      }
  };
  a.work();
  ```



## 常用api

### String

创建对象的方式：

	1. string name = “ xxx”; （存在堆内存常量(公共)池，相同的只存在一个）  

      	2. new 构造器得到字符串对象 （存在堆内存上）

**String常用API**

- 判断字符串内容  
  - == 是判断字符串的地址的，字符串判断内容相等要使用 **equals** ：Str1.equals( Str2 )

###  Object 







## 容器

### ArrayList

集合，可以改变长度，元素可重复，带索引







## 多线程并发







## Java 8 新特性

###  Lambda 表达式

​	别名：闭包  Java 8 重要特性  作用：**把函数作为参数传递到方法中**  简化匿名类的代码写法

 语法格式：

```java
(parameters) -> expression
或
(parameters) ->{ statements; }
```

### 方法引用

用一对冒号  **::** 



### Stream



### Optional 类



