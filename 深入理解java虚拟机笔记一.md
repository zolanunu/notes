## 深入理解java虚拟机（一）

java虚拟机，和java基础也是不可分割的，也是java平台中最底层的一部分。学习java虚拟机可以知道在new的过程中到底发生了什么；内存的模式是什么样子的；再者java虚拟机中提供了一套GC机制，也能很清楚的了解java虚拟机是如何收集内存垃圾的；就算如此，可能也会出现内存溢出和内存泄漏的情况，是为什么；因此，对这些有困惑，学习java虚拟机能帮助我们知道一些在java运行程序的过程中，到底发生了什么。

## 写在笔记之前

啃深入理解java虚拟机这本书，也是第二遍了，第一遍只是详细看了内存模式和java回收垃圾机制以及线程的内容，但是没做总结。趁着者二遍的复习，再加深一下理解吧。也会参考一些网上的思考。

学习，任重而道远。也希望自己在痛苦中破茧重生。这段时间，最难熬的时间是在吃饭点和睡觉时间。

### 自动内存管理机制

#### java虚拟机运行时数据区

![](/img/java_jvm_1.png)

java虚拟机在执行java程序的过程中，会把它管理的内存划分为若干个不同的数据区域。不同的区域有不同的作用，也存储着不同的数据，生存时间也是各不一样的，有的依赖于虚拟机进程，有的依赖于用户线程。

java运行时数据区主要的划分如上面图所示。

接下来详细说一些每个区域的作用和注意点。

这里给一个自己的记忆技巧吧：两栈一堆一区一器

#### 程序计数器

```
The Java Virtual Machine can support many threads of execution at once (JLS §17).
Each Java Virtual Machine thread has its own pc (program counter) register.
At any point, each Java Virtual Machine thread is executing the code of a single method, 
namely the current method (§2.6) for that thread.
If that method is not native, the pc register contains the address of the Java Virtual
Machine instruction currently being executed.
If the method currently being executed by the thread is native, 
the value of the Java Virtual Machine's pc register is undefined.
The Java Virtual Machine's pc register is wide enough to hold a returnAddress or
a native pointer on the specific platform.
```

- 程序计数器在java虚拟机内存中较小的内存空间，被看作是当前线程所执行的字节码的行号指示器；

- 在虚拟机的概念模型中，字节码解释器工作时就是通过改变这个计数器的值来选择下一条需要执行的字节码指令，分支，循环，跳转，异常处理，线程恢复等基功能都需要依赖这个计数器来完成；

- 由于java虚拟机的多线程是通过线程轮流切换并分配处理器执行时间的方式来实现的，在任何一个确定的时刻，一个处理器都只会执行一条线程中的指令。所以，为了线程切换后都能切换到正确的执行位置，每条线程都需要有一个独立的程序计数器，各个线程之间计数器不影响，独立存储，程序计数器是内存区域中线程独有的；

- 如果线程正在执行一个java方法，那么计数器记录的是正在执行的虚拟机字节码指令的地址，如果执行的是Native方法，计数器值为空，这个内存区域就是null，空；

- 这片内存区域是唯一一个在java虚拟机规范中没有规定任何OutOfMemoryError情况的区域。

#### 方法区

- 各个线程共享的内存区域；

- 存储被java虚拟机加载的类信息，常量，静态变量，即时编译器编译后的代码等数据。虽然java虚拟机规范把方法区描述成堆的一个逻辑部分，但是也有一个别名：非堆，也是为了和堆区分开；

- HotSpot虚拟机使用”永久代“来实现方法区，如此，HotSpot的垃圾收集器可以像管理java堆一样管理这部分内存，能够省去专门为方法区编写内存管理代码的工作。

- java虚拟机规范对方法区的限制很宽松，除了和java堆一样不需要连续的内存和可以选择固定大小或者可以扩展外，还可以选择不实现垃圾收集。

- 相对而言，垃圾收集行为在这个区域是比较少出现的，但是并非数据进入了方法区就永久存在了。

- 这个区域的内存回收目标主要是针对常量池的回收和对类型的卸载，一般来说，这个区域的回收成绩比较难以令人满意，尤其是类型的卸载，条件很苛刻，但是这部分区域的回收确实是必要的；

- 根据java虚拟机规范，当方法区无法满足内存分配需求的时候，将抛出OutOfMeomoryError异常。

#### 运行时常量池

Java中的常量池，实际上分为两种形态：静态常量池和运行时常量池。

**静态常量池**，即*.class文件中的常量池，class文件中的常量池不仅仅包含字符串(数字)字面量，还包含类、方法的信息，占用class文件绝大部分空间。

**运行时常量池**则是jvm虚拟机在完成类装载操作后，将class文件中的常量池载入到内存中，并保存在方法区中，我们常说的常量池，就是指方法区中的运行时常量池。

- 运行时常量池是方法区的一部分；

- **Class文件**中除了有类的版本，字段，方法，接口等描述信息外，还有一项信息是常量池，用于存放编译器生成的各种字面量和符号引用。

- java虚拟机规范对class文件每一个部分很严格的规定，但是对运行时常量池，并没有很细节的要求。

- 运行时常量池相对于Class文件常量池的另外一个重要特征是具备动态性，java语言并不要求常量一定只有编译期才能产生，也就是并非预置入Class文件常量池的内容才能进入方法区运行时常量池，运行期间也可能将新的常量放入池中。比如：String的intern()方法。

- 运行时常量池是方法区的一部分，自然受到方法区内存的限制，当常量池无法在申请到内存时会抛出OutOfMemoryError异常。

#### java堆

- 线程共享的，是java虚拟机管理的最大的一块内存；

- 这块区域存放的是对象实例，所有的对象实例和数组都要在堆上分配；

- java堆是垃圾收集器管理的主要区域；

- 采用分代收集的算法来进行垃圾回收，java堆就分为了新生代和老年代（还可以进一步细化）；

- 如果堆中没有内存完成实例分配，并且堆也没有办法再扩展，就会抛出OutOfMemoryError异常。

#### 本地方法栈

本地方法栈是jvm调用操作系统方法所使用的栈。

- 虚拟机栈是执行java方法（字节码），本地方法栈是为了虚拟机中使用到的Native方法服务；

- 会抛出StackOverflowErrorh和OutOfMemoryError异常；

#### java虚拟机栈

- 线程私有的，生命周期与线程一致

- 虚拟机栈描述的是java方法执行的内存模式：每个方法执行的时候都会创建一个栈帧，用来存储局部变量表，操作数栈，动态连接，方法出口等信息。

- java虚拟机栈中，如果线程请求的栈深度大于虚拟机所允许的深度，将会抛出StackOverflowError异常；

- 如果虚拟机栈可以动态扩展，但是扩展也没能申请到足够的内存，将会抛出OutOfMemoryEroor异常。

### 参考

- java virtual specification machine： https://docs.oracle.com/javase/specs/jvms/se7/html/index.html

- 深入理解java虚拟机
