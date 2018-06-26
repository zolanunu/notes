## 异常处理

### 概念

异常是程序中出现的一些错误，但是并不是所有的错误都是异常，并且错误有时间是可以避免的。

异常发生的原因有很多种，但是主要有：

- 输入非法数据

- 要打开的文件不存在

- 网络通信时连接中断，或者JVM内存溢出

上面这些异常都是因为用户错误引起的，也有的是程序错误引起，还有因为物理错误引起的。

在理解java异常是如何工作的，需要知道，异常主要分为三个类型：

- **检查性异常**：最具代表的检查性异常是用户错误或问题引起的异常，这是程序员无法预见的。例如要打开一个不存在文件时，一个异常就发生了，这些异常在编译时不能被简单地忽略。

- **运行时异常**：行时异常是可能被程序员避免的异常。与检查性异常相反，运行时异常可以在编译时被忽略。

- **错误**：错误不是异常，而是脱离程序员控制的问题。错误在代码中通常被忽略。例如，当栈溢出时，一个错误就发生了，它们在编译也检查不到的。

### Exception类

![](./img/javabase_Exception1.jpg)

#### java 内置异常类

#### 异常方法

在Throwable类中的方法

```
public String getMessage()
返回关于发生的异常的详细信息。
这个消息在Throwable类的构造函数中初始化了。
public Throwable getCause()
返回一个Throwable 对象代表异常原因。
public String toString()
使用getMessage()的结果返回类的串级名字。
public void printStackTrace()
打印toString()结果和栈层次到System.err，即错误输出流。
public StackTraceElement [] getStackTrace()
返回一个包含堆栈层次的数组。
下标为0的元素代表栈顶，最后一个元素代表方法调用堆栈的栈底。
public Throwable fillInStackTrace()
用当前的调用栈层次填充Throwable对象栈层次，
添加到栈层次任何先前信息中。
```

#### 捕获异常

使用 try 和 catch 关键字可以捕获异常。try/catch 代码块放在异常可能发生的地方。

try/catch代码块中的代码称为保护代码，使用 try/catch 的语法如下：

```
// 单层try/catch
try
{
   // 程序代码
}catch(ExceptionName e1)
{
   //Catch 块
}
import java.io.*;
public class ExcepTest{
 
   public static void main(String args[]){
      try{
         int a[] = new int[2];
         System.out.println("Access element three :" + a[3]);
      }catch(ArrayIndexOutOfBoundsException e){
         System.out.println("Exception thrown  :" + e);
      }
      System.out.println("Out of the block");
   }
}
// 多重捕获

try{
   // 程序代码
}catch(异常类型1 异常的变量名1){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}
```

#### throw/throws

如果一个方法没有捕获一个检查性异常，那么该方法必须使用 throws 关键字来声明。throws 关键字放在方法签名的尾部。

也可以使用 throw 关键字抛出一个异常，无论它是新实例化的还是刚捕获到的。

#### finally关键字

finally 关键字用来创建在 try 代码块后面执行的代码块。

无论是否发生异常，finally 代码块中的代码总会被执行。

在 finally 代码块中，可以运行清理类型等收尾善后性质的语句。

finally 代码块出现在 catch 代码块最后，语法如下：

```
try{
  // 程序代码
}catch(异常类型1 异常的变量名1){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}finally{
  // 程序代码
}
```

首先一个不容易理解的事实：在 try块中即便有return，break，continue等改变执行流的语句，finally也会执行。

也就是说：try…catch…finally中的return 只要能执行，就都执行了，他们共同向同一个内存地址（假设地址是0×80）写入返回值，后执行的将覆盖先执行的数据，而真正被调用者取的返回值就是最后一次写入的。

![](./img/javabase_Exception.gif)

### 自定义异常类

```
class MyException extends Exception{
}
import java.io.*;
 
//自定义异常类，继承Exception类
public class InsufficientFundsException extends Exception
{
  //此处的amount用来储存当出现异常（取出钱多于余额时）所缺乏的钱
  private double amount;
  public InsufficientFundsException(double amount)
  {
    this.amount = amount;
  } 
  public double getAmount()
  {
    return amount;
  }
}
```

### 异常链化

在一些大型的，模块化的软件开发中，一旦一个地方发生异常，则如骨牌效应一样，将导致一连串的异常。假设B模块完成自己的逻辑需要调用A模块的方法，如果A模块发生异常，则B也将不能完成而发生异常，但是B在抛出异常时，会将A的异常信息掩盖掉，这将使得异常的根源信息丢失。异常的链化可以将多个模块的异常串联起来，使得异常信息不会丢失。

异常链化:以一个异常对象为参数构造新的异常对象。新的异对象将包含先前异常的信息。这项技术主要是异常类的一个带Throwable参数的函数来实现的。这个当做参数的异常，我们叫他根源异常（cause）。

查看Throwable类源码，可以发现里面有一个Throwable字段cause，就是它保存了构造时传递的根源异常参数。这种设计和链表的结点类设计如出一辙，因此形成链也是自然的了。

```
public class Throwable implements Serializable {
    private Throwable cause = this;
    
    public Throwable(String message, Throwable cause) {
        fillInStackTrace();
        detailMessage = message;
        this.cause = cause;
    }
     public Throwable(Throwable cause) {
        fillInStackTrace();
        detailMessage = (cause==null ? null : cause.toString());
        this.cause = cause;
    }
     
    //........
}
```

### 通用异常

- **JVM(Java虚拟机) 异常**：由 JVM 抛出的异常或错误。例如：NullPointerException 类，ArrayIndexOutOfBoundsException 类，ClassCastException 类。

- **程序级异常**：由程序或者API程序抛出的异常。例如 IllegalArgumentException 类，IllegalStateException 类。


### 注意事项

- 当子类重写父类的带有 throws声明的函数时，其throws声明的异常必须在父类异常的可控范围内——用于处理父类的throws方法的异常处理器，必须也适用于子类的这个带throws方法 。这是为了支持多态。

例如，父类方法throws 的是2个异常，子类就不能throws 3个及以上的异常。父类throws IOException，子类就必须throws IOException或者IOException的子类。

```
class Father
{
    public void start() throws IOException
    {
        throw new IOException();
    }
}
 
class Son extends Father
{
    public void start() throws Exception
    {
        throw new SQLException();
    }
}
/*******假设上面的代码是允许的（实质是错误的）********/
class Test
{
    public static void main(String[] args)
    {
        Father[] objs = new Father[2];
        objs[0] = new Father();
        objs[1] = new Son();
 
        for(Father obj:objs)
        {
        //因为Son类抛出的实质是SQLException，
	//而IOException无法处理它。
        //那么这里的try。。catch就不能处理Son中的异常。
        //多态就不能实现了。
            try {
                 obj.start();
            }catch(IOException)
            {
                 //处理IOException
            }
         }
   }
}
```

- Java程序可以是多线程的。每一个线程都是一个独立的执行流，独立的函数调用栈。如果程序只有一个线程，那么没有被任何代码处理的异常会导致程序终止。如果是多线程的，那么没有被任何代码处理的异常仅仅会导致异常所在的线程结束。

也就是说，Java中的异常是线程独立的，线程的问题应该由线程自己来解决，而不要委托到外部，也不会直接影响到其它线程的执行。

### 如何设计异常

参考： `http://www.importnew.com/28000.html`