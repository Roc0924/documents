###内存模型

内存模型可以理解为在特定的操作协议下，对特定的内存或者高速缓存进行读写访问的过程抽象


- java内存模型

Java内存模型中规定了所有的变量都存储在主内存中，每条线程还有自己的工作内存（可以与前面讲的处理器的高速缓存类比），线程的工作内存中保存了该线程使用到的变量到主内存副本拷贝，线程对变量的所有操作（读取、赋值）都必须在工作内存中进行，而不能直接读写主内存中的变量。不同线程之间无法直接访问对方工作内存中的变量，线程间变量值的传递均需要在主内存来完成，线程、主内存和工作内存的交互关系如下图所示：




---

```java

/**
 * Create with IntelliJ IDEA
 * Author               : wangzhenpeng
 * Date                 : 2017/11/30
 * Time                 : 10:04
 * Description          :
 */
public class Test {
    public static void main(String[] args) {
        Integer a = 1;
        Integer b = 2;
        Integer c = 3;
        Integer d = 3;
        Integer e = 321;
        Integer f = 321;
        Long g = 3L;

        System.out.println(c == d);
        System.out.println(e == f);
        System.out.println(c == (a+b));
        System.out.println(c.equals(a+b));
        System.out.println(g == (a+b));
        System.out.println(g.equals(a+b));
    }
}

```

运行结果

```
true
false
true
true
true
false
```
对于c == d比较及结果为true
首先，我们需要知道，对于对象的比较==比较的是对象的地址。
其次，我们需要了解自动装箱调用的Integer.valueOf(int i);的源码：

```
    public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
```
当i在[-128~127]之间时，使用缓存实例。所以c，d 返回的地址相同，所以返回true。
同理e == f返回false容易理解。
PS:

```
Integer a = new Integer(100);
Integer b = new Integer(100);
System.out.println(a == b);
```
输出结果为false。这又是为什么呢？
a和b是通过new操作创建出来的，分别在堆上开创空间。所以返回的地址不相同；

那么为什么c == (a+b) 返回的是true呢？
要知道==如果不遇到算数运算是不会触发自动拆箱的。有这个前提，说明此时比较的不再是对象的地址，而是对象的值 3 == （1 + 2）

c.equals(a+b)则是自动拆箱再装箱，并且比较的是对象的值。

g == (a+b)返回true，说明==触发自动拆箱的时候，还触发了类型转换

g.equals(a+b)返回false，说明equals不会触发类型转换

---
