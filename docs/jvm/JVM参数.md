官网：https://docs.oracle.com/javase/8/docs/technotes/tools/unix/java.html#BGBCIEFC

1. 标准参数

   如： -version，-help，-server，-cp

2. -X参数
   
   非标准参数，也就是在JDK各个版本中可能会变动。

     如：
      - -Xint 解释执行
      - -Xcomp 第一次使用就编译成本地代码 
      - -Xmixed 混合模式，JVM自己来决定
![image](https://java-run-blog.oss-cn-zhangjiakou.aliyuncs.com/file/2b093b679b8347ea880e4f419a6f09e8)

3、-XX参数

    使用得最多的参数类型

    非标准化参数，相对不稳定，主要用于JVM调优和Debug

    1. Boolean类型
    格式:-XX:[+-]<name> +或-表示启用或者禁用name属性 
    比如:-XX:+UseConcMarkSweepGC 表示启用CMS类型的垃圾回收器
    -XX:+UseG1GC 表示启用G1类型的垃圾回收器

    2. 非Boolean类型 
    格式:-XX<name>=<value>表示name属性的值是value。
    比如:-XX:MaxGCPauseMillis=500。

4、其他参数

    -Xms1000等价于-XX:InitialHeapSize=1000
    -Xmx1000等价于-XX:MaxHeapSize=1000
    -Xss100等价于-XX:ThreadStackSize=100

## 设置参数

    运行jar包的时候:java -XX:+UseG1GC xxx.jar
    web容器比如tomcat，可以在脚本中的进行设置 通过jinfo实时调整某个java进程的参数(参数只有被标记为manageable的flags可以被实时修改)

## 常用参数含义

![image](http://java-run-blog.oss-cn-zhangjiakou.aliyuncs.com/file/d4f64979d0df4327ab15e62371d80056)

![image](http://java-run-blog.oss-cn-zhangjiakou.aliyuncs.com/file/6355fb86aac74a168cba867ce08083d5)

![image](http://java-run-blog.oss-cn-zhangjiakou.aliyuncs.com/file/9d98301ea5f04fcaafe1778ae4f90649)