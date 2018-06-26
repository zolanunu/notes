## jdk:java development kit

java开发工具箱

### eclipse markdown

百度上的方法用不上

java 对大小写敏感

修饰符：public private friendly...

main方法必须声明为public

java解释器

java虚拟机

注释：eclipse中的快捷键设置

java是一种强类型语言，说明每一个变量声明一种类型

数据类型：

整型（int,short,long,byte）;byte,short类型主要用于特定的应用场合，例如，底层文件处理或者需要控制占用存储空间量的大数组

在java中，整型的范围与运行java代码的机器无关。java都是有符号类型

浮点类型:float,double

char类型

boolean

变量：声明要被初始化

常量：一般是使用大写CM，final关键字，定义在main方法外部

运算符

### 字符串String类

StringBuilder和StringBuffer

### 大数值

BigInteger:任意精度的整数运算

BigDecimal:任意精度的浮点数运算

访问器方法和更改器方法:

不可变的类和可变类：final

static：将域定义为static，每个类中只有一个这样的域。而每一个对象对于所有的实例域都有自己的一份拷贝，但是对于static修饰的域，是所有实例所共有的

静态常量：final + static

静态方法：不能向对象实施操作的方法，没有隐式参数，this关键字在非静态方法中，就表示这个方法的隐式参数

方法参数：值调用和对象引用

值调用：方法接受的是调用者提供的值

引用调用：表示方法接收的是调用者提供的变量地址

### 对象参数的问题
### 重载overload

签名不同


构造器：重载构造器，this(...)调用另一个构造器
垃圾回收

public修饰的意味着该类或者方法可以分享。但是没有public和private 修饰的方法变量类等可以被同一个包中的所有方法访问
### 文档注释

javadoc实用程序

方法注释

域注释

通用注释