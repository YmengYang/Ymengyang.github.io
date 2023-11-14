# 为什么说java语言“编译与解释并存”

java同时具有编译型语言和解释性语言的特征。由java编写的程序先通过编译，生成字节码（.class文件），这种字节码必须由java解释器来解释执行。

> 编译型语言——编译型语言会通过编译器将源代码一次性翻译成可被该平台执行的机器码。一般情况下，编译语言的执行速度比较快，开发效率比较低。常见的编译性语言有 C、C++、Go、Rust 等等。
> 解释型语言——解释型语言会通过解释器一句一句的将代码解释（interpret）为机器代码后再执行。解释型语言开发效率比较快，执行速度比较慢。常见的解释性语言有 Python、JavaScript、PHP 等等。

Java 程序从源代码到运行的过程如下图所示：

![Image](image/2023-11-13-Resume之java基础/1699877456937.png)

# 基本类型和包装类型的区别

+ 存储方式：基本数据类型的局部变量存放在java虚拟机栈中的局部变量表中，成员变量（未被static修饰）存放在java虚拟机的堆中。包装类型属于对象类型，几乎所有对象实例都存于堆中。
+ 占用空间：基本数据类型所占用空间小
+ 默认值：成员变量包装类型不赋值就是null，而基本数据类型有默认值且不是null。
+ 比较方式：对于基本类型，==比较的是值。对于包装类型，==比较的是对象的内存地址，要比较包装类对象之间的值，用equals().

**为什么说是几乎所有对象实例都存在于堆中呢？** 这是因为 HotSpot 虚拟机引入了 JIT 优化之后，会对对象进行逃逸分析，如果发现某一个对象并没有逃逸到方法外部，那么就可能通过标量替换来实现栈上分配，而避免堆上分配内存

# 包装类型的缓存机制
java基本数据类型的包装类型大部分都用到了缓存机制来提升性能。

Byte,Short,Integer,Long这4种包装类型默认创建了数值[-128,127]的相应类型的缓存数据，Character创建了数值在[0,127]范围的缓存数据，Boolean直接返回True或false。

如果超出对应范围仍然会去创建新的对象，缓存的范围区间的大小只是在性能和资源之间的权衡。


两种浮点数类型的包装类Float,Double并没有实现缓存机制。
~~~java
Integer i1 = 33;
Integer i2 = 33;
System.out.println(i1 == i2);// 输出 true

Float i11 = 333f;
Float i22 = 333f;
System.out.println(i11 == i22);// 输出 false

Double i3 = 1.2;
Double i4 = 1.2;
System.out.println(i3 == i4);// 输出 false
~~~


下面我们来看一个问题：下面的代码的输出结果是 true 还是 false 呢？
~~~java
Integer i1 = 40;
Integer i2 = new Integer(40);
System.out.println(i1==i2);
~~~
`Integer i1 = 40`这一行代码会发生装箱，也就是说这行代码等价于 `Integer i1=Integer.valueOf(40)` 。因此，i1 直接使用的是缓存中的对象。而`Integer i2 = new Integer(40) `会直接创建新的对象。  
因此答案是false。


**注意：所有整型包装类对象之间值的比较，全部使用 equals 方法比较。**  
对于`Integer var = ?`在-128~127之间的赋值，Integer对象是在IntegerCache.cache产生，会复用已有对象，这个区间内的Integer值可以直接使用==进行判断，但是这个区间之外的所有数据，都会在堆上产生，并不会复用已有对象。区间内的Integer值可以直接使用==进行判断，但是区间外的所有数据，都会在堆上产生，并不会复用已有对象。  

结合Integer缓存源码理解:
~~~java
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
private static class IntegerCache {
    static final int low = -128;
    static final int high;
    static {
        // high value may be configured by property
        int h = 127;
    }
}
~~~

