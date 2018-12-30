---
title: Some basic details in Java
date: 2010-06-06 19:27:11
updated: 2010-06-06 19:27:11
categories: 知识积累
tags:
  - Java
---

乘着微风徐来总结思考最近做的题目中关于java的的一些细节,也是笔试中java基础考核点

<!-- more -->

# java访问权限

Java 类访问权限修饰符表

| 修饰符    | 同一个类(Self) | 同一个包(packages) | 不同包的子类(subclass) | 不同包的非子类(project or world) |
| --------- | -------------- | ------------------ | ---------------------- | -------------------------------- |
| private   | yes            | no                 | no                     | no                               |
| default   | yes            | yes                | no                     | no                               |
| protected | yes            | yes                | yes                    | no                               |
| public    | yes            | yes                | yes                    | yes                              |

* default又称无修饰符访问权限 或 friendly
* 访问权限修饰符权限从高到低排列是public ,protected ,friendly, private。

类所具有访问修饰符表

| 类     | 访问修饰符 |                                                         |   |
| ------ | ---------- | ------------------------------------------------------- | - |
| 外部类 | 单独类     | public,default,abstract,final (final,abstract 不能并存) |   |
| 内部类 | 普通内部类 | 四种权限修饰符，final,abstract,static(即成为嵌套类)     |   |
| 内部类 | 匿名内部类 | 无修饰符                                                |   |
| 内部类 | 嵌套类     | 必含有static修饰符，可另有四种权限修饰符,final,abstract |   |
| 内部类 | 局部内部类 | final,abstract                                          |   |

内部类说明

* **普通内部类**: 对象内隐式保存了一个指向外围类对象的引用
* **匿名内部类**: 在方法或作用域内的内部类，申明时刻同时创建类实例，用在只需要一个临时的实例，常见线程创建，监听器创建。
* **嵌套类**: 直接申明普通内部类为static ，不需要内部类对象与外围类对象之间有关联。内部类不能访问外部类非static字段和方法。
* **局部内部类**: 在方法或作用域内的内部类，在块内声明，在块内可以通过其构造函数创建多个类实例。

参考

* 详细说明：[http://malipei.javaeye.com/blog/151135](http://malipei.javaeye.com/blog/151135)
* 表格：[http://www.cnitblog.com/dickznew/archive/2007/03/09/23779.html](http://www.cnitblog.com/dickznew/archive/2007/03/09/23779.html)

# java基本类型

Java八大基本类型表：另特殊void，参考java编程思想中）

| 类别     | 基本型别 | 封装装类  | 大小        | 最小值    | 最大值         |   |   |
| -------- | -------- | --------- | ----------- | --------- | -------------- | - | - |
| 整形     | char     | Character | 16-bit (2B) | Unicode 0 | Unicode 2^16-1 |   |   |
| 整形     | byte     | Byte      | 8-bit (1B)  | -128      | +127           |   |   |
| 整形     | short    | Short     | 16-bit (2B) | -2^15     | +2^15-1        |   |   |
| 整形     | int      | Integer   | 32-bit (4B) | -2^31     | +2^31-1        |   |   |
| 整形     | long     | Long      | 64-bit (8B) | -2^63     | +2^63-1        |   |   |
| 浮点型   | float    | Float     | 32-bit (4B) | IEEE754   | IEEE754        |   |   |
| 浮点型   | double   | Double    | 64-bit (8B) | IEEE754   | IEEE754        |   |   |
| 布尔类型 | boolean  | Boolean   | —           | —         | —              |   |   |
| 特殊     | void     | —         | —           | —         | —              |   |   |

补充

* 另两个高精度计算类：BigInteger与BigDecimal，支持任意精度的整数和定点数
* java基本数据类型是存储在堆栈中，而大多数对象的实例中内容是分配存储在堆中。
* java中没有sizeof()方法，基本类型长度都一定，并封装在封装类中，如java.lang.Integer.SIZE，java.lang.Integer.MAX_VALUE

# 万类超类-Object

概述

* Object类是所有类的基类。在不明确给出超类的情况下，java会自动把object作为要定义类的超类。
* 可以使用类型为object的变量指向任意类型的对象。Object obj=new String("abc");当然，Object类型的变量只能用作各种值得通用持有者，要对他们进行任何专门的操作，都需要知道他们的原始类型并进行类型转换。String e=(String)obj;

Object的常用方法

| 方法              | 定义作用                                                                                                                                                  |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| boolean equals()  | 比较对象是否相等，Object内部通过==判断实现，如果两个对象指向同一内存区域，则返回真，否则返回false。经常会被子类覆盖，自定义成判断两对象内容是否对应相等。 |
| String toString() | 返回表示当前对象值的字符串                                                                                                                                |
| Class  getClass() | 返回类定义的类对象。该对象含有关于当前对象的信息                                                                                                          |
| Object clone()    | 创建对象的副本，java为新实例分配内存，并且将当前类所占内存中的内容拷贝到新实例的内存中                                                                    |
| int hashCode()    | 返回对象的hash码，Object中是native实现，方便 java.util.Hashtable中使用                                                                                    |

equals()和 hashCode()用法

Java API中定义equal()覆盖规则

* 对称性：如果x.equals(y)返回是“true”，那么y.equals(x)也应该返回是“true”。
* 反射性：x.equals(x)必须返回是“true”。
* 类推性：如果x.equals(y)返回是“true”，而且y.equals(z)返回是“true”，那么z.equals(x)也应该返回是“true”。
* 一致性：如果x.equals(y)返回是“true”，只要x和y内容一直不变，不重复x.equals(y)多少次，返回都是“true”。
* 其他：x.equals(null)，永远返回是“false”；x.equals(和x不同类型的对象)永远返回是“false”。

Java API中定义hashCode()覆盖规范

* 如果两个对象相同(**运用equals方法比较**)，那么它们的hashCode值一定要相同(如：new String("abc").hashCode()==new String("abc").hashCode()为真)；
* 如果两个对象的hashCode相同，它们并不一定相同 。

**组队使用场景**
在自定义类，并运用容器处理时，需要子重写hashCode与equals,在HashMap内部实现中，如 public V put(K key, V value) ;判断是否已存在键是通过 ( e.hash == hash  &&  ( (k = e.key) == key || key.equals(k) ) ) 判断的，也就可以看出是要同时判断hashCode与equals，此处有疑问，个人认为可以这样写 ((k = e.key) == key)||e.hash == hash && key.equals(k)),如果引用都一样，那么就直接可以判等了。

样例：适HashMap装填自定义类型

```Java
public final class MyData {
    final int a;
    final int b;
    public MyData(int a, int b) {
        this.a = a;
        this.b = b;
    }
    public boolean equals(Object obj) {
        if (obj == this) {
            return true;
        }
        if (!(obj instanceof MyData)) {
            return false;

        }
        if ((this.a == ((MyData) obj).a) && (this.b == ((MyData) obj).b)) {
            return true;
        }
        return false;
    }
    public int hashCode() {
        return a+b;
    }
}
```

参考

* Java面试基本类型：[http://hi.baidu.com/setcc/blog/item/f1346cfacbd481106d22ebf5.html](http://hi.baidu.com/setcc/blog/item/f1346cfacbd481106d22ebf5.html)
* Java基本类型：[http://tech.e800.com.cn/articles/2009/916/1253068489832_1.html](http://tech.e800.com.cn/articles/2009/916/1253068489832_1.html)
* equals和hashCode的API分析：[http://lionheart.javaeye.com/blog/135077](http://lionheart.javaeye.com/blog/135077)
* equals和hashCode使用说明：[http://blog.csdn.net/RichardSundusky/archive/2007/02/12/1508028.aspx](http://blog.csdn.net/RichardSundusky/archive/2007/02/12/1508028.aspx)

# java.lang.String类

**API解析**
String类是final类型，不可被继承, String类是的本质是字符数组char\[\], 并且其值不可改变。String内部字段private final char value\[\];

**String池**
Java运行时会维护一个String Pool（String池），JavaDoc翻译很模糊“字符串缓冲区”。String池用来存放运行时中产生的各种字符串，并且池中的字符串的内容不重复。而一般对象不存在这个缓冲池，并且创建的对象仅仅存在于方法的堆栈区，当使用任何方式来创建一个字符串对象s时，Java运行时（运行中JVM）会拿着这个X在String池中找是否存在内容相同的字符串对象，如果不存在，则在池中创建一个字符串s，否则，不在池中添加。

**String两种创建方式**:

* 使用new关键字创建字符串，比如String s1 = new String("abc");只要使用new关键字来创建对象，则一定会（在堆区或栈区）创建一个新的对象。
* 直接指定。比如String s2 = "abc";使用直接指定或者使用纯字符串串联来创建String对象，则仅仅会检查维护String池中的字符串，池中没有就在池中创建一个，有则指向池内对应的地址。

**intern()方法**
String的intern()方法就是扩充常量池的一个方法（字符串常量，可以通过String.intern()方法来实现共享一个单独的String实例的目的）；当一个String实例str调用intern()方法时，Java查找常量池中是否有相同Unicode的字符串常量，如果有，则返回其的引用，如果没有，则在常量池中增加一个Unicode等于str的字符串并返回它的引用。

**判断String相等**:

* ==判断，是判断两String引用是否指向同一存储地址
* equals();先判断是否同为String对象，然后判断对应值是否相等

**String运算效率**
String值操作 a+b; a.concat();new StringBuffer().append();在大量连续操作中，效率由低到高。

**考察题目** String s = new String("xyz");创建了两个String Object，一个在String池中，一个在堆中。

代码样例

```Java
String a = "abc";
String b = new String("abc");
String c = b.intern();
System.out.println(a == b); //false,地址比较
System.out.println(a.equals(b)); //true，值比较
System.out.println(a=="abcc".substring(0, 3)); //substring返回内部是用new String()实现！
System.out.println(a=="abc"+"");//+运算，并且两端都为直接量，都是在String池中存储
System.out.println(a==c); //intern();返回String池常驻引用
b=b+"c";
a=a+"c";
System.out.println(b=="abcc");  //false,当+两段有引用量，则内部new String()并栈独立存储
System.out.println(a=="abcc");  //false,同上
```

参考

* Sun邮件String讨论：[http://dlstone.bokee.com/185678.html](http://dlstone.bokee.com/185678.html)
* Java String分析：[http://laiseeme.javaeye.com/blog/102442](http://laiseeme.javaeye.com/blog/102442)
* java Native方法：[http://www.javaeye.com/topic/72543](http://www.javaeye.com/topic/72543)

** Java操作符instanceof

**概述：** instanceof运算符是用来在运行时指出对象是否是特定类的一个实例。instanceof 通过返回一个布尔值来指出，这个对象是否是这个特定类或者是它的子类的一个实例。 原理：利用**RTTI**:运行时类型识别（run-time type identification,RTTI）,可以实现多态，向上转型，类型检查。 不同于**反射**，在反射里：.class在编译时不可获得，是在运行时，打开检查.class文件，并构建类对象。

**用法：** boolean result = object instanceof class 参数：result：object 任意对象；class：任意已定义的对象类,即类名 说明：如果 object 是 class 的一个实例，则 instanceof 运算符返回 true。如果 object 不是指定类的一个实例，或者 object 是 null，则返回 false。

样列

```Java
 System.out.println(new Thread() instanceof Runnable); //true 接口适用，
 System.out.println("abc" instanceof String); //true本类适用，特许类枚举类也适用
 System.out.println("abc" instanceof Object); //true超类适用
/**
*以下为其他几种方式判断
*/
System.out.println("".getClass()==String.class);
System.out.println("".getClass().equals(String.class));
System.out.println(String.class.isInstance(""));
```

# Java超类子类关系

总结

* main入口类首先编译，如果所在类继承其他类，其他类先于main当前类编译。
* 超类先于子类编译，构造初始化，初始化子类是先初始化超类

**样列**:

```Java
 //基类Base
public class Base {
    public int local=0;

    public static void staticPrint() {
        System.out.println("base static print");
    }

    public  void print() {
        System.out.println("base non-static print");
    }

    public Base(){
        System.out.println("base  initial");
    }

    public void pLocal(){
        System.out.println(local);
    }

    static {
        System.out.println("base static");
    }
}

//子类Sub
public class Sub extends Base {
    public int local = 1;

    public Sub() {
        System.out.println("sub initial");
    }

    public static void staticPrint() {
        System.out.println("sub static print");
    }

    public void print() {
        System.out.println("sub non-static print");
    }

    public void pLocal() {
        System.out.println(local);
    }

    static {
        System.out.println("sub static");
    }
}

//测试类Test
public class Test {
    static {
        System.out.println("test static");
    }

    public static void main(String\[\] args) {
        System.out.println("0***********");
        Base s;
        System.out.println("1***********");
        s = new Sub();
        System.out.println("2***********");
        s.staticPrint(); // 直接调用Base的静态输出
        System.out.println("3***********");
        s.print(); // 最终对象sub中覆盖了此方法，执行子类中方法
        System.out.println("4***********");
        s.pLocal(); // 同3,执行sub类中的方法
        System.out.println("5***********");
        System.out.println(s.local); // 找字段，直接找当前应用类型字段，此处为Base基类对应字段，
        System.out.println("6***********");
        System.out.println(((Sub) s).local);// 强制恢复转型为Sub后，获取的是Sub的字段值
        System.out.println("7***********");
    }
}
```

输出结果

```console

test static
0***********
1***********
base static
sub static
base  initial
sub initial
2***********
base static print
3***********
sub non-static print
4***********
1
5***********
0
6***********
1
7***********
```

# Java修饰符final，static

## final

**final 数据**:

* 场景：1永不改变的编译时常量，2对象不可改变应用（对象内部可以改变）
* final数据：final是数据值不变，即是final又是static的字段只占据一段不能改变的存储空间

**final 参数**
Java允许在参数列表中以声明的方式将参数值为final，使得在方法的内部无法更改参数引用所指向的对象的引用，但可改变对象内部属性。

**final 方法**:

* 把方法锁定，在继承中不能被覆盖，保证方法行为不变；
* 方法指定为final，就是同意编译器将针对该方法的所有调用转为内嵌调用（类似编译时宏迭代而非运行时方法调用），可以消除方法调用压栈出栈等开销。（适用于方法体较小的方法）

**final类**
被声明为final的类不允许被继承，并且所有类方法都对应隐式声明为final

附注

* 类中的所有private方法都隐式指定为final，由于类中的private方法不能被覆盖，因此子类中同名方法即使创建新方法，不存在覆盖概念。
* 接口字段隐式自动声明为static和final，所有的方法自动是public，（显示声明public与否都不影响）

## static

用static修饰的代码块表示静态代码块，当Java虚拟机（JVM）加载类时，就会执行该代码块（用处非常大）。 **static:** static在Java语言中的使用有四种：(变量、方法、代码块、内部类)

**static变量** 按照是否静态的对类成员变量进行分类可分两种：

* 一种是被static修饰的变量，叫静态变量或类变量
* 另一种是没有被static修饰的变量，叫实例变量。

两者的区别是：

* 对于静态变量在内存中只有一个拷贝（节省内存），JVM只为静态分配一次内存，在加载类的过程中完成静态变量的内存分配，可用类名直接访问（方便），当然也可以通过对象来访问（但是这是不推荐的）。
* 对于实例变量，没创建一个实例，就会为实例变量分配一次内存，实例变量可以在内存中有多个拷贝，互不影响（灵活）。

**静态方法** 静态方法可以直接通过类名调用，任何的实例也都可以调用，因此静态方法中不能用this和super关键字，不能直接访问所属类的实例变量和实例方法(就是不带static的成员变量和成员成员方法)，只能访问所属类的静态成员变量和成员方法。 因为static方法独立于任何实例，因此static方法必须被实现，而不能是抽象的abstract。

**static代码块** static代码块也叫静态代码块，是在类中独立于类成员的static语句块，可以有多个，位置可以随便放，它不在任何的方法体内，JVM加载类时会执行这些静态的代码块，如果static代码块有多个，JVM将按照它们在类中出现的先后顺序依次执行它们，每个代码块只会被执行一次。

**静态内部类-即为嵌套类**:

* 为创建一个static内部类的对象，我们不需要一个外部类对象。  
* 不能从static内部类的一个对象中访问一个外部类对象。
  
参考：

* Java中static详解：[http://jeelee.javaeye.com/blog/528508](http://jeelee.javaeye.com/blog/528508)