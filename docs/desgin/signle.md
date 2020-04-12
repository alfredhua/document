- 掌握单例模式的应用场景。
- 掌握IDEA环境下的多线程调试方式。
- 掌握保证线程安全的单例模式策略。
- 掌握反射暴力攻击单例解决方案及原理分析。
- 序列化破坏单例的原理及解决方案。
- 掌握常见的单例模式写法。
- 掌握原型模式的应用场景及常用写法。

任何情况下只有一个实例，提供全局一个访问点。

ServletContext、ServletConfig、BeanFactory、ApplicationContext、DBPool

饿汉式单例：
 > 在初始话的时候直接new出来了，不需要在调用的时候去new，这样就避免了线程安全的问题。

优点：1.线程绝对安全。2.执行效率高。在类加载的时候就初始化了。

缺点：1.浪费类型空间，占用内存。占着空间，浪费资源。

```java
public class Hungry {

    private Hungry(){}
    private static final Hungry hungry = new Hungry();

    public static Hungry getInstance(){
        return  hungry;
    }

}
```

懒汉式：
    在需要的时候才会去创建。
    
    优点：1.占用空间小
    
    缺点：1.需要自己解决线程安全问题。
  特点：在外部类被调用的时候内部类才会被加载内部类一定是要在方法调用之前初始化巧妙地避免了线程安全问题
   这种形式兼顾饿汉式的内存浪费，也兼顾synchronized性能问题，完美地屏蔽了这两个缺点。
   史上最牛B的单例模式的实现方式
```java

public class LazyOne{
    
    private static LazyOne lazyOne=null;
    private LazyOne(){}
    
    public static LazyOne getInstance(){
        //线程不安全
        if(lazyOne==null){
            lazyOne=new LazyOne();
        }
        return lazyOne;
    }
}



public class LazyTwo{
    
    private static LazyTwo lazyTwo=null;
    private LazyTwo(){}
    
    public static synchronized LazyTwo getInstance(){
        //线程安全，执行效率低
        if(lazyTwo==null){
            lazyTwo=new LazyTwo;
        }
        return lazyTwo;
    }
}


public class LazyThree{
    
    private static LazyThree lazythree=null;
    private LazyThree(){}
    
    public static  LazyThree getInstance(){
        //线程安全，执行效率可以
       if(lazythree==null){
            synchronized(LazyThree.class){
                if(lazythree==null){
                  lazythree=new LazyThree();
                }
            }
        }
        return lazythree;
    } 
}



public class LazyFour{
    
    private static LazyFour lazyFour=null;
    private LazyFour(){
        //防止反射入侵单例
        if(lazyFour!=null){
            throw new RunTimeException("单例被入侵");
        }
        
    }
    
    public static  LazyTwo getInstance(){
        lazyFour=LazyHandle.lazyFour;
    } 
    
    private static class LazyHandle{
        static LazyFour lazyFour=new LazyFour();
    }
    
}

```

注册试：


```java
public class ReginsterSingle {

    //HashMap 变成ConcurrentHashmap就是线程安全的
    private Map<String,Object> registerMap=new HashMap<>();

    private ReginsterSingle(){}

    public ReginsterSingle getInstance(String name){

        if(name!=null){
            name = ReginsterSingle.class.getName();
        }
        if (registerMap.get(name)==null){
            registerMap.put(name, new ReginsterSingle());
        }
        return (ReginsterSingle)registerMap.get(name);
    }

}
//枚举式
public enum RegiterEnum {
    INSTANCE,BLACK,WHITE;
    public void getInstance(){}
}

```


序列化和反序列化：
1. 把对象转换为字节序列的过程称为对象的序列化。
1. 把字节序列恢复为对象的过程称为对象的反序列化。
　　


对象的序列化主要有两种用途：
1. 把对象的字节序列永久地保存到硬盘上，通常存放在一个文件中。
1. 在网络上传送对象的字节序列;


实现readResolve方法可以防止反序列化。
```
   public  final static Seriable INSTANCE = new Seriable();
    private Seriable(){}

    public static  Seriable getInstance(){
        return INSTANCE;
    }

    private  Object readResolve(){
        return  INSTANCE;
    }
```
