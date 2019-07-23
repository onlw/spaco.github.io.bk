---
title: types-of-dependency-injection
permalink: types-of-dependency-injection
categories:
- program
tags: 
- IoC
comments: true
date: 2019-07-15
updated: 2019-07-15
---

Types of Dependency Injection
此篇文章使用 Java 代码作为演示

如果希望了解 Dependency Injection 和  Inversion of Control 可以参考··

Spring通过DI（依赖注入）实现IoC（控制反转)

建议看一下这篇文章

https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-dependencies

## Types

### Constructor Injection

基于构造函数的DI由容器调用具有多个参数的构造函数来完成，每个参数表示一个依赖项。 调用具有特定参数的静态工厂方法来构造bean几乎是等效的，本讨论同样处理构造函数和静态工厂方法的参数。 以下示例显示了一个只能通过构造函数注入进行依赖注入的类：

```java
public class SimpleMovieLister {

    // the SimpleMovieLister has a dependency on a MovieFinder
    private MovieFinder movieFinder;

    // a constructor so that the Spring container can inject a MovieFinder
    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // business logic that actually uses the injected MovieFinder is omitted...
}
```

#### Advantages

- 能够在构造阶段就创建完整、合法的对象；
- 带有参数的构造函数可以明确地告诉你创建一个合法的对象需要哪些参数

#### Disadvantages

- 构造函数依赖的太多，会显得很复杂（事实上，当一个控制器依赖太多service，这个控制器本身就是有问题的，需要拆解）

  ```java
  public class SimpleMovieLister {
  
      private A a;
  		private B b;
    	private C c;
    	private D d;
    
      public SimpleMovieLister(A a,B b,C c,D d) {
          this.a = a;
        	this.b = b;
        	this.c = c;
        	this.d = d;
      }
  }
  ```

  

### Setter Injection | Property Injection

在调用无参数构造函数或无参数`static`工厂方法来实例化bean之后，基于setter的DI由bean上的容器调用setter方法完成。

以下示例显示了一个只能通过使用纯setter注入进行依赖注入的类。这个类是传统的Java。它是一个POJO，它不依赖于容器特定的接口，基类或注释。

```java
public class SimpleMovieLister {

    // the SimpleMovieLister has a dependency on the MovieFinder
    private MovieFinder movieFinder;

    // a setter method so that the Spring container can inject a MovieFinder
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // business logic that actually uses the injected MovieFinder is omitted...
} 
```

#### Advantages

- 如果依赖的「插件」太多时，选择 Setter 注入更优

#### Disadvantages

- 无法在构造阶段就创建完整、合法的对象；

- 带有参数的构造函数无法明确地告诉你创建一个合法的对象需要哪些参数

  

### Method Injection | Function Injection 

What is method injection？

Spring 中 Method Injection 分为 [Lookup Method Injection](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-lookup-method-injection) （查找方法注入）和 [Arbitrary Method Replacement](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-arbitrary-method-replacement)（任意方法替换）

在大多数应用程序场景中，容器中的大多数bean都是 [单例](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-scopes-singleton)。当单例bean需要与另一个单例bean协作或非单例bean需要与另一个非单例bean协作时，通常通过将一个bean定义为另一个bean的属性来处理依赖关系。当bean生命周期不同时会出现问题。假设单例bean A需要使用非单例（原型）bean B，可能是在A上的每个方法调用上。容器只创建一次单例bean A，因此只有一次机会来设置属性。每次需要时，容器都不能为bean A提供bean B的新实例。即 需要新实例。

A solution is to forego some inversion of control. You can [make bean A aware of the container](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-aware) by implementing the `ApplicationContextAware` interface, and by [making a `getBean("B")` call to the container](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-factory-client) ask for (a typically new) bean B instance every time bean A needs it. The following example shows this approach:



## Summary

Constructor Injection 和 Setter Injection | Property Injection 是较常见的的注入方式，Method Injection 使用的较少。

个人比较推荐 Constructor Injection，优点如上所述。

还有一些文章说 Annotation Injection （注解注入）：通过注解将类注入到类属性中。个人感觉本质上和 Constructor Injection 没什么区别，都是初始化时将类注入到类属性中。

不推荐以下写法，自己通过注解初始化类参数：

**随着版本的迭代，开发者更注重于业务本身，而会忽略初始化的参数越来越多**

```php
class Foo
{
  	/**
     * @Inject A
     */
    private A a;
  
    /**
     * @Inject B
     */
    private B b;
  
    /**
     * @Inject C
     */
    private C c;
}
```



## References

- [DI-wiki](https://en.wikipedia.org/wiki/Dependency_injection)
- [method-injection](https://spring.io/blog/2004/08/06/method-injection/)
- [docs.spring.io.beans-dependencies](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-dependencies)
- [A quick intro to Dependency Injection: what it is, and when to use it](https://www.freecodecamp.org/news/a-quick-intro-to-dependency-injection-what-it-is-and-when-to-use-it-7578c84fa88f/)
- [Dependency Injection](https://www.tutorialsteacher.com/ioc/dependency-injection)
- [spring-framework-reference.core](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html)
- [Java方法注入（Method_injection）](http://www.qingpingshan.com/rjbc/java/255320.html)
- [Understand Dependency Injection: Function/Method Injection](https://www.c-sharpcorner.com/UploadFile/dacca2/understand-dependency-injection-functionmethod-injection/)
- [Spring查找方法注入(Lookup method injection)的底层实现原理](https://my.oschina.net/zudajun/blog/664659)
- [Difference Between @Resource, @Autowired and @Inject in Spring Injection](https://javabeat.net/difference-resource-autowired-inject-spring-injection/)