## 什么是 Spring 框架

​	Spring 官网：https://spring.io/。

​	它是很多模块的集合，使用这些模块可以很方便地协助我们进行开发。这些模块是：核心容器、数据访问/集成,、Web、AOP（面向切面编程）、工具、消息和测试模块。比如：Core Container 中的 Core 组件是Spring 所有组件的核心，Beans 组件和 Context 组件是实现IOC和依赖注入的基础，AOP组件用来实现面向切面编程。

- **核心技术** ：依赖注入(DI)，AOP，事件(events)，资源，i18n，验证，数据绑定，类型转换，SpEL。
- **测试** ：模拟对象，TestContext框架，Spring MVC 测试，WebTestClient。
- **数据访问** ：事务，DAO支持，JDBC，ORM，编组XML。
- **Web支持** : Spring MVC和Spring WebFlux Web框架。
- **集成** ：远程处理，JMS，JCA，JMX，电子邮件，任务，调度，缓存。
- **语言** ：Kotlin，Groovy，动态语言

## **Spring 的优点？**

- spring属于低侵入式设计，代码的污染极低；
- spring的DI机制将对象之间的依赖关系交由框架处理，减低组件的耦合性；
- Spring提供了AOP技术，支持将一些通用任务，如安全、事务、日志、权限等进行集中式管理，从而提供更好的复用。
- spring对于主流的应用框架提供了集成支持。
  
  

## 列举一些重要的Spring模块？

- **Spring Core：** 基础,可以说 Spring 其他所有的功能都需要依赖于该类库。主要提供 IoC 依赖注入功能。
- **Spring Aspects** ： 该模块为与AspectJ的集成提供支持。
- **Spring AOP** ：提供了面向切面的编程实现。
- **Spring JDBC** : Java数据库连接。
- **Spring JMS** ：Java消息服务。
- **Spring ORM** : 用于支持Hibernate等ORM工具。
- **Spring Web** : 为创建Web应用程序提供支持。
- **Spring Test** : 提供了对 JUnit 和 TestNG 测试的支持。

## @RestController vs @Controller

​	单独使用 `@Controller` 不加 `@ResponseBody`的话一般使用在要返回一个视图的情况，这种情况属于比较传统的Spring MVC 的应用，对应于前后端不分离的情况。

 	@RestController =@Controller +@ResponseBody

## 谈谈自己对于 Spring IoC 和 AOP 的理解

- IOC

  ​	IoC（Inverse of Control:控制反转）是一种**设计思想**，就是 **将原本在程序中手动创建对象的控制权，交由Spring框架来管理。** IoC 在其他语言中也有应用，并非 Spring 特有。 **IoC 容器是 Spring 用来实现 IoC 的载体， IoC 容器实际上就是个Map（key，value）,Map 中存放的是各种对象。**

  ​	将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。 **IoC 容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的。** 在实际项目中一个 Service 类可能有几百甚至上千个类作为它的底层，假如我们需要实例化这个 Service，你可能要每次都要搞清这个 Service 所有底层类的构造函数，这可能会把人逼疯。如果利用 IoC 的话，你只需要配置好，然后在需要的地方引用就行了，这大大增加了项目的可维护性且降低了开发难度。

- AOP

  ​		AOP(Aspect-Oriented Programming:面向切面编程)能够将那些与业务无关，**却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来**，便于**减少系统的重复代码**，**降低模块间的耦合度**，并**有利于未来的可拓展性和可维护性**。

  ​		**Spring AOP就是基于动态代理的**，如果要代理的对象，实现了某个接口，那么Spring AOP会使用**JDK Proxy**，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候Spring AOP会使用**Cglib** ，这时候Spring AOP会使用 **Cglib** 生成一个被代理对象的子类来作为代理

## Spring AOP 和 AspectJ AOP 有什么区别？

​	Spring AOP 属于运行时增强，而 AspectJ 是编译时增强。Spring AOP 基于代理(Proxying)，而 AspectJ 基于字节码操作(Bytecode Manipulation)。

​	Spring AOP 已经集成了 AspectJ ，AspectJ 应该算的上是 Java 生态系统中最完整的 AOP 框架了。AspectJ 相比于 Spring AOP 功能更加强大，但是 Spring AOP 相对来说更简单，

​	如果我们的切面比较少，那么两者性能差异不大。但是，当切面太多的话，最好选择 AspectJ ，它比Spring AOP 快很多。

## Filter和拦截器区别

### Filter：

- 过滤器拦截web访问url地址。严格意义上讲，filter只是适用于web中，依赖于Servlet容器，利用Java的回调机制进行实现。
- Filter过滤器：和框架无关，可以控制最初的http请求，但是更细一点的类和方法控制不了。
- **过滤器可以拦截到方法的请求和响应(ServletRequest request, ServletResponse response)**，并对请求响应做出像响应的过滤操作，
- 比如**设置字符编码，鉴权操作**等

### 拦截器：

- ***\*拦截器拦截以 .action结尾的url，拦截Action的访问\****。 Interfactor是基于**Java的反射机制**（APO思想）进行实现，不依赖Servlet容器。
- **拦截器可以在方法执行之前(preHandle)和方法执行之后(afterCompletion)进行操作，回调操作(postHandle)**，***\*可以获取执行的方法的名称\****，请求(HttpServletRequest)
- Interceptor：**可以控制请求的控制器和方法**，但**控制不了请求方法里的参数(\**只能获取参数的名称，不能获取到参数的值\**)**
- **（**用于处理页面提交的请求响应并进行处理，例如做国际化，做主题更换，过滤等）。

### 

## Spring 中的 bean 的作用域有哪些?

- singleton : 唯一 bean 实例，Spring 中的 bean 默认都是单例的。
- prototype : 每次请求都会创建一个新的 bean 实例。
- request : 每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP request内有效。
- session : 每一次HTTP请求都会产生一个新的 bean，该bean仅在当前 HTTP session 内有效。
- global-session： 全局session作用域，仅仅在基于portlet的web应用中才有意义，Spring5已经没有了。Portlet是能够生成语义代码(例如：HTML)片段的小型Java Web插件。它们基于portlet容器，可以像servlet一样处理HTTP请求。但是，与 servlet 不同，每个 portlet 都有不同的会话。

## Spring中的Bean是线程安全的嘛？

​	对于单例Bean,所有线程都共享一个单例实例Bean,因此是存在资源的竞争。

​	如果单例Bean,是一个无状态Bean，也就是线程中的操作不会对Bean的成员执行查询以外的操作，那么这个单例Bean是线程安全的。比如Spring mvc 的 Controller、Service、Dao等，这些Bean大多是无状态的，只关注于方法本身。

​	对于有状态的bean，Spring官方提供的bean，一般提供了通过ThreadLocal去解决线程安全的方法，比如RequestContextHolder、TransactionSynchronizationManager、LocaleContextHolder等。	

​	容器本身并没有提供Bean的线程安全策略，因此可以说**Spring容器中的Bean本身不具备线程安全的特性**。**因此是否线程安全完全取决于Bean本身的特性。**

## @Component 和 @Bean 的区别是什么？

1. 作用对象不同: `@Component` 注解作用于类，而`@Bean`注解作用于方法。
2. `@Component`通常是通过类路径扫描来自动侦测以及自动装配到Spring容器中（我们可以使用 `@ComponentScan` 注解定义要扫描的路径从中找出标识了需要装配的类自动装配到 Spring 的 bean 容器中）。`@Bean` 注解通常是我们在标有该注解的方法中定义产生这个 bean,`@Bean`告诉了Spring这是某个类的示例，当我需要用它的时候还给我。
3. `@Bean` 注解比 `Component` 注解的自定义性更强，而且很多地方我们只能通过 `@Bean` 注解来注册bean。比如当我们引用第三方库中的类需要装配到 `Spring`容器时，则只能通过 `@Bean`来实现。

## 将一个类声明为Spring的 bean 的注解有哪些?

我们一般使用 `@Autowired` 注解自动装配 bean，要想把类标识成可用于 `@Autowired` 注解自动装配的 bean 的类,采用以下注解可实现：

- `@Component` ：通用的注解，可标注任意类为 `Spring` 组件。如果一个Bean不知道属于哪个层，可以使用`@Component` 注解标注。
- `@Repository` : 对应持久层即 Dao 层，主要用于数据库相关操作。
- `@Service` : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao层。
- `@Controller` : 对应 Spring MVC 控制层，主要用户接受用户请求并调用 Service 层返回数据给前端页面。

## Spring 中的 bean 生命周期?

- Bean 容器找到配置文件中 Spring Bean 的定义。
- Bean 容器利用 Java Reflection API 创建一个Bean的实例。
- 如果涉及到一些属性值 利用 `set()`方法设置一些属性值。
- 如果 Bean 实现了 `BeanNameAware` 接口，调用 `setBeanName()`方法，传入Bean的名字。
- 如果 Bean 实现了 `BeanClassLoaderAware` 接口，调用 `setBeanClassLoader()`方法，传入 `ClassLoader`对象的实例。
- 与上面的类似，如果实现了其他 `*.Aware`接口，就调用相应的方法。
- 如果有和加载这个 Bean 的 Spring 容器相关的 `BeanPostProcessor` 对象，执行`postProcessBeforeInitialization()` 方法
- 如果Bean实现了`InitializingBean`接口，执行`afterPropertiesSet()`方法。
- 如果 Bean 在配置文件中的定义包含 init-method 属性，执行指定的方法。
- 如果有和加载这个 Bean的 Spring 容器相关的 `BeanPostProcessor` 对象，执行`postProcessAfterInitialization()` 方法
- 当要销毁 Bean 的时候，如果 Bean 实现了 `DisposableBean` 接口，执行 `destroy()` 方法。
- 当要销毁 Bean 的时候，如果 Bean 在配置文件中的定义包含 destroy-method 属性，执行指定的方法。

## SpringMVC 工作原理

1. 客户端（浏览器）发送请求，直接请求到 `DispatcherServlet`。
2. `DispatcherServlet` 根据请求信息调用 `HandlerMapping`，解析请求对应的 `Handler`。
3. 解析到对应的 `Handler`（也就是我们平常说的 `Controller` 控制器）后，开始由 `HandlerAdapter` 适配器处理。
4. `HandlerAdapter` 会根据 `Handler `来调用真正的处理器开处理请求，并处理相应的业务逻辑。
5. 处理器处理完业务后，会返回一个 `ModelAndView` 对象，`Model` 是返回的数据对象，`View` 是个逻辑上的 `View`。
6. `ViewResolver` 会根据逻辑 `View` 查找实际的 `View`。
7. `DispaterServlet` 把返回的 `Model` 传给 `View`（视图渲染）。
8. 把 `View` 返回给请求者（浏览器）

## Spring 管理事务的方式有几种？

1. 编程式事务，在代码中硬编码。(不推荐使用)
2. 声明式事务，在配置文件中配置（推荐使用）

**声明式事务又分为两种：**

1. 基于XML的声明式事务
2. 基于注解的声明式事务

## Spring 事务中的隔离级别有哪几种?

- **TransactionDefinition.ISOLATION_DEFAULT:** 使用后端数据库默认的隔离级别，Mysql 默认采用的 REPEATABLE_READ隔离级别，可重复读。Oracle 默认采用的 READ_COMMITTED隔离级别.
- **TransactionDefinition.ISOLATION_READ_UNCOMMITTED:** 最低的隔离级别，允许读取尚未提交的数据变更，**可能会导致脏读、幻读或不可重复读**
- **TransactionDefinition.ISOLATION_READ_COMMITTED:** 允许读取并发事务已经提交的数据，**可以阻止脏读，但是幻读或不可重复读仍有可能发生**
- **TransactionDefinition.ISOLATION_REPEATABLE_READ:** 对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，**可以阻止脏读和不可重复读，但幻读仍有可能发生。**
- **TransactionDefinition.ISOLATION_SERIALIZABLE:** 最高的隔离级别，完全服从ACID的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，**该级别可以防止脏读、不可重复读以及幻读**。但是这将严重影响程序的性能。通常情况下也不会用到该级别。

## Spring 事务中哪几种事务传播行为?

**支持当前事务的情况：**

- **TransactionDefinition.PROPAGATION_REQUIRED：** 如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。
- **TransactionDefinition.PROPAGATION_SUPPORTS：** 如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。
- **TransactionDefinition.PROPAGATION_MANDATORY：** 如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。（mandatory：强制性）

**不支持当前事务的情况：**

- **TransactionDefinition.PROPAGATION_REQUIRES_NEW：** 创建一个新的事务，如果当前存在事务，则把当前事务挂起。
- **TransactionDefinition.PROPAGATION_NOT_SUPPORTED：** 以非事务方式运行，如果当前存在事务，则把当前事务挂起。
- **TransactionDefinition.PROPAGATION_NEVER：** 以非事务方式运行，如果当前存在事务，则抛出异常。

**其他情况：**

- **TransactionDefinition.PROPAGATION_NESTED：** 如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于TransactionDefinition.PROPAGATION_REQUIRED。

## BeanFactory和ApplicationContext有什么区别？

​    BeanFactory和ApplicationContext是Spring的两大核心接口，都可以当做Spring的容器。其中ApplicationContext是BeanFactory的子接口。

1. BeanFactory：是Spring里面最底层的接口，包含了各种Bean的定义，读取bean配置文档，管理bean的加载、实例化，控制bean的生命周期，维护bean之间的依赖关系。ApplicationContext接口作为BeanFactory的派生，除了提供BeanFactory所具有的功能外，还提供了更完整的框架功能：

​	①继承MessageSource，因此支持国际化。

​	②统一的资源文件访问方式。

​	③提供在监听器中注册bean的事件。

​	④同时加载多个配置文件。

​	⑤载入多个（有继承关系）上下文 ，使得每一个上下文都专注于一个特定的层次，比如应用的web层。

2. ①BeanFactroy采用的是延迟加载形式来注入Bean的，即只有在使用到某个Bean时(调用getBean())，才对该Bean进行加载实例化。这样，我们就不能发现一些存在的Spring的配置问题。如果Bean的某一个属性没有注入，BeanFacotry加载后，直至第一次使用调用getBean方法才会抛出异常。

    ②ApplicationContext，它是在容器启动时，一次性创建了所有的Bean。这样，在容器启动时，我们就可以发现Spring中存在的配置错误，这样有利于检查所依赖属性是否注入。 ApplicationContext启动后预载入所有的单实例Bean，通过预载入单实例bean ,确保当你需要的时候，你就不用等待，因为它们已经创建好了。

    ③相对于基本的BeanFactory，ApplicationContext 唯一的不足是占用内存空间。当应用程序配置Bean较多时，程序启动较慢。

3. BeanFactory通常以编程的方式被创建，ApplicationContext还能以声明的方式创建，如使用ContextLoader。

4. BeanFactory和ApplicationContext都支持BeanPostProcessor、BeanFactoryPostProcessor的使用，但两者之间的区别是：BeanFactory需要手动注册，而ApplicationContext则是自动注册。

```java
<!-- 将CellPhone部署成prototype的范围 -->
<bean id="cellPhone" class="com.abc.CellPhone" scope="prototype" />
<bean id="developer" class="com.abc.Developer">
    <!-- getPhone方法返回CellPhone，每次调用将获取新的CellPhone -->
    <lookup-method name="getPhone" bean="cellPhone" />
</bean>
  
  public class CellPhone implements Phone {
    public CellPhone() {
        System.out.println("Spring实例化依赖的Bean...CellPhone实例");
    }
    public return call() {
        return "正在打电话...";
    }
}

  public abstract class Developer implements Person {
    public Developer() {
        System.out.println("Spring实例化主调的Bean...Developer实例");
    }
    //定义一个抽象方法，该方法将由Spring实现
    public abstract Phone getPhone();
    @Override
    public void call() {
        System.out.println("正在使用 " + getPhone() + " 打电话");
        System.out.println(getPhone().call());
    }
}

  public class Test {
    public static void main(String args[]) {
        ApplicationContext context = 
                new ClassPathXmlApplicationContext("applicationContext.xml");
        Developer d = context.getBean("developer", Developer.class);
        d.call();
        d.call();
    }
}

```
## 在applicationgContext.xml文件中定义了一个bean，id为authService，通过ApplicationContext实例对象的getBean方法获取到这个bean，这个背后的实现原理是什么？

Spring容器启动的时候会解析applicationgContext.xml，将xml中定义的bean(如authService)解析成Spring内部的BeanDefinition，并以beanName(如authService)为key，BeanDefinition(如authService相应的BeanDefinition)为value存储到DefaultListableBeanFactory中的beanDefinitionMap属性中(其实它就是一个ConcurrentHashMap类型的属性)，同时将beanName存入beanDefinitionNames中(List类型)，然后遍历beanDefinitionNames中的beanName，进行bean的实例化并填充属性，在实例化的过程中，如果有依赖没有被实例化将先实例化其依赖，然后实例化本身，实例化完成后将实例存入单例bean的缓存中，当调用getBean方法时，到单例bean的缓存中查找，如果找到并经过转换后返回这个实例(如AuthService的实例)，之后就可以直接使用了。



## 说一下xml文件的解析过程？

代码中指定要加载的xml文件后，Spring容器初始化的过程中，通过ResourceLoader接口实现类，例如ClassPathXmlApplicationContext，将xml文件路径转换成对应的Resource文件，例如ClassPathResource，然后通过DocumentLoader对Resource文件进行转换，转换成Document文件，接着通过DefaultBeanDefinitionDocumentReader对Document进行解析，并使用BeanDefinitionParserDelegate对元素进行解析，解析xml中bean定义的各个元素，存入BeanDefinition中。



## 那你再详细说一下这个BeanDefinition是什么？

一个对象的生命周期要想被Spring容器管理，那么它的类信息必须先转成Spring内部的数据结构，BeanDefinition就是Spring框架内部用来描述对象的类信息的数据结构。例如类名、scope、属性、构造函数参数列表、依赖的bean、是否是单例类、是否是懒加载等，其实就是将Bean的定义信息存储到这个BeanDefinition相应的属性中，后面对Bean的操作就直接对BeanDefinition进行，例如拿到这个BeanDefinition后，可以根据里面的类名、构造函数、构造函数参数，使用反射进行对象创建。BeanDefinition是一个接口，是一个抽象的定义，实际使用的是其实现类，如ChildBeanDefinition、RootBeanDefinition、GenericBeanDefinition等。BeanDefinition继承了AttributeAccessor，说明它具有处理属性的能力；BeanDefinition继承了BeanMetadataElement，说明它可以持有Bean元数据元素，作用是可以持有XML文件的一个bean标签对应的Object。



## 刚刚你有说到DefaultListableBeanFactory，它在Spring框架中的作用是什么？

DefaultListableBeanFactory是整个Bean加载的核心部分，是Spring注册及加载Bean的默认实现。DefaultListableBeanFactory间接实现了BeanFactory接口，而在BeanFactory接口中定义了很多和bean操作相关的方法，例如getBean、containsBean、isSingleton等，所以DefaultListableBeanFactory也相应持有了这些操作。



## 那BeanFactory又是什么？

BeanFactory是用于访问Spring Bean容器的根接口，是一个单纯的Bean工厂，也就是常说的ioc容器的顶层定义，各种ioc容器是在其基础上为了满足不同需求而扩展的，包括经常使用的ApplicationContext。



## 如何理解BeanFactory和FactoryBean？

BeanFactory定义了ioc容器的最基本形式，并提供了ioc容器应遵守的的最基本的接口，也就是Spring ioc所遵守的最底层和最基本的编程规范，它只是个接口，并不是ioc容器的具体实现。它的职责包括：实例化、定位、配置应用程序中的对象及建立这些对象间的依赖。再来说说FactoryBean，一般情况下，Spring通过反射机制利用bean的class属性实例化Bean，然而在某些情况下，实例化Bean过程比较复杂，如果按照传统的方式，则需要在bean的定义中提供大量的配置信息，而配置这种方式的灵活性是受限的，这时采用编码的方式可能会是一个比较合适的方案，Spring为此提供了FactoryBean的工厂类接口，用户可以通过实现该接口定制实例化Bean的逻辑。



## 如果想在初始化前修改bean的属性，如何实现？

自定义一个BeanFactoryPostProcessor，让它实现BeanFactoryPostProcessor接口，并实现postProcessBeanFactory方法，在这个方法中可以在初始化前修改bean的属性。



## 这个自定义的BeanFactoryPostProcessor是如何自动调用的？

在Spring容器初始化的过程中会自动触发，具体代码在AbstractApplicationContext类中会调用invokeBeanFactoryPostProcessors方法，在这个方法中筛选出所有实现BeanFactoryPostProcessor接口的类名称，然后遍历调用这些实现类的postProcessBeanFactory方法。



## 如果想在bean被初始化时进行拦截，进行额外初始化操作，如何实现？

自定义BeanPostProcessor，让它实现BeanPostProcessor接口，在这个接口中定义了两个方法：postProcessBeforeInitialization和postProcessAfterInitialization。postProcessBeforeInitialization方法会在afterPropertiesSet和自定义的初始化方法之前执行，通过实现这个方法，在方法的内部进行初始化之前的额外操作。postProcessAfterInitialization方法会在afterPropertiesSet和自定义的初始化方法之后执行，通过实现这个方法，在方法的内部进行初始化之后的额外操作。



## 在Spring容器初始化的过程中，所有定义的bean都会被初始化吗？

不是，默认只初始化所有未初始化的非懒加载的单例Bean，scope为其它值的bean会在使用到的时候进行初始化，如prototype。



## 有看过Spring中bean初始化的源码吗？

看过，单例bean的初始化，通过反射进行实例对象的创建，在进行属性填充时，如果依赖的对象没有创建，则先创建依赖对象，最后将bean实例加入单例bean实例的缓存中。



## 在bean实例化的过程中，Spring是如何解决循环依赖的？

Spring只对单例bean的循环依赖进行了解决，同时如果是通过构造函数注入造成的循环依赖，Spring也没有办法解决，只是抛出BeanCurrentlyInCreationException异常。如果是通过setter方式注入而产生的循环依赖，Spring在创建bean对象时，通过提前暴露一个ObjectFactory用来返回一个创建中的bean对象，从而使其它bean能够引用到这个bean。



## singleton中注入prototype ben的问题

spring实例化一个bean时会先实例话它依赖的bean，对于singleton，spring只会实例化一次， 但是在有些场景下，例如购物场景： 购物服务属于公共无状态服务，一般定义成一个singleton的bean，购物车则通常定义为prototype 而通常又会在购物服务中注入购物车实例，以便向购物车添加商品, 如果使用传统的 @Autoware方式注入购物车实例，那么多个访客可能就共用了同一个购物车实例，因为 购物服务是单例的，注入只会发生一次。 为了解决这个问题，

spring提供了两个方案：

**单例继承ApplicationContextAware**

继承了ApplicationContextAware的购物服务bean可以拿到当前的context，从而调用context.getBean()获取 一个新的购物车bean，解决了脏数据问题。 但是这种方式不够灵活，毕竟要实现一个接口，显式地耦合了spring框架。

类注解上PersonDao增加@Scope("prototype") 

放弃@Autowired

```java
public class PersonService  implements ApplicationContextAware {

private ApplicationContext applicationContext;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }

    protected PersonDao createPersonDao() {
        return this.applicationContext.getBean( PersonDao.class);
    }
}
```

这种方式是单例的依赖注入只会注入一次，所以不会生效，只有这样的方式才可以拿到的bean是多例的。