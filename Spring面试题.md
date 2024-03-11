# 一、Spring面试题

### 1.spring基础

#### **1.什么是spring框架？**

spring是一款开源的轻量级java开发框架，旨在提高开发人员的开发效率以及系统的可维护性。spring框架一般指的是spring Framework，是很多模块的集合，使用这些模块可以很方便地协助进行开发，比如spring支持IOC和AOP、可以很方便地对数据库进行访问、可以很方便集成第三方组件（电子邮件、任务、调度、缓存等等）、对单元测试支持比较好，支持RESTful java 应用程序的开发。

**spring的核心思想：开箱即用，提高开发效率**。

**2.spring包含哪些模块**

![image-20240306151925375](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240306151925375.png)

各个模块的依赖关系：

![image-20240306152037100](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240306152037100.png)

**Core Container**

spring框架的核心模块，只要提供IOC依赖注入功能的支持。spring其他所有的功能基本都需要依赖于该模块。

- spring-core：spring框架基本的核心工具类
- spring-beans:提供对bean的创建、配置和管理等功能的支持。
- spring-context：提供国际化、事件传播、资源加载等功能的支持。
- spring-expression：提供对表达式语言（spring Expression Language)，Spel的支持，值依赖core模块，不依赖其他模块，可以单独使用，

**AOP（Aspect-Oriented Programming)**

- spring-aspects:该模块为与AspectJ的集成提供支持。
- spring-aop:提供了面向切面的编程实现。
- spring-instrument：提供了JVM添加代理（agent）的功能。为Tomcat提供一个织入代理，能够为Tomcat传递类文件。

**Data Access/Integration**

- spring-jdbc:提供了对数据库访问的抽象JDBC。java只需要和JDBC API交互。
- spring-tx:提供对事务的支持。
- spring-orm:提供对Hibernate、JPA、IBatis等ORM框架。
- spring-oxm:提供一个抽象层支撑OXM（object-to-XML-Mapping)
- spring-jms:消息服务，对spring-message的继承。

**Spring Web**

- spring-web:对web功能的实现提供一些最基础的支持。
- spring-webmvc:对springmvc的实现。
- spring-websocket:提供了对websocket的支持，Websocket可以让客户端和服务端进行双向通信。
- spring-webflux:提供webFlux的支持。完全异步。

**Messaging**

spring-messaging 主要职责为spring框架集成一些基础的报文传送应用。

**Spring Test**

spring 团队提倡测试驱动开发（TDD）。有了控制反转（IOC）的帮助，单元测试和集成测试变得更简单。

spring的测试模块对Junit(单元测试框架)、TestNG（类似JUnit)、Mockito(主要用来Mock对象)、PowerMock(解决Mockito的问题)等常用测试框架支持。

**3.spring、spring MVC、springboot之间的关**系

Spring 包含了多个功能模块（上面刚刚提到过），其中最重要的是 Spring-Core（主要提供 IoC 依赖注入功能的支持） 模块， Spring 中的其他模块（比如 Spring MVC）的功能实现基本都需要依赖于该模块

Spring MVC 是 Spring 中的一个很重要的模块，主要赋予 Spring 快速构建 MVC 架构的 Web 程序的能力。MVC 是模型(Model)、视图(View)、控制器(Controller)的简写，其核心思想是通过将业务逻辑、数据、显示分离来组织代码。

Spring Boot 只是简化了配置，如果你需要构建 MVC 架构的 Web 程序，你还是需要使用 Spring MVC 作为 MVC 框架，只是说 Spring Boot 帮你简化了 Spring MVC 的很多配置，真正做到开箱即用！

### 2.springIOC

IOC（inversion of control：控制反转）是一种设计思想，是将原本在程序中手动创建对象的控制权，交由spring框架来管理。

- 控制：指的是对象创建（实例化、管理）的权力
- 反转：控制权交给外部环境（spring框架、IOC容器）

将对象之间的相互依赖关系交给IOC容器来管理，并由IOC容器完成对象的注入。简化了应用的开发，把应用从复杂的依赖关系中解放出来。IOC容器就像一个工厂，需要创建对象的时候，只需要配置好文件、注解即可。【在spring中，IOC容器是用来实现IOC的载体，IOC容器实际上就是一个Map(key,value),map中存放的是各种对象】

**spring Bean:Bean代指被IOC容器所管理的对象。**

#### 1、将一个类声明为Bean的注解有哪些？

- @Component:通用注解，可标注任意类为spring组件。如果一个Bean不知道属于哪个层，可以使用@Component注解标注。
- @Repository:对应持久层即Dao层，主要用于数据库相关操作。
- @Service:对应服务层，主要涉及一些复杂的逻辑，需要用到DAO层。
- @Controller:对应spring MVC控制层，主要用于接受用户请求并调用service层返回数据给前端页面。

#### 2.@Component 和 @Bean的区别是什么？

- @Component注解用作类，而@Bean注解用作方法。
- @Componet通常是通过类路径扫描来自动侦测以及自动装配到spring容器中。@Bean注解通常是我们在标有该注解的方法中定义产生这个Bean，告诉spring这个是某个类的实例，需要用得到的时候还回来。
- @Bean注解比@Component注解的自定义性更强，很多地方只能通过@Bean注解来注册bean。比如引用第三方库中的类需要装配spring容器时，只能通过Bean来实现。

#### 3.注入Bean的注解有哪些？

spring 内置的@Autowired以及JDK内置的@Resource和@Inject都可以用于注入Bean.但是@Autowired和@Resource使用的比较多一些。

4.@Autowired和@Resource的区别是：

- Autowired属于spring内置的注解，默认的注入方式为byType(根据类型进行匹配)，也就是说会优先根据接口类型去匹配并注入Bean(接口的实现类)autowired可以通过@Qualifier注解来显示指定名称。支持在构造函数、方法、字段和参数上使用。
- Resource属于JDK提供的注解，默认注入方式为byName(根据名称进行匹配）。如果无法通过名称匹配到对应的Bean的话，注入方式会变为byType.@Resource可以通过name属性来显示指定名称。而Resource主要用于字段和方法上的注入，不支持在构造函数或参数上使用。

#### 4.Bean的作用域

- **singleton**:IOC容器中只有唯一的bean实例。spring中的bean默认都是单例的，是对单例设计模式的应用。
- **prototype**：每次获取都会创建一个新的bean实例，连续两次getbean()，得到不同的实例。
- **request（**仅Web应用可用）：每次HTTP请求都会产生一个新的Bean（请求Bean），该Bean仅在当前HTTP request内有效。
- **session**(仅Web应用可用)：每一次新session的HTTP请求都会产生一个新的Bean（会话Bean),该bean仅在当前HTTP session内有效。
- **application、global-session**(仅Web应用可用):每个web应用在启动时创建一个Bean（应用Bean）,该bean仅在当前应用启动时间内有效。
- **websocket**:（仅Web应用可用）每一次websocket会话产生一个新的bean。

配置bean的作用域：

```
//XML方式
<bean id="..." class="..." scope="singleton"></bean>
//注解方式
@Bean
@Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE)
public Person personPrototype() {
    return new Person();
}
```

#### 5.Bean线程是安全的吗？

Spring 框架中的 Bean 是否线程安全，取决于其作用域和状态。

prototype 作用域下，每次获取都会创建一个新的 bean 实例，不存在资源竞争问题，所以不存在线程安全问题。singleton 作用域下，IoC 容器中只有唯一的 bean 实例，可能会存在资源竞争问题（取决于 Bean 是否有状态）。如果这个 bean 是有状态的话，那就存在线程安全问题（有状态 Bean 是指包含可变的成员变量的对象）。大部分 Bean 实际都是无状态（没有定义可变的成员变量）的（比如 Dao、Service），这种情况下， Bean 是线程安全的。

对于有状态单例 Bean 的线程安全问题，常见的有两种解决办法：

1. 在 Bean 中尽量避免定义可变的成员变量。
2. 在类中定义一个 `ThreadLocal` 成员变量，将需要的可变成员变量保存在 `ThreadLocal` 中（推荐的一种方式）。

#### 6.bean的生命周期【重】

- Bean容器找到配置文件中spring Bean的定义。
- Bean容器利用java Reflection API创建一个Bean的实例。
- 如果涉及到一些属性值利用set()方法设置一些属性值。
- 如果Bean实现了BeanNameAware接口，调用setBeanName()方法，传入Bean的名字。
- 如果Bean实现了如果 Bean 实现了 `BeanClassLoaderAware` 接口，调用 `setBeanClassLoader()`方法，传入 `ClassLoader`对象的实例。
- 如果 Bean 实现了 `BeanFactoryAware` 接口，调用 `setBeanFactory()`方法，传入 `BeanFactory`对象的实例。
- 与上面的类似，如果实现了其他 `*.Aware`接口，就调用相应的方法。
- 如果有和加载这个 Bean 的 Spring 容器相关的 `BeanPostProcessor` 对象，执行`postProcessBeforeInitialization()` 方法
- 如果 Bean 实现了`InitializingBean`接口，执行`afterPropertiesSet()`方法。
- 如果 Bean 在配置文件中的定义包含 init-method 属性，执行指定的方法。
- 如果有和加载这个 Bean 的 Spring 容器相关的 `BeanPostProcessor` 对象，执行`postProcessAfterInitialization()` 方法
- 当要销毁 Bean 的时候，如果 Bean 实现了 `DisposableBean` 接口，执行 `destroy()` 方法。
- 当要销毁 Bean 的时候，如果 Bean 在配置文件中的定义包含 destroy-method 属性，执行指定的方法。

![image-20240306162845660](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240306162845660.png)

![image-20240306162857824](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240306162857824.png)

### 3.spring AOP

#### 1.谈谈对AOP的了解

AOP（Aspect-Oriented Programming：面向切面编程)能够将那些与事务无关却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限管理）封装起来，便于减少系统的重复代码，降低模块间的耦合度，利于可拓展性和可维护性。spring AOP就是基于动态代理的。如果要代理的对象，实现了某个接口，那么 Spring AOP 会使用 **JDK Proxy**，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候 Spring AOP 会使用 **Cglib** 生成一个被代理对象的子类来作为代理。

**也可以使用 AspectJ ！Spring AOP 已经集成了 AspectJ ，AspectJ 应该算的上是 Java 生态系统中最完整的 AOP 框架了**

![image-20240306163258110](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240306163258110.png)

#### **2.spring AOP和AspectJ AOP的区别：**

spring AOP属于运行时增强，而AspectJ 是编译时增强。spring AOP基于代理（proxying)，而AspectJ基于字节码操作。

Spring AOP 已经集成了 AspectJ ，AspectJ 应该算的上是 Java 生态系统中最完整的 AOP 框架了。AspectJ 相比于 Spring AOP 功能更加强大，但是 Spring AOP 相对来说更简单，如果我们的切面比较少，那么两者性能差异不大。但是，当切面太多的话，最好选择 AspectJ ，它比 Spring AOP 快很多。

#### 3.AspectJ定义的通知类型有哪些？

- Before(前置通知)：目标对象的方法调用之前触发
- After(后置通知)：目标对象的方法调用之后触发
- AfterReturning(返回通知)：目标对象的方法调用完成，在返回结果值之后触发
- AfterThrowing(异常通知)：目标对象的方法运行中抛出/触发异常后触发。【fterReturning 和 AfterThrowing 两者互斥。如果方法调用成功无异常，则会有返回值；如果方法抛出了异常，则不会有返回值】
- Around(环绕通知)：编程式控制目标对象的方法调用。环绕通知是所有通知类型中可操作范围最大的一种，因为它可以直接拿到目标对象，以及要执行的方法，所以环绕通知可以任意的在目标对象的方法调用前后搞事，甚至不调用目标对象的方法

#### 4.多个切面的执行顺序如何控制？

```
1.通常使用@Oder注解直接定义切面顺序
// 值越小优先级越高
@Order(3)
@Component
@Aspect
public class LoggingAspect implements Ordered {
2.实现Ordered接口重写getOrder方法
@Component
@Aspect
public class LoggingAspect implements Ordered {

    // ....

    @Override
    public int getOrder() {
        // 返回值越小优先级越高
        return 1;
    }
}
```

### 4.spring MVC

#### 1.谈谈对spring MVC的了解。

MVC 是模型(Model)、视图(View)、控制器(Controller)的简写，其核心思想是通过将业务逻辑、数据、显示分离来组织代码。

MVC 是一种设计模式，Spring MVC 是一款很优秀的 MVC 框架。Spring MVC 可以帮助我们进行更简洁的 Web 层的开发，并且它天生与 Spring 框架集成。Spring MVC 下我们一般把后端项目分为 Service 层（处理业务）、Dao 层（数据库操作）、Entity 层（实体类）、Controller 层(控制层，返回数据给前台页面)

#### 2.spring MVC的核心组件

- DispatchServlet:**核心的中央处理器**，负责接收请求、分发、并给予客户端响应
- HandlerMapping:**处理器映射器**，根据URL去匹配查找能处理的Handler,并会将请求涉及到的拦截器和Handler一起封装。
- HandlerAdapter:**处理器适配器**，根据HandlerMapping找到的Handler，适配执行对应的Handler
- Handler：**请求处理器**，处理实际请求的处理器
- ViewResolver:**视图解析器**，根据Handler返回的逻辑视图/视图，解析并渲染真正的视图，并传递给DispatchServlet响应客户端。

#### 3.spring Mvc工作原理【重】

![image-20240306164742851](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240306164742851.png)

**流程说明（重要）：**

1. 客户端（浏览器）发送请求， `DispatcherServlet`拦截请求。
2. `DispatcherServlet` 根据请求信息调用 `HandlerMapping` 。`HandlerMapping` 根据 URL 去匹配查找能处理的 `Handler`（也就是我们平常说的 `Controller` 控制器） ，并会将请求涉及到的拦截器和 `Handler` 一起封装。
3. `DispatcherServlet` 调用 `HandlerAdapter`适配器执行 `Handler` 。
4. `Handler` 完成对用户请求的处理后，会返回一个 `ModelAndView` 对象给`DispatcherServlet`，`ModelAndView` 顾名思义，包含了数据模型以及相应的视图的信息。`Model` 是返回的数据对象，`View` 是个逻辑上的 `View`。
5. `ViewResolver` 会根据逻辑 `View` 查找实际的 `View`。
6. `DispaterServlet` 把返回的 `Model` 传给 `View`（视图渲染）。
7. 把 `View` 返回给请求者（浏览器）

#### 4.统一异常怎么做

推荐使用注解的方式统一异常处理，具体会使用到 `@ControllerAdvice` + `@ExceptionHandler` 这两个注解 。

```
@ControllerAdvice
@ResponseBody
public class GlobalExceptionHandler {

    @ExceptionHandler(BaseException.class)
    public ResponseEntity<?> handleAppException(BaseException ex, HttpServletRequest request) {
      //......
    }

    @ExceptionHandler(value = ResourceNotFoundException.class)
    public ResponseEntity<ErrorReponse> handleResourceNotFoundException(ResourceNotFoundException ex, HttpServletRequest request) {
      //......
    }
}
```

这种异常处理方式下，会给所有或者指定的 `Controller` 织入异常处理的逻辑（AOP），当 `Controller` 中的方法抛出异常的时候，由被`@ExceptionHandler` 注解修饰的方法进行处理。

`ExceptionHandlerMethodResolver` 中 `getMappedMethod` 方法决定了异常具体被哪个被 `@ExceptionHandler` 注解修饰的方法处理异常。

```
@Nullable
  private Method getMappedMethod(Class<? extends Throwable> exceptionType) {
    List<Class<? extends Throwable>> matches = new ArrayList<>();
    //找到可以处理的所有异常信息。mappedMethods 中存放了异常和处理异常的方法的对应关系
    for (Class<? extends Throwable> mappedException : this.mappedMethods.keySet()) {
      if (mappedException.isAssignableFrom(exceptionType)) {
        matches.add(mappedException);
      }
    }
    // 不为空说明有方法处理异常
    if (!matches.isEmpty()) {
      // 按照匹配程度从小到大排序
      matches.sort(new ExceptionDepthComparator(exceptionType));
      // 返回处理异常的方法
      return this.mappedMethods.get(matches.get(0));
    }
    else {
      return null;
    }
  }
```

**`getMappedMethod()`会首先找到可以匹配处理异常的所有方法信息，然后对其进行从小到大的排序，最后取最小的那一个匹配的方法(即匹配度最高的那个)**

### 5.Spring 框架中用到的设计模式

- **工厂设计模式** : Spring 使用工厂模式通过 `BeanFactory`、`ApplicationContext` 创建 bean 对象。
- **代理设计模式** : Spring AOP 功能的实现。
- **单例设计模式** : Spring 中的 Bean 默认都是单例的。
- **模板方法模式** : Spring 中 `jdbcTemplate`、`hibernateTemplate` 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。
- **包装器设计模式** : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
- **观察者模式:** Spring 事件驱动模型就是观察者模式很经典的一个应用。
- **适配器模式** : Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配`Controller`。

### 6.spring事物

#### 1.spring管理事务的方式有几种？

- **编程式事务**：在代码中硬编码：通过TransactionTemplate或者TransactionManager手动管理事务，事务范围过大会出现事务未提交导致超时，因此事务要比锁的粒度更小。
- **声明式事务**：在XML配置文件中配置或者直接基于注解（单体应用或者简单业务系统推荐使用）：实际是通过AOP实现（基于@Transactional的全注解方式使用z最多）

#### 2.spring事务中哪几种事务传播行为？

**事务传播行为是为了解决业务层方法之间互相调用的事务问题**。

```
正确的事务传播行为可能的值下:
1.TransactionDefinition.PROPAGATION_REQUIRED使用的最多的一个事务传播行为，我们平时经常使用的@Transactional注解默认使用就是这个事务传播行为。如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。2.TransactionDefinition.PROPAGATION_REQUIRES_NEW创建一个新的事务，如果当前存在事务，则把当前事务挂起。也就是说不管外部方法是否开启事务，Propagation.REQUIRES_NEW修饰的内部方法会新开启自己的事务，且开启的事务相互独立，互不干扰。
3.TransactionDefinition.PROPAGATION_NESTED如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于TransactionDefinition.PROPAGATION_REQUIRED。4.TransactionDefinition.PROPAGATION_MANDATORY如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。（mandatory：强制性）这个使用的很少
```

```
若是错误的配置以下 3 种事务传播行为，事务将不会发生回滚：TransactionDefinition.PROPAGATION_SUPPORTS: 如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。TransactionDefinition.PROPAGATION_NOT_SUPPORTED: 以非事务方式运行，如果当前存在事务，则把当前事务挂起。TransactionDefinition.PROPAGATION_NEVER: 以非事务方式运行，如果当前存在事务，则抛出异常。
```

#### 3.spring事务中的隔离级别有哪几种？

```
TransactionDefinition.ISOLATION_DEFAULT :使用后端数据库默认的隔离级别，MySQL 默认采用的 REPEATABLE_READ 隔离级别 Oracle 默认采用的 READ_COMMITTED 隔离级别.
TransactionDefinition.ISOLATION_READ_UNCOMMITTED :最低的隔离级别，使用这个隔离级别很少，因为它允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读
TransactionDefinition.ISOLATION_READ_COMMITTED : 允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或不可重复读仍有可能发生TransactionDefinition.ISOLATION_REPEATABLE_READ : 对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但幻读仍有可能发生。TransactionDefinition.ISOLATION_SERIALIZABLE : 最高的隔离级别，完全服从 ACID 的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及幻读。但是这将严重影响程序的性能。通常情况下也不会用到该级别
```

#### 4.Transactional(rollbackFor = Exception.class)注解了解吗？

`Exception` 分为运行时异常 `RuntimeException` 和非运行时异常。事务管理对于企业应用来说是至关重要的，即使出现异常情况，它也可以保证数据的一致性。

当 `@Transactional` 注解作用于类上时，该类的所有 public 方法将都具有该类型的事务属性，同时，我们也可以在方法级别使用该标注来覆盖类级别的定义。

`@Transactional` 注解默认回滚策略是只有在遇到`RuntimeException`(运行时异常) 或者 `Error` 时才会回滚事务，而不会回滚 `Checked Exception`（受检查异常）。这是因为 Spring 认为`RuntimeException`和 Error 是不可预期的错误，而受检异常是可预期的错误，可以通过业务逻辑来处理。

如果想要修改默认的回滚策略，可以使用 `@Transactional` 注解的 `rollbackFor` 和 `noRollbackFor` 属性来指定哪些异常需要回滚，哪些异常不需要回滚。

```
例如，如果想要让所有的异常都回滚事务，可以使用如下的注解：
@Transactional(rollbackFor = Exception.class)
public void someMethod() {
// some business logic
}
如果想要让某些特定的异常不回滚事务，可以使用如下的注解：
@Transactional(noRollbackFor = CustomException.class)
public void someMethod() {
// some business logic
}
```

### 7.spring Data JPA

#### 1.JPA的审计功能是做什么的？有什么用？

审计功能主要是帮助我们记录数据库操作的具体行为比如某条记录是谁创建的、什么时间创建的、最后修改人是谁、最后修改时间是什么时候。

```
@Data
@AllArgsConstructor
@NoArgsConstructor
@MappedSuperclass
@EntityListeners(value = AuditingEntityListener.class)
public abstract class AbstractAuditBase {

    @CreatedDate
    @Column(updatable = false)
    @JsonIgnore
    private Instant createdAt;

    @LastModifiedDate
    @JsonIgnore
    private Instant updatedAt;

    @CreatedBy
    @Column(updatable = false)
    @JsonIgnore
    private String createdBy;

    @LastModifiedBy
    @JsonIgnore
    private String updatedBy;
}
```

`@CreatedDate`: 表示该字段为创建时间字段，在这个实体被 insert 的时候，会设置值

`@CreatedBy` :表示该字段为创建人，在这个实体被 insert 的时候，会设置值

`@LastModifiedDate`、`@LastModifiedBy`同理。

#### 2.实体之间的关联关系注解有哪些？

- `@OneToOne` : 一对一。
- `@ManyToMany`：多对多。
- `@OneToMany` : 一对多。
- `@ManyToOne`：多对一。

利用 `@ManyToOne` 和 `@OneToMany` 也可以表达多对多的关联关系。

### 8.spring Security

#### 1.有哪些控制请求访问权限的方法？

![image-20240306170402908](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240306170402908.png)

- `permitAll()`：无条件允许任何形式访问，不管你登录还是没有登录。
- `anonymous()`：允许匿名访问，也就是没有登录才可以访问。
- `denyAll()`：无条件决绝任何形式的访问。
- `authenticated()`：只允许已认证的用户访问。
- `fullyAuthenticated()`：只允许已经登录或者通过 remember-me 登录的用户访问。
- `hasRole(String)` : 只允许指定的角色访问。
- `hasAnyRole(String)` : 指定一个或者多个角色，满足其一的用户即可访问。
- `hasAuthority(String)`：只允许具有指定权限的用户访问
- `hasAnyAuthority(String)`：指定一个或者多个权限，满足其一的用户即可访问。
- `hasIpAddress(String)` : 只允许指定 ip 的用户访问。

#### 2.hasRole 和hasAuthority有区别吗？

一言以蔽之，代码上来说，hasRole 和 hasAuthority 写代码时前缀不同，但是最终执行是一样的；设计上来说，role 和 authority 这是两个层面的权限设计思路，一个是角色，一个是权限，角色是权限的集合。

#### 3.如何对密码进行加码？

Spring Security 提供了多种加密算法的实现，开箱即用，非常方便。这些加密算法实现类的父类是 `PasswordEncoder` ，如果你想要自己实现一个加密算法的话，也需要继承 `PasswordEncoder`。

`PasswordEncoder` 接口一共也就 3 个必须实现的方法。

```
public interface PasswordEncoder {
    // 加密也就是对原始密码进行编码
    String encode(CharSequence var1);
    // 比对原始密码和数据库中保存的密码
    boolean matches(CharSequence var1, String var2);
    // 判断加密密码是否需要再次进行加密，默认返回 false
    default boolean upgradeEncoding(String encodedPassword) {
        return false;
    }
}
```

#### 4.如何更优雅更换系统使用的加密算法？

推荐的做法是通过 `DelegatingPasswordEncoder` 兼容多种不同的密码加密方案，以适应不同的业务需求。

从名字也能看出来，`DelegatingPasswordEncoder` 其实就是一个代理类，并非是一种全新的加密算法，它做的事情就是代理上面提到的加密算法实现类。在 Spring Security 5.0 之后，默认就是基于 `DelegatingPasswordEncoder` 进行密码加密的。

# 二、springboot面试题

#### 1.介绍一下spring及其的缺点。

spring是重量级企业开发框架Enterprise javaBean（EJB）的替代品。spring为企业级java开发提供了一种相对简单的方法，通过**依赖注入**和面向切面编程，**用简单的**java对象（Plain Old Java Object,POJO)实现EJB的功能。

虽然spring的组件是轻量级的，但它的配置确实重量级的（需要大量XML配置）

#### 2.为什么要有springboot？

spring旨在简化J2EE的开发，springboot旨在简化spring开发（减少配置文件，开箱即用）

#### 3.springboot的主要优点

- 开发基于spring的应用程序很容易。
- springboot项目所需的开发或工程时间明显减少，通常会提高整体生产力。
- springboot不需要写大量样本代码、XML配置文件和注释。
- spring引导应用程序可以很容易地与spring生态系统集成，如spring JDBC、spring ORM、spring Data、springSecurity等。
- springboot遵循“固执己见的默认配置”，以减少开发工作。
- springboot应用程序提供嵌入式HTTP服务器（tomcat)可以轻松地开发和测试web应用程序。
- springboot提供命令行接口（CLI）工具，用于开发和测试springboot应用程序。
- springboot提供了多种插件，内置工具（Maven和Gradle)开发和测试。

#### 4.什么是spring Boot Starters?

spring Boot Starters是一系列依赖关系的集合，这个依赖包含的子依赖中包含了开发REST服务所需要的所有依赖。

#### 5.springboot 支持哪些内嵌Servlet容器？

![image-20240306211224391](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240306211224391.png)

#### 6.介绍一下@SpringBootApplication注解

@SpringBootApplication可以看作是@Configuration、@EnableAutoConfiguration、@ComponentScan注解的集合。

- @Configuration：允许在上下文中注册额外的bean或导入其他配置类
- @EnableAutoConfiguration：启用springboot的自动配置机制
- @ComponentScan：扫描被@Compont(@Service、@Controller)注解的bean，注解默认会扫描该类所在的包下所有的类。

@EnableAutoConfiguration是启动自动配置的关键，这个注解通过spring提供的@Import注解导入了AutoConfigurationImportSelector类（@Import注解可以导入配置类或者Bean到当前类中）。AutoConfigurationImportSelector类中getCandidateConfigurations方法会将所有自动配置类的信息以List的形式返回。这些配置信息会被spring容器作Bean来管理。@Conditional注解。@ConditionalOnClass(指定的类必须存在于类路径下)，@Conditional(容器中是否有指定的Bean)等都是对@Conditional注解的扩展。

#### 7.开发RESTful Web服务常用的注解有哪些？

**Spring Bean相关：**

- @Autowired:自动导入对象到类中，被注入进的类同样要被spring容器管理。
- @RestController:注解是@Controller和@ResponseBody的合集，表示这个是个控制器bean，并且是将函数的返回值直接填入HTTP响应体中，是REST风格的控制器。
- @Component:通用的注解，可标注任意类为spring组件，如果一个bean不知道属于哪个层，可以使用@Component注解标注。
- @Reposity:对应持久层即DAO层，主要用于数据库相关操作。
- @Service:对应服务层，主要涉及一些复杂的逻辑，需要用到Dao层。
- @Controller:对应spring MVC控制层，主要用于接受用户请求并调用Service层返回数据给前端页面。

**处理常见的HTTP请求类型：**

- @GetMapping:GET请求
- @PostMapping:POST请求
- @PutMapping:PUT请求
- @DeleteMapping:DELETE请求

**前后端传值：**

- @RequestParam以及@Pathvairable:前者用于获取查询参数，后者用于获取路径参数。
- @RequestBody:用于读取Request请求（可能是GET\POST\PUT\DELETE）的body部分并且Content-Type为application、json格式的数据，接收到的数据之后会自动将数据绑定到java对象上。系统会使用HttpMessageConverter或者自定义的HttpMessageConverter请求的body中的json字符串转换为java对象。

#### 8.springboot常用的两种配置文件

通过application.properties或者application.yml进行配置。

#### 9.什么是YAML，它配置的优势是？

YAML是一种人类可读的数据序列化语言。它通常用于配置文件。与属性文件相比，如果我们想要在配置文件中天健复杂的竖向，YAML文件就更加结构化，而且更少混淆。YAML具有分层配置数据。

相比于Properties配置的方式，YAML配置的方式更加直观清晰，简介明了。但是YAML不支持@PropertySource注解导入自定义的YAML配置。

#### 10.常用的Bean映射工具有哪些？

![image-20240308194411586](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240308194411586.png)

#### 11.springboot如何监控系统实际运行状况？

使用springboot Actuator 来对springboot 项目进行简单的监控。集成这个模块之后，springboot应用程序就自带一些开箱即用的获取程序运行时的内部状态信息的API。

#### 12.springboot如何做请求参数校验？

只需要spring-boot-starter-web依赖就够了，它的子依赖包含了我们需要的东西。

#### 13.校验注解：

![image-20240308195200428](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240308195200428.png)

![image-20240308195214977](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240308195214977.png)

#### 14.如何使用springboot实现全局异常处理？

使用@ControllerAdvice和@ExceptionHandler处理全局异常。

#### 15.springboot中实现定时任务？

使用@Scheduled 注解就能很方便地创建一个定时任务。单纯依靠@Scheduled注解还不行，还需要再springboot 中我们只需要在启动类上加上@EnableScheduling注解，这样才可以启动定时任务。@EnableScheduling注解的作用是发现注解@Scheduled的任务并在后台执行该任务。

#### 16.如何快速构建一个springboot项目？

![image-20240308195448944](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240308195448944.png)

#### 17.不同的环境的配置文件

![image-20240308195556739](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240308195556739.png)
