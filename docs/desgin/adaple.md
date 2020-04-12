委派模式：

委派模式注重的是结果：

核心是：就是分发、调度、派遣

BOSS ----> leader ---->target

```
public interface ITarget {

     void doing(String com);
}

public class TargetA implements ITarget {

    @Override
    public void doing(String com) {
        System.out.println("我是员工A，我在做"+com+"工作");
    }
}

public class TargetB implements ITarget {

    @Override
    public void doing(String com) {
        System.out.println("我是员工B，我在做"+com+"工作");
    }
}

public class Leader {

    private Map<String,ITarget> doMap=new HashMap<>();

    public Leader() {
        doMap.put("加密", new TargetA());
        doMap.put("登录", new TargetB());
    }
    //自己不干活，让别人干，自己负责选择让谁去干。
    public void doing(String com){
        doMap.get(com).doing(com);

    }

}

public class Boss {

    //客户请求（Boss）、委派者（Leader）、被被委派者（Target）
    //委派者要持有被委派者的引用
    //代理模式注重的是过程， 委派模式注重的是结果
    //策略模式注重是可扩展（外部扩展），委派模式注重内部的灵活和复用
    //委派的核心：就是分发、调度、派遣

    //委派模式：就是静态代理和策略模式一种特殊的组合
    public static void main(String[] args){
        new Leader().doing("加密");
    }

}


```

适配器模式：

注重的是兼容。稳定的代码不去修改，直接继承下来。


```
public interface SignService {

    void sign();

    void login();
}

public class SignServiceImpl implements SignService {
    @Override
    public void sign() {
        System.out.println("注册");
    }

    @Override
    public void login() {
        System.out.println("登录");
    }
}

public class QQSignService extends SignServiceImpl {

    public void qqSign(){
        //qq注册
        super.sign();
    }
}

public class SignTest {

    public static void main(String[] args){
        QQSignService qqSignService = new QQSignService();
        qqSignService.qqSign();
    }
}
```