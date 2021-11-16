# 关键字

**1 权限修饰符**
Public
Protect 同包下可以访问，其他包时，子类（非测试类）继承了另一个包的类（父类）就可
缺省（不写） 只能在本包下
Private 只能在本类
Private<缺省<protect<public
**2 static**
①静态变量在类加载的时候初始化，不需要创建对象，内存就开辟了
②静态变量存储在**方法区内存**中
③static可以定义静态代码块：
语法格式：static{}（在程序入口之前执行）
类加载时执行，并且只执行一次
④在一个类中可以执行多个，遵循自上而下的顺序
**3 this**
指向对象自身地址
**this不能使用在static中**
this可以调用实例方法，而不用创建对象。
**this()只能出现在构造函数的第一行（super()也是只能出现构造函数的第一行,两者不能共存）**
**4 super**
代表当前对象的父类型特征,super()通过子类的构造方法，调用父类的构造方法（调用时，已经创建了对象）初始化父类型特征。
**5 abstract**
用于定义抽象类
**6 volatile**
用于保证线程的可见性和有序性，具体解释见高并发章节。
**7 transient**
保证参数游离化，不参与序列化。
**8 import**
导入包
**9 native**
调用本地方法
**10 synchronized**
加锁，用于保证线程的原子性、有序性和可见性。
**11 final**
final类无法被继承
final方法无法被覆盖（重写）
final变量赋值无法被修改
final给实例变量要求必须手动赋值（加static）

# Object类

1、关于Objecte类中**toString方法**
public String toString(){
return
}
默认实现：类名对象的内存地址转换为十六进制的形式。
toStringd()的作用：通过调用这个方法可以将一个Java对象转换成字符串表示形式
建议所有子类重写toString()方法。向简洁、详实、易阅读的方向发展

2、 关于Object类中的**equals()方法**【因为类中的equals()方法比较的是地址，如果需要比较两个对象的具体情况需要重写equals()方法】
源代码：
public Boolean equals(Object obj){
return (this==obj)
}
设计目的：以后编程的过程，通过equals判断两个对象是否相等
==判断的是基本数据类型和地址，不能判断两个对象是否相等
String类已经重写了equals()方法，toString()方法

3、在object类中的**finalize()方法**
语法：protected void finalize() throw Throwable{}
finalize只有一个方法体，里面没有代码，这个方法是protected修饰的
不需要程序猿手动调用，JVM 的垃圾回收器回收的时候，当一个对象即将被垃圾回收器回收的时候，垃圾回收器（GC）负责调用。

4、**hashCode()方法**
Native：底层调用C++程序
hashcode()方法的返回的是哈希码，实际上是地址经过哈希算法得到的一个值。

5、Object类中**wait()**【使用时当前线程需要有锁】和**notify()**（生产者和消费者模式）:建立在线程同步基础之上，因为多线程操作一个数据，有线程安全问题。必须放在synchronized中。
o.wait()：让正在o对象上活动的线程（当前线程）进入等待状态，无期限等待，直到被唤醒为止。被唤醒后还需要拿到锁才能执行。**释放锁**。
o.notify()：唤醒,**不释放锁**。

# String类

**一、基本介绍**

**底层是final byte[](jdk 9之后) final char [] ** jdk1.8代码：

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];
}
```

String a=“abc”无法变，字符串是存在方法区的字符串常量池中，因为字符串在开发中使用太频繁。为了执行效率
String s= ”xy”;
String s2= ”xy”;
String a=new String(“xy”);
String b=new String(“xy”);
凡是双引号的都在字符串常量池中有一份

**二、方法**

**A.charAt(a)** 返回字符串A一个下标为a的字符。
A.compareTo（B） 比较A、B字符串的大小，从第一个字符串往后比较。
A.contains(B) 判断A是否包含B
A.endsWith(B) 判断当前字符串是否以某个字符串结尾
A.startsWih(B) 判断当前字符串是否以某个字符串开始
A.equalsIgnoreCase(B) 忽略大小写比较A、B字符串
A.getBytes() 将字符串转为byte数组
A.indexOf(B) 判断B某个字符串在当前字符串第一次出现的索引
A.lastIndexOf(B) 判断某个字符串在当前字符串最后一次出现的索引
A.isEmpty(B) 判断B字符串是否为空
**A.length() **计算A字符串的长度
A.repalce(old char【old string】,new char【new string】)替换所有old char，
A.split(“a”)以a为标志来对A进行拆分，返回一个String数组
**A．subString(a,b)从下标为a（包括）开始截字符串，到b（不包括）结束**
A．toCharArray() 将字符串转换成char[]
A．toLowerCase() 转换成小写
A．toUpperCase() 转成大写
A.trim()去除字符串前后空白
String中只有一个静态方法，不需要new对象，valueOf()将非字符串转换成字符串
valueOf(Object obj) 转换成String类型（会调用toString方法，故而根据需求重写toString方法）

# StringBuffer、StringBuilder

- StringBuffer

底层实际上一个byte[](char[])数组，往StringBuffer（与String不兼容不能往String中添加）中放数据，是转换成byte[](char[]).**初始量是**16，**扩容是两倍**+2。

主要方法：

```java
append();//添加元素
length();//长度
subString(start,end);//子符串
reverse();//反转
toString();//返回字符串
```

- StringBuilder

底层数组的初始量容量也是16，扩容2x+2；StringBuilder也能完成字符串拼接。

主要方法：

```java
append();//添加元素
length();//长度
subString(start,end);//子符串
reverse();//反转
toString();//返回字符串
```

- 区别

StringBuffer有**synchronized关键词**修饰，表示**在多线程环境下运行时安全**的；StringBuilder是多线程不安全。在多线程中局部变量适合用StringBuilder,成员变量适合用StringBuffer。

# 集合

**1、集合的概述**

- 数组也是一个集合，集合是一个容器，可以容纳其他类型的数据。
- 集合不能直接存储基本数据类型，不能直接存储java对象，集合中存储的是引用。
- 注意：集合在java中本身是一个容器，是一个对象。集合在任何时候存储的都是引用
- 在java中，每一个不同的集合，底层对应不同的数据结构。往不同的集合中存储元素，等于将数据放到了不同的数据结构中。

**2、集合的分类**

- 2.1集合继承结构：

![img](https://uploadfiles.nowcoder.com/images/20211009/628143978_1633784986108/013F6BC898F6FB7EEF059530386F323D)

new ArrayList();创建一个底层是数组的集合

new LinkedList();创建一个底层是双向链表的集合

new TreeSet() 创建一个底层是二叉树的集合

SortedSet是HashSet()无序(存取顺序不一定相同)不可重复（不会报错，只能有一个输出）的，但是存入的元素是可排序的。

- 2.2键值对方式

![img](https://uploadfiles.nowcoder.com/images/20211009/628143978_1633785546274/45604148B42DE05BF88DA2FD1584A788)

**3、具体集合介绍**

**1 Arraylist**

- 初始化容量**10**，底层是Object数组。当超过容量时，会进行数组扩容，为原来的**1.5**倍。

![img](https://uploadfiles.nowcoder.com/images/20211011/628143978_1633951957088/612FA650A68545BF664CC8D8AEBAA16C)

![img](https://uploadfiles.nowcoder.com/images/20211011/628143978_1633951930481/96CE18A8CA4FF05677B91494E45A27D9)

- 优化：预估初始化容量，给定一个初始化容量，减少数组扩容。（同StringBuffer）
- 用的最多的集合ArrayList,原因：向数组末位添加元素效率不受影响，我们在检索查找某个元素的操作比较多。

**2 LinkedList**

底层是**双向链表**

<img src="https://uploadfiles.nowcoder.com/images/20211011/628143978_1633952056592/C4BA5C562F8C88BDEC414D8ECD457382" alt="img" style="zoom:67%;" />

**3 Vector**

底层是Object数组,初始容量是**10**，扩容时是原来的**两倍**

![img](https://uploadfiles.nowcoder.com/images/20211011/628143978_1633952139358/507C42C70ED4AF81A38E96A8218C7ABB)

![img](https://uploadfiles.nowcoder.com/images/20211011/628143978_1633952242855/3B1F37723D6A5D92FC4459222387358D)

**4 HashMap**

- 底层实际上是一个数组+链表，数组中每一个元素是一个单向列表。（数组和链表的结合体）**key值可以为**null.
- 找到数组的下标是通过对key的地址进行hashcode()转为哈希，在对哈希值进行哈希算法得到数组下标。

<img src="https://uploadfiles.nowcoder.com/images/20211011/628143978_1633952425710/2CF168605D2FF540F594B91EEFBAC637" alt="img" style="zoom:80%;" />

- **为什么哈希表的增删和查询效率都很高**？

  增删是在链表上完成，查询也不需要都扫描，只需要部分扫描。Hashmap会先调用hashcode（确定数组下标），equals（确定元素是否相同）方法。

- Hashmap的key**无序的原因**：因为不一定挂到哪个单向列表上了

- **不可重复的原因**：equals方法来保证不可重复，重复的话会把value给覆盖。

- Hashmap集合的默认**初始化容量是16**，**默认加载因子是**0.75**，**两倍扩容。

  默认加载因子：当hashmap底层数组容量达到75%的时候，数组开始扩容。

  Hashmap初始化容量必须是2的倍数，这是因为达到散列均匀，为了提高hashmap的存取效率。


![img](https://uploadfiles.nowcoder.com/images/20211011/628143978_1633952758786/607F5F1BD01B3DAE948A0E6560A6C01E)

- 在jdk8之后，如果哈希表的单向列表节点超过8的时候，数据结构会变成红黑树结构。当红黑树节点小于6时会自动变成单向列表。二叉树检索也会减少。

**5 HashTable**

- HashTable和HashMap的实现原理几乎一样，差别无非是
- HashTable**不允许key和value为null**
- HashTable是线程安全的。但是HashTable线程安全的策略实现代价却太大了，简单粗暴，get/put所有相关操作都是synchronized的，这相当于给整个哈希表加了一把大锁。多线程访问时候，只要有一个线程访问或操作该对象，那其他线程只能阻塞，相当于将所有的操作串行化，在竞争激烈的并发场景中性能就会非常差。
- 底层也是哈希表数据结构，底层**初始化容量11**，**加载因子**0.75**.扩容是**二倍再加一。

# ConcurrentHashMap与HashMap

我们知道HashMap是线程不安全的，在多线程环境下，使用Hashmap进行put操作会引起死循环，导致CPU利用率接近100%，所以在并发情况下不能使用HashMap。

- **ConcurrentHashMap**

主要就是为了应对hashmap在并发环境下不安全而诞生的，ConcurrentHashMap的设计与实现非常精巧，大量的利用了volatile，final，CAS等lock-free技术来减少锁竞争对于性能的影响。

我们都知道Map一般都是数组+链表结构（JDK1.8该为数组+红黑树）。

ConcurrentHashMap避免了对全局加锁改成了局部加锁操作，这样就极大地提高了并发环境下的操作速度，由于ConcurrentHashMap在JDK1.7和1.8中的实现非常不同，接下来我们谈谈JDK在1.7和1.8中的区别。

- **JDK1.7版本的CurrentHashMap的实现原理**

在JDK1.7中ConcurrentHashMap采用了数组+Segment+分段锁的方式实现。

Segment(分段锁)-减少锁的粒度ConcurrentHashMap中的分段锁称为Segment，它即类似于HashMap的结构，即内部拥有一个Entry数组，数组中的每个元素又是一个链表,同时又是一个ReentrantLock（Segment继承了ReentrantLock）。

内部结构：ConcurrentHashMap使用分段锁技术，将数据分成一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据的时候，其他段的数据也能被其他线程访问，能够实现真正的并发访问。

从上面的结构我们可以了解到，ConcurrentHashMap定位一个元素的过程需要进行两次Hash操作。第一次Hash定位到Segment，第二次Hash定位到元素所在的链表的头部。

**该结构的优劣势**：坏处是这一种结构的带来的副作用是Hash的过程要比普通的HashMap要长。好处是写操作的时候可以只对元素所在的Segment进行加锁即可，不会影响到其他的Segment，这样，在最理想的情况下，ConcurrentHashMap可以最高同时支持Segment数量大小的写操作（刚好这些写操作都非常平均地分布在所有的Segment上）。所以，通过这一种结构，ConcurrentHashMap的并发能力可以大大的提高。

- **JDK1.8版本的CurrentHashMap的实现原理**

JDK8中ConcurrentHashMap参考了JDK8 HashMap的实现，采用了数组+链表+红黑树的实现方式来设计，内部大量采用CAS操作，这里我简要介绍下CAS。

**CAS**是compare and swap的缩写，即我们所说的比较交换。cas是一种基于锁的操作，而且是乐观锁。在java中锁分为乐观锁和悲观锁。悲观锁是将资源锁住，等一个之前获得锁的线程释放锁之后，下一个线程才可以访问。而乐观锁采取了一种宽泛的态度，通过某种方式不加锁来处理资源，比如通过给记录加version来获取数据，性能较悲观锁有很大的提高。CAS 操作包含三个操作数 —— 内存位置（V）、预期原值（A）和新值(B)。如果内存地址里面的值和A的值是一样的，那么就将内存里面的值更新成B。CAS是通过无限循环来获取数据的，如果在第一轮循环中，a线程获取地址里面的值被b线程修改了，那么a线程需要自旋，到下次循环才有可能机会执行。JDK8中彻底放弃了Segment转而采用的是Node，其设计思想也不再是JDK1.7中的分段锁思想。

Node：保存key，value及key的hash值的数据结构。其中value和next都用volatile修饰，保证并发的可见性。

```java
class Node<K,V> implements Map.Entry<K,V> {
    final int hash;
    final K key;
    volatile V val;
    volatile Node<K,V> next;
    //... 省略部分代码
}
```

Java8 ConcurrentHashMap结构基本上和Java8的HashMap一样，不过保证线程安全性。在JDK8中ConcurrentHashMap的结构，由于引入了红黑树，使得ConcurrentHashMap的实现非常复杂，我们都知道，红黑树是一种性能非常好的二叉查找树，其查找性能为O（logN），但是其实现过程也非常复杂，而且可读性也非常差，DougLea的思维能力确实不是一般人能比的，早期完全采用链表结构时Map的查找时间复杂度为O（N），JDK8中ConcurrentHashMap在链表的长度大于某个阈值的时候会将链表转换成红黑树进一步提高其查找性能。

# 内部类

1 成员内部类

他定义在另一个类中。**成员内部类可以无条件访问外部类的属性和方法，但是外部类想要访问内部类属性或方法时，必须要创建一个内部类对象，然后通过该对象访问内部类的属性或方法。**

2 局部内部类

局部内部类存在于方法中。他和成员内部类的区别在于**局部内部类的访问权限仅限于方法或作用域内。局部内部类就像局部变量一样，前面不能添加访问修饰符以及static修饰符。**

3 匿名内部类

通过实现接口创建一个匿名类对象传递过去。事实上还可以通过继承类来创建一个匿名内部类对象。
**注意事项**：匿名内部类没有构造方法。也是唯一没有构造方法的内部类。**匿名内部类和局部内部类只能访问外部类的final变量。**

4 静态内部类

静态内部类和成员内部类相比多了一个static修饰符。它与类的静态成员变量一般，是不依赖于外部类的。同时静态内部类也有它的特殊性。因为外部类加载时只会加载静态域，所以静态内部类不能使用外部类的非静态变量与方法。同时可以知道成员内部类里面是不能含静态属性或方法的。

静态内部类对象的创建一般是**外部类.内部类 类名 = new 外部类.内部类();**
成员内部类对象的创建一般是**外部类.内部类 类名 = 外部类对象名.new 内部类();**



# 反射

Java发射机制是指在运行状态下，对于任意一个类，都可以得到其方法与属性。

**发射机制的相关类**

| 类名          | 用途                                             |
| ------------- | ------------------------------------------------ |
| Class类       | 代表类的实体，在运行的Java应用程序中表示类和接口 |
| Field类       | 代表类的成员变量（成员变量也称为类的属性）       |
| Method类      | 代表类的方法                                     |
| Constructor类 | 代表类的构造方法                                 |

**class类**

- 获取Class类的方法

| 获取方法                       | 代码实现                                |
| ------------------------------ | --------------------------------------- |
| 1 通过实例化对象getClass()获取 | Class cl = new D().getClass();          |
| 2 通过Class.forName()获取      | Class cl = Class.forName("practice.D"); |
| 3 直接使用对象.class文件       | Class cl = D.class;l                    |

- Class类中的实例化方法

| 方法          | 用途       |
| ------------- | ---------- |
| newInstance() | 实例化对象 |

- 获取类中的**属性（fields）**

| 方法                          | 用途                   |
| ----------------------------- | ---------------------- |
| getField(String name)         | 获得某个公有的属性对象 |
| getFields()                   | 获得所有公有的属性对象 |
| getDeclaredField(String name) | 获得某个属性对象       |
| getDeclaredFields()           | 获得所有属性对象       |

对属性的操作：

```java
fields[1].getName() //获取属性名称
fields[1].getType().getSimpleName() //获取属性类型名称
int i = fields[1].getModifiers();
String modify = Modifier.toString(i); //获取修饰符
fields[0].set(d,111); //修改值，前提是该类已实例化并且不是private修饰，若是private修饰需要提前打破封装
fields[1].setAccessible(true); //打破封装
```

```java
    public static void main(String[] args) throws Exception {
        Class c = Class.forName("practice1.D");
        D d = (D) c.newInstance();
        System.out.println(d.a);
        Field[] fields = c.getDeclaredFields();
        fields[0].set(d,111);
        fields[1].setAccessible(true);
        fields[1].set(d,222);
        System.out.println(d.a);
    }
```

- 获取类中的**注解（Annotations）**

| 方法                                            | 用途                                   |
| ----------------------------------------------- | -------------------------------------- |
| getAnnotation(Class<A> annotationClass)         | 返回该类中与参数类型匹配的公有注解对象 |
| getAnnotations()                                | 返回该类所有的公有注解对象             |
| getDeclaredAnnotation(Class<A> annotationClass) | 返回该类中与参数类型匹配的所有注解对象 |
| getDeclaredAnnotations()                        | 返回该类所有的注解对象                 |

- 获取类中的**方法（Methods）**

| 方法                                                       | 用途                   |
| ---------------------------------------------------------- | ---------------------- |
| getMethod(String name, Class...<?> parameterTypes)         | 获得该类某个公有的方法 |
| getMethods()                                               | 获得该类所有公有的方法 |
| getDeclaredMethod(String name, Class...<?> parameterTypes) | 获得该类某个方法       |
| getDeclaredMethods()                                       | 获得该类所有方法       |

对方法的操作

```Java
Method method = c.getDeclaredMethod("play", int.class);
method.invoke(d,3);
```

- 获取类中的构造器（Constructor）

| 方法                                               | 用途                                   |
| -------------------------------------------------- | -------------------------------------- |
| getConstructor(Class...<?> parameterTypes)         | 获得该类中与参数类型匹配的公有构造方法 |
| getConstructors()                                  | 获得该类的所有公有构造方法             |
| getDeclaredConstructor(Class...<?> parameterTypes) | 获得该类中与参数类型匹配的构造方法     |
| getDeclaredConstructors()                          | 获得该类所有构造方法                   |

# 注解

Java 注解（Annotation）又称 Java 标注，是 JDK5.0 引入的一种注释机制。Java 语言中的类、方法、变量、参数和包等都可以被标注。Java 标注可以通过反射获取标注内容。在编译器生成类文件时，标注可以被嵌入到字节码中。Java 虚拟机可以保留标注内容，在运行时可以获取到标注内容 。 当然它也支持自定义 Java 标注。

<img src="C:\Users\wangxue\AppData\Roaming\Typora\typora-user-images\image-20211115185904538.png" alt="image-20211115185904538" style="zoom:33%;" />

- **JDK内置注解**：

```java
@Deprecated //用来标记的元素已经过时。

@Override //表示重写父类中的方法，只能注解方法，给编译器参考，和运行无关。标识性注解，给编译器参考，编译器 看到方法上有这个注解的时候，编译器会自动检查方法是否重写了父类的方法。

@SuppressWarnings //指示应该在注释元素中取消显示指定的编译器警告。
```

- **元注解**（用来注释注解的）：

Target :用来表示被标注的注解可以出现在哪个位置上

```java
@Target(ElementType.METHOD)//只能出现在方法上

@Target(value={METHOD,FIELD,CONSTRUCTOR});
```

Retention：用来表示被标注的注解最终保存在哪里

```java
@Retention(RetentionPolicy.SOURCE)//表示该注解只被保留在java源文件中

@Retention(RetentionPolicy.CLASS)//表示该注解被保存在class文件中

@Retention(RetentionPolicy.RUNTIME)//表示该注解被保存在class文件中，并且可以被反射机制读取
```

- **自定义注解**：

```java
@Target(value = {ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)//该注解保存在class文件中,并且可以通过反射机制获得
public @interface MyAnnotation {
    String value();
    String[] name();
}
@MyAnnotation(value = "wangxue",name={"wx","tj"})
class A{
}
```

# 异常

Java中的异常主要分为Error和Exception。

![image-20211115195939731](C:\Users\wangxue\AppData\Roaming\Typora\typora-user-images\image-20211115195939731.png)

**Error** 指Java程序运行错误，如果程序在启动时出现Error，则启动失败；如果程序运行过程中出现Error，则系统将退出程序。出现Error是系统的内部错误或资源耗尽，Error不能在程序运行过程中被动态处理，一旦出现Error，系统能做的只有记录错误的原因和安全终止。

**Exception** 指 Java程序运行异常，在运行中的程序发生了程序员不期望发生的事情，可以被Java异常处理机制处理。Exception也是程序开发中异常处理的核心，可分为RuntimeException（运行时异常）和CheckedException（检查异常），如下图所示

<img src="C:\Users\wangxue\AppData\Roaming\Typora\typora-user-images\image-20211115200027008.png" alt="image-20211115200027008" style="zoom:67%;" />

异常处理：

（1）**抛出异常**：遇到异常时不进行具体的处理，直接将异常抛给调用者，让调用者自己根据情况处理。抛出异常的三种形式：throws、throw和系统自动抛出异常。其中throws作用在方法上，用于定义方法可能抛出的异常；throw作用在方法内，表示明确抛出一个异常。

（2）**使用try catch捕获并处理异常**：使用费try catch 捕获异常能够有针对性的处理每种可能出现的异常，并在捕获到异常后根据不同的情况做不同的处理。其使用过程比较简单：用try catch语句块将可能出现异常的代码包起来即可。

# 浅拷贝与深拷贝

- **浅拷贝**

按位拷贝对象，它会创建一个**新对象**，这个对象有着原始对象属性值的一份精确拷贝。如果属性是**基本类型**，拷贝的就是基本类型的值；如果属性是**内存地址**（引用类型），拷贝的就是内存地址 ，因此如果其中一个对象改变了这个地址，就会**影响**到另一个对象。（**浅拷贝仅仅复制所考虑的对象，而不复制它所引用的对象。**）

两个引用`student1`和`student2`指向不同的两个对象，但是两个引用`student1`和`student2`中的两个`teacher`**引用指向的是同一个对象**，所以说明是**浅拷贝**。

<img src="C:\Users\wangxue\AppData\Roaming\Typora\typora-user-images\image-20211115202824834.png" alt="image-20211115202824834" style="zoom:67%;" />

实现方式：

```java
public class Test4 {
    public static void main(String[] args) throws CloneNotSupportedException {
        Teacher teacher = new Teacher();
        teacher.setName("teacher Li");
        Student student1 = new Student();
        student1.setAge(18);
        student1.setName("wangxue");
        student1.setTeacher(teacher);
        Student student2 = (Student)student1.clone();
        student2.setAge(19);
        student2.setName("tangjuan");
        System.out.println(student1);
        System.out.println(student2);
        Teacher teacher2 = student2.getTeacher();
        teacher2.setName("Teacher Zhang");
        System.out.println(student1);
        System.out.println(student2);
    }
}
class Student implements Cloneable{
    private int age;
    private String name;
    private Teacher teacher;
    @Override
    public String toString() {
        return "Student{" +
                "age=" + age +
                ", name='" + name + '\'' +
                ", teacher=" + teacher +
                '}';
    }
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public Teacher getTeacher() {
        return teacher;
    }
    public void setTeacher(Teacher teacher) {
        this.teacher = teacher;
    }
}
class Teacher{
    private String name;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Teacher{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

结果：

```java
Student{age=18, name='wangxue', teacher=Teacher{name='teacher Li'}}
Student{age=19, name='tangjuan', teacher=Teacher{name='teacher Li'}}
Student{age=18, name='wangxue', teacher=Teacher{name='Teacher Zhang'}}
Student{age=19, name='tangjuan', teacher=Teacher{name='Teacher Zhang'}}
```

验证了浅拷贝创建了一个新的对象，对于基本数据类型此对象拷贝了其值，修改一个对象的基本数据类型不会影响另一个对象，但是对于引用数据类型，拷贝的是地址，修改会对另一个对象带来影响。

- **深拷贝**

在拷贝引用类型成员变量时，为引用类型的数据成员另辟了一个独立的内存空间，实现真正内容上的拷贝。（深拷贝把要复制的对象所引用的对象都复制了一遍。）

两个引用`student1`和`student2`指向不同的两个对象，两个引用`student1`和`student2`中的两个`teacher`引用指向的是两个对象，但对`teacher`对象的修改只能影响`student1`对象,所以说是`深拷贝`。

<img src="C:\Users\wangxue\AppData\Roaming\Typora\typora-user-images\image-20211115203048726.png" alt="image-20211115203048726" style="zoom:67%;" />

**实现方式1**（将引用数据类型实现cloneable）：

```java
public class Test4 {
    public static void main(String[] args) throws CloneNotSupportedException {
        Teacher teacher = new Teacher();
        teacher.setName("teacher Li");
        Student student1 = new Student();
        student1.setAge(18);
        student1.setName("wangxue");
        student1.setTeacher(teacher);
        Student student2 = (Student)student1.clone();
        student2.setAge(19);
        student2.setName("tangjuan");
        Teacher teacher2 = student2.getTeacher();
        teacher2.setName("Teacher Zhang");
        System.out.println(student1);
        System.out.println(student2);
    }
}
class Student implements Cloneable{
    private int age;
    private String name;
    private Teacher teacher;
    @Override
    public String toString() {
        return "Student{" +
                "age=" + age +
                ", name='" + name + '\'' +
                ", teacher=" + teacher +
                '}';
    }
    @Override
    protected Object clone() throws CloneNotSupportedException {
        Student student = (Student)super.clone();
        student.setTeacher((Teacher) teacher.clone());
        return student;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public Teacher getTeacher() {
        return teacher;
    }
    public void setTeacher(Teacher teacher) {
        this.teacher = teacher;
    }
}
class Teacher implements Cloneable{
    private String name;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
    @Override
    public String toString() {
        return "Teacher{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

结果：

```java
Student{age=18, name='wangxue', teacher=Teacher{name='teacher Li'}}
Student{age=19, name='tangjuan', teacher=Teacher{name='Teacher Zhang'}}
```

**实现方式2**（序列化实现）：

```java
public class Test4 {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        Teacher teacher = new Teacher();
        teacher.setName("Teacher Li");
        Student student1 = new Student();
        student1.setAge(18);
        student1.setName("wangxue");
        student1.setTeacher(teacher);
        Student student2 = copyStudent(student1);
        student2.setAge(19);
        student2.setName("wangxue");
        student2.getTeacher().setName("Teacher Zhang");
        System.out.println(student1);
        System.out.println(student2);
    }
    private static Student copyStudent(Student student) throws IOException, ClassNotFoundException {
        Student re = null;
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        ObjectOutputStream oop = new ObjectOutputStream(baos);
        oop.writeObject(student);
        ObjectInputStream ois = new ObjectInputStream(new ByteArrayInputStream(baos.toByteArray()));
        re = (Student) ois.readObject();
        return re;
    }
}
class Student implements Serializable {
    private static final long serialVersionUID = -2531333633577348256L;
    private int age;
    private String name;
    private Teacher teacher;
    @Override
    public String toString() {
        return "Student{" +
                "age=" + age +
                ", name='" + name + '\'' +
                ", teacher=" + teacher +
                '}';
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public Teacher getTeacher() {
        return teacher;
    }
    public void setTeacher(Teacher teacher) {
        this.teacher = teacher;
    }
}
class Teacher implements Serializable{
    private static final long serialVersionUID = -1207643466142373552L;
    private String name;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    @Override
    public String toString() {
        return "Teacher{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

结果：

```java
Student{age=18, name='wangxue', teacher=Teacher{name='Teacher Li'}}
Student{age=19, name='wangxue', teacher=Teacher{name='Teacher Zhang'}}
```

- **浅拷贝与深拷贝的特点**

 **浅拷贝特点**

(1) 对于基本数据类型的成员对象，因为基础数据类型是值传递的，所以是直接将属性值赋值给新的对象。基础类型的拷贝，其中一个对象修改该值，不会影响另外一个。
(2) 对于引用类型，比如数组或者类对象，因为引用类型是引用传递，所以浅拷贝只是把内存地址赋值给了成员变量，它们指向了同一内存空间。改变其中一个，会对另外一个也产生影响。

 **深拷贝特点**

(1) 对于基本数据类型的成员对象，因为基础数据类型是值传递的，所以是直接将属性值赋值给新的对象。基础类型的拷贝，其中一个对象修改该值，不会影响另外一个（和浅拷贝一样）。
(2) 对于引用类型，比如数组或者类对象，深拷贝会新建一个对象空间，然后拷贝里面的内容，所以它们指向了不同的内存空间。改变其中一个，不会对另外一个也产生影响。
(3) 对于有多层对象的，每个对象都需要实现 `Cloneable` 并重写 `clone()` 方法，进而实现了对象的串行层层拷贝。
(4) 深拷贝相比于浅拷贝速度较慢并且花销较大。

# IO流

- 定义

IO流：通过IO可以完成硬盘文件的读和写（数据传输的通道）。

![image-20211116153941296](C:\Users\wangxue\AppData\Roaming\Typora\typora-user-images\image-20211116153941296.png)

- 分类

![image-20211116160034845](C:\Users\wangxue\AppData\Roaming\Typora\typora-user-images\image-20211116160034845.png)

- 具体介绍

流只能默认创建文件，不能创建目录。

1. 文件专属流

   FileInputStream

   FileOutputStream（**写文件需要在末尾进行flush刷新**）

   FileReader

   FileWriter

   文件复制：

   ```java
       public static void main(String[] args) {
           FileInputStream fis=null;
           FileOutputStream fos=null;
           try {
               fis=new FileInputStream("D:\\Java\\bk.jpg");
               fos=new FileOutputStream("D:\\Java\\bk1.jpg",true);
               byte[] bytes=new byte[1024];
               int reader=0;
               while((reader=fis.read(bytes))!=-1){
                   fos.write(bytes,0,reader);
               }
               fos.flush();
           } catch (FileNotFoundException e) {
               e.printStackTrace();
           } catch (IOException e) {
               e.printStackTrace();
           }finally {
               if(fis!=null) {
                   try {
                       fis.close();
                   } catch (IOException e) {
                       e.printStackTrace();
                   }
               }
               if (fos!=null) {
                   try {
                       fos.close();
                   } catch (IOException e) {
                       e.printStackTrace();
                   }
               }
           }
       }
   ```

   目录复制（先建目录，在复制文件）：

   ```java
   public class DirCopy {
       public static void main(String[] args) {
           File file1=new File("D:\\Java\\面经");
           File file2=new File("D:\\Java\\面经2");/**
            文件夹不再目录时，需要创建。流只能自动创建文件，不能自动创建目录
            */
           file2.mkdir();
           copy(file1,file2);
       }
       //问题在这！！！！！
       public static void copy(File file1, File file2){
           File[] files=file1.listFiles();
           for (int i=0;i<files.length;i++){
               //若子目录中有文件就进行复制
               if(files[i].isFile()){
                   File file3=new File(file2+"\\"+files[i].getName());
                   trans(files[i],file3);
               }
               //子目录为目录的话，则继续读，并且需要创建的目录也需要改变
               else{
                   File file3=new File(file2+"\\"+files[i].getName());
                   file3.mkdir();
                   copy(files[i],file3);
               }
           }
       }
       /**
        * 文件复制具体过程
        * @param file1
        * @param file2
        */
       private static void trans(File file1,File file2){
           FileInputStream fileInputStream=null;
           FileOutputStream fileOutputStream=null;
           try {
               fileInputStream=new FileInputStream(file1);
               fileOutputStream=new FileOutputStream(file2);
               byte[] bytes=new byte[1024*1024];//1M
               int reader=0;
               while((reader=fileInputStream.read(bytes))!=-1){
                   fileOutputStream.write(bytes,0,reader);
               }
               fileOutputStream.flush();
           } catch (FileNotFoundException e) {
               e.printStackTrace();
           } catch (IOException e) {
               e.printStackTrace();
           }finally {
               if(fileInputStream!=null){
                   try {
                       fileInputStream.close();
                   } catch (IOException e) {
                       e.printStackTrace();
                   }
               }
               if (fileOutputStream!=null){
                   try {
                       fileOutputStream.close();
                   } catch (IOException e) {
                       e.printStackTrace();
                   }
               }
           }
       }
   }
   ```

   

2. 转换流

   InputStreamReader

   OutputStreamWriter

3. 缓冲流

   BufferedInputStream

   BufferedOutputStream

   BufferedWriter

   BufferedReader

4. 数据流

   DateInputStream

   DateOutputStream

5. 标准输出流

   PrintWriter

   PrintStream

6. 对象专属流（**用于对象序列化**）

   ObjectInputStream

   ObjectOutputStream

   <img src="C:\Users\wangxue\AppData\Roaming\Typora\typora-user-images\image-20211116191119219.png" alt="image-20211116191119219" style="zoom:67%;" />

   序列化：

   ```java
   public class Serialize {
       public static void main(String[] args) throws Exception{
           List<Student> list=new ArrayList<>();
           list.add(new Student(111,"wangxue1"));
           list.add(new Student(222,"wangxue2"));
           list.add(new Student(333,"wangxue3"));
           ObjectOutputStream objectOutputStream=new ObjectOutputStream(
                   new FileOutputStream("D:\\Java\\序列化1"));
           objectOutputStream.writeObject(list);
           objectOutputStream.flush();
           objectOutputStream.close();
       }
   }
   class Student implements Serializable {
       private int id;
       private transient String name;
       public Student(int id, String name) {
           this.id = id;
           this.name = name;
       }
       public int getId() {
           return id;
       }
       public String getName() {
           return name;
       }
       @Override
       public String toString() {
           return "Student{" +
                   "id=" + id +
                   ", name='" + name + '\'' +
                   '}';
       }
   }
   ```

   反序列化：

   ```java
   public class Deserialize {
       public static void main(String[] args) throws Exception{
           ObjectInputStream objectInputStream=new ObjectInputStream(
                   new FileInputStream("D:\\Java\\序列化1"));
           List<Student> list=(List<Student>) objectInputStream.readObject();
           for (Student s:
                list) {
               System.out.println(s);
           }
       }
   }
   ```

   
