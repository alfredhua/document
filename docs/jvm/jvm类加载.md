
https://docs.oracle.com/javase/specs/jvms/se8/html/index.html

1. 加载：
    
    查找和导入class文件,

    - 通过一个类的全限定名获取定义此类的二进制字节流
    - 将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构 
    - 在Java堆中生成一个代表这个类的java.lang.Class对象，作为对方法区中这些数据的访问入口

2. 链接
 - 验证：保证被加载类的正确性
  文件格式验证，元数据验证，字节码验证，符号引用验证
 - 准备：为类的静态变量分配内存，并将其初始化为默认值
 - 解析：把类中的符号引用转换为直接引用

3. 初始化

   对类的静态变量，静态代码块执行初始化操作

![image](http://java-run-blog.oss-cn-zhangjiakou.aliyuncs.com/585ec14a09f64b85b90b3855a1cee8ce.png)

## 2、类装载器ClassLoader
1. Bootstrap ClassLoader 负责加载$JAVA_HOME中 jre/lib/rt.jar 里所有的class或 Xbootclassoath选项指定的jar包。由C++实现，不是ClassLoader子类。
2. Extension ClassLoader 负责加载java平台中扩展功能的一些jar包，包括$JAVA_HOME中 jre/lib/*.jar 或 -Djava.ext.dirs指定目录下的jar包。
3. App ClassLoader 负责加载classpath中指定的jar包及 Djava.class.path 所指定目录下的类和 jar包。
4. Custom ClassLoader 通过java.lang.ClassLoader的子类自定义加载class，属于应用程序根据 自身需要自定义的ClassLoader，如tomcat、jboss都会根据j2ee规范自行实现ClassLoader。
![image](http://java-run-blog.oss-cn-zhangjiakou.aliyuncs.com/57e113a4e7084f0bb78d4338efa47825.png)


- 加载原则：检查某个类是否已经加载顺序是自底向上，从Custom ClassLoader到BootStrap ClassLoader逐层检查，只要某个Classloader已加载，就视为已加载此类，保证此类只所有ClassLoader加载一次。 加载的顺序:加载的顺序是自顶向下，也就是由上层来逐层尝试加载此类。
- 双亲委派机制：
1. 如果一个类加载器在接到加载类的请求时，它首先不会自己尝试去加载这个类，而是把 这个请求任务委托给父类加载器去完成，依次递归，如果父类加载器可以完成类加载任务，就成功返回;只有父类加载器无法完成此加载任务时，才自己去加载。

2. 优势:Java类随着加载它的类加载器一起具备了一种带有优先级的层次关系。比如，Java中的 Object类，它存放在rt.jar之中,无论哪一个类加载器要加载这个类，最终都是委派给处于模型 最顶端的启动类加载器进行加载，因此Object在各种类加载环境中都是同一个类。如果不采用 双亲委派模型，那么由各个类加载器自己取加载的话，那么系统中会存在多种不同的Object 类。
3. 破坏:可以继承ClassLoader类，然后重写其中的loadClass方法，其他方式大家可以自己了解拓展一下。



## 运行时数据区
 在装载阶段的第(2),(3)步可以发现有运行时数据，堆，方法区等名词
(2)将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构

 (3)在Java堆中生成一个代表这个类的java.lang.Class对象，作为对方法区中这些数据的访问入口

说白了就是类文件被类装载器装载进来之后，类中的内容(比如变量，常量，方法，对象等这些数 据得要有个去处，也就是要存储起来，存储的位置肯定是在JVM中有对应的空间)


![image](http://java-run-blog.oss-cn-zhangjiakou.aliyuncs.com/f988e07c200c4aac9dac13d11f90cb85.png
)


1. Method Area(方法区)

    用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。
当方法区无法满足内存分配需求时，将抛出OutOfMemoryError异常。

2. Heap(堆)

    Java堆是Java虚拟机所管理内存中最大的一块，在虚拟机启动时创建，被所有线程共享。
Java对象实例以及数组都在堆上分配。

3. Java Virtual Machine Stacks(虚拟机栈)

    经过上面的分析，类加载机制的装载过程已经完成，后续的链接，初始化也会相应的生效。
    
    假如目前的阶段是初始化完成了，后续做啥呢?肯定是Use使用咯，不用的话这样折腾来折腾去 有什么意义?那怎样才能被使用到?换句话说里面内容怎样才能被执行?比如通过主函数main调 用其他方法，这种方式实际上是main线程执行之后调用的方法，即要想使用里面的各种内容，得 要以线程为单位，执行相应的方法才行。
    
    那一个线程执行的状态如何维护?一个线程可以执行多少个方法?这样的关系怎么维护呢?
    
    虚拟机栈是一个线程执行的区域，保存着一个线程中方法的调用状态。换句话说，一个Java线程的运行 状态，由一个虚拟机栈来保存，所以虚拟机栈肯定是线程私有的，独有的，随着线程的创建而创建。
    
    每一个被线程执行的方法，为该栈中的栈帧，即每个方法对应一个栈帧。 调用一个方法，就会向栈中压入一个栈帧;一个方法调用完成，就会把该栈帧从栈中弹出。

    ![image](http://java-run-blog.oss-cn-zhangjiakou.aliyuncs.com/a3fb6eb8d2d249db9670b6c577d0fcf8.png
)
4. The pc Register(程序计数器)

    我们都知道一个JVM进程中有多个线程在执行，而线程中的内容是否能够拥有执行权，是根据 CPU调度来的。
假如线程A正在执行到某个地方，突然失去了CPU的执行权，切换到线程B了，然后当线程A再获 得CPU执行权的时候，怎么能继续执行呢?这就是需要在线程中维护一个变量，记录线程执行到 的位置。

    程序计数器占用的内存空间很小，由于Java虚拟机的多线程是通过线程轮流切换，并分配处理器执行时 间的方式来实现的，在任意时刻，一个处理器只会执行一条线程中的指令。因此，为了线程切换后能够 恢复到正确的执行位置，每条线程需要有一个独立的程序计数器(线程私有)。
如果线程正在执行Java方法，则计数器记录的是正在执行的虚拟机字节码指令的地址; 如果正在执行的是Native方法，则这个计数器为空。

5. Native Method Stacks(本地方法栈) 

    如果当前线程执行的方法是Native类型的，这些方法就会在本地方法栈中执行。