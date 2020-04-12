静态代理:

静态代理不需要接口，只是一个代理对象拿到了被代理对象的引用，有代理对象调用被代理对象而已。


```
public class Father {

    private Son son;
    
    public Father(Son son) {
        this.son=son;
    }

    public void findSonLove(){
        System.out.println("代理前-----------");
        son.findLove();
        System.out.println("被代理后---------");
    }
}


public class Son {

    public void findLove(){
        System.out.println("son 被代理");
    }
}

public class StaticTest {

    public static void main(String[] args){
            new Father(new Son()).findSonLove();
    }
}


```

动态代理：JKD和CGLIB俩种方式
    
JDK动态代理：
```
public interface  Person {
    void findLove();
    void eat();
}


public class MeiPoProxy implements InvocationHandler {

    private Person person;

    public Object getInstance(Person person){
        this.person=person;
        Class<? extends Person> aClass = person.getClass();
        return Proxy.newProxyInstance(ClassLoader.getSystemClassLoader(), aClass.getInterfaces(), this);
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("被代理前----------------");
        method.invoke(this.person, args);
        System.out.println("被代理后--------------");

        return null;
    }
}

public class SomeBody implements Person {


    @Override
    public void findLove() {
        System.out.println("some body 被代理");

    }
    @Override
    public void eat() {

    }
}


public class JDKProxyTest {

    public static void main(String[] args) {
        try {
            Person obj = (Person)new JDK58().getInstance(new XieMu());
            System.out.println(obj.getClass());
            obj.findJob();
            //原理：
            //1、拿到被代理对象的引用，并且获取到它的所有的接口，反射获取
            //2、JDK Proxy类重新生成一个新的类、同时新的类要实现被代理类所有实现的所有的接口
            //3、动态生成Java代码，把新加的业务逻辑方法由一定的逻辑代码去调用（在代码中体现）
            //4、编译新生成的Java代码.class
            //5、再重新加载到JVM中运行
            //以上这个过程就叫字节码重组

            //JDK中有个规范，只要要是$开头的一般都是自动生成的

            //通过反编译工具可以查看源代码
            byte [] bytes = ProxyGenerator.generateProxyClass("$Proxy0",new Class[]{Person.class});
            FileOutputStream os = new FileOutputStream("E://$Proxy0.class");
            os.write(bytes);
            os.close();

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

}

```


CGLIB动态代理:



```
public class SomeBody implements Person {

    @Override
    public void findLove() {
        System.out.println("some body 被代理");

    }
    @Override
    public void eat() {

    }
}



public class CglibProxy implements MethodInterceptor {

    public Object getInstance(Class clazz){
        Enhancer enhancer=new Enhancer();
        enhancer.setSuperclass(clazz);
        enhancer.setCallback(this);
        return enhancer.create();

    }

    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        System.out.println("被代理前------------");
        methodProxy.invokeSuper(o, objects);
        System.out.println("被代理后------------");
        return null;
    }

}

public class CglibTest {

    public static void main(String[] args){
        CglibProxy cglibProxy=new CglibProxy();
        Person instance =(Person) cglibProxy.getInstance(SomeBody.class);
        instance.findLove();
    }
}


```


自己动手实现动态代理：
    动态代理之所以是动态的，是因为代理之前，也不知道代理的是什么，只有在代码运行时才知道，所以就需要动态的生成中间代理，然后进行代理。
    
实现方式:
1. 动态生成源代码.java文件。
2. JAVA文件输出到磁盘。
3. 把自己生成的java文件编辑成class文件。
4. 将编译的class文件加载到JVM中。
5. 返回的字节码重组成新的对象。


```
public class CustomerMeiPoProxy implements HuaIncationHandle {


    private Person person;

    public Object getInstance(Person person){
        this.person=person;
        Class<? extends Person> aClass = person.getClass();
        return HuaProxy.newProxyInstance(new HuaClassLoader(), aClass.getInterfaces(), this);
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("被代理前-------------------");

        method.invoke(person, args);
        System.out.println("被代理后-------------------");
        return null;
    }
}


public class HuaClassLoader  extends ClassLoader{

    private File classPathFile;

    public HuaClassLoader() {
        String classPath = HuaClassLoader.class.getResource("").getPath();
        this.classPathFile = new File(classPath);
    }

    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        String className = HuaClassLoader.class.getPackage().getName() + "." + name;

        if (classPathFile != null){
            File classFile = new File(classPathFile,name.replaceAll("\\.","/") + ".class");
            if (classFile.exists()){

                FileInputStream inputStream = null;
                ByteArrayOutputStream outputStream=null;


                try {
                    inputStream=new FileInputStream(classFile);
                    outputStream=new ByteArrayOutputStream();
                    byte [] buff = new byte[1024];
                    int len;
                    while((len=inputStream.read(buff))!=-1){
                        outputStream.write(buff,0,len);
                    }
                    return  defineClass(className,outputStream.toByteArray(),0,outputStream.size());
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }else {

            return null;
        }
        return null;
    }
}

public interface HuaIncationHandle {

     Object invoke(Object proxy, Method method, Object[] args) throws Throwable;

}

public class HuaProxy {


    private static String ln="\r\n";

    public static Object newProxyInstance(HuaClassLoader loader,Class<?>[] interfaces, HuaIncationHandle h){

        //1.动态的生成java文件。
        try {
            String s = generaterStr(interfaces);

            //2.输出到磁盘上，
            String path = HuaProxy.class.getResource("").getPath();
            System.out.println(path+"------");
            File file = new File(path + "$Proxy0.java");

            FileWriter fileWriter = new FileWriter(file);
            fileWriter.write(s);
            fileWriter.flush();
            fileWriter.close();

            //3.编译成class文件。
            JavaCompiler systemJavaCompiler = ToolProvider.getSystemJavaCompiler();
            StandardJavaFileManager standardFileManager = systemJavaCompiler.getStandardFileManager(null, null, null);

            Iterable<? extends JavaFileObject> javaFileObjects = standardFileManager.getJavaFileObjects(file);
            JavaCompiler.CompilationTask task = systemJavaCompiler.getTask(null,standardFileManager,null,null,null,javaFileObjects);
            task.call();
            standardFileManager.close();

            //4.加载到JVM中。
            Class<?> proxyClass = loader.findClass("$Proxy0");
            Constructor c = proxyClass.getConstructor(HuaIncationHandle.class);
            //5.返回字节码重组以后的新的代理对象
            return c.newInstance(h);
        }catch (Exception e){
            e.printStackTrace();
        }
        return null;
    }

    private static String generaterStr(Class<?>[] interfaces) {
        StringBuffer sb=new StringBuffer();

        sb.append("package com.gpxy.proxy.custome;"+ln);
        sb.append("import com.gpxy.proxy.Person;" + ln);
        sb.append("import java.lang.reflect.Method;" + ln);

        sb.append("public class $Proxy0 implements "+interfaces[0].getName()+"{"+ln );

            sb.append(" HuaIncationHandle  h; "+ln);

            sb.append(" public $Proxy0 (HuaIncationHandle h){"+ln);
                    sb.append("this.h=h;"+ln);
            sb.append("}"+ln);

        for ( Method method :interfaces[0].getMethods()){
                sb.append(" public "+method.getReturnType().getName()+" "+method.getName()+"(){" +ln);
                                sb.append("try{"+ln);
                                     sb.append("Method m = " + interfaces[0].getName() + ".class.getMethod(\"" + method.getName() + "\",new Class[]{});" + ln);
                                     sb.append("this.h.invoke(this,m,null);" + ln);
                                sb.append("}catch(Throwable e){"+ln);
                                     sb.append("e.printStackTrace();" + ln);
                                sb.append("}"+ln);
                sb.append("}"+ln);
             }
        sb.append("}"+ln);

        return sb.toString();
    }
}

public class SomeBody implements Person {


    @Override
    public void findLove() {
        System.out.println("some body 被代理");

    }
    @Override
    public void eat() {

    }
}

public class CustomerProxyTest {
    public static void main(String[] args){
        CustomerMeiPoProxy customerMeiPoProxy=new CustomerMeiPoProxy();
        Person instance =(Person) customerMeiPoProxy.getInstance(new SomeBody());
        instance.findLove();
    }
}
```
    
    
    