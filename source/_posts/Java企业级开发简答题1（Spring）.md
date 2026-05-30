---
title: Java企业级开发简答题1（Spring）
date: 2025-07-11 15:11:11
tags:
  - Java
  - Spring
  - 后端
categories:
  - Java企业级开发
---

# 作业 1：简答题

> 22371495 吴自强

[TOC]

### 1. Spring 的核心容器有哪些模块组成？列举 Spring 框架的优点

<u>Spring核心容器的四个模块为</u>：

- Spring-core模块：提供了框架的基本功能，包括 IoC（控制反转）和依赖注入（DI）机制。
- Spring-beans模块：提供对 Bean 的配置、创建和管理，是 IoC 的核心部分。
- Spring-context模块：基于 Core 和 Beans 模块构建，提供更高级的应用框架（如国际化、事件传播、资源访问等），常用类如 `ApplicationContext`。
- Spring-expression模块：提供强大的表达式语言，用于在运行时查询和操作对象图（如 `#{user.name}`）。

<u>Spring 框架的优点</u>：

1. **轻量级、非侵入性**
   Spring 是轻量级的，依赖注入不依赖于具体实现类，业务代码不需要继承特定类，降低耦合。
2. **IoC（控制反转）和 DI（依赖注入）**
   通过 IoC 容器管理对象生命周期和依赖关系，使得组件更容易解耦和测试。
3. **AOP（面向切面编程）支持**
   方便实现事务管理、日志记录、安全控制等横切关注点。
4. **统一事务管理**
   提供对声明式和编程式事务的支持，能整合 JDBC、Hibernate、JPA、MyBatis 等事务处理。
5. **良好的集成能力**
   能很好地整合第三方框架（如 Hibernate、MyBatis、Quartz、Redis）和 Java EE 技术（如 JPA、JMS、Servlet）。
6. **模块化设计**
   各模块相互独立，可按需引入，避免臃肿。
7. **支持多种视图技术**
   如 JSP、Thymeleaf、FreeMarker 等，适用于不同前端技术栈。
8. **强大的社区和生态**
   Spring Boot、Spring Cloud 等项目简化了微服务和企业级开发的复杂性。

### 2. 在 Spring 框架中，什么是控制反转？什么是依赖注入？使用控制反转与依赖注入有什么优点？

<u>控制反转</u>指的是**对象的创建和依赖关系的管理不再由程序本身控制，而是交由 Spring 容器统一管理**。
传统方式中，类通常会通过 `new` 创建依赖对象；使用 IoC 后，这个过程被“反转”到了框架中。控制权由调用者转移到Spring容器，控制权发生了反转，这就是Spring的控制反转。

<u>依赖注入</u>是控制反转的一种实现方式，指的是**由 Spring 容器将对象所依赖的组件（依赖项）“注入”到对象中**，而不是由对象自己创建依赖。

<u>使用控制反转与依赖注入的优点</u>：

| 优点             | 说明                                       |
| ---------------- | ------------------------------------------ |
| **解耦**         | 对象之间通过接口和容器联系，降低模块间依赖 |
| **提高可测试性** | 易于使用 Mock 替代真实依赖进行单元测试     |
| **可配置性强**   | 对象关系可以通过配置文件或注解灵活管理     |
| **复用性高**     | 可重用组件被容器统一管理，避免重复创建     |
| **增强扩展性**   | 容易实现 AOP（如事务、日志）等功能         |
| **降低维护成本** | 对象关系集中管理，修改影响范围小           |

### 3. 在 Spring 框架中，有哪些不同类型的依赖注入实现方式？

Spring框架的依赖注入通常有两种实现方式：

- 一种是**构造方法**注入
- 另一种是**属性setter方法**注入。

对于两种方法注入，Spring框架都是基于Java的反射机制实现的。

### 4. 在Spring 框架中，BeanFactory与AppliacationContext有什么区别？ApplicationContext的实现类有哪些?

ApplicaitonContext是BeanFactory的子接口。

- BeanFactory：IOC 容器基本实现，是 Spring 内部的使用接口，不提供开发人员进行使用，这种方式加载配置文件时候不会创建对象，在获取对象（使用时）才去创建对象 。（晚加载）
- ApplicationContext：BeanFactory 接口的子接口，提供更多更强大的功能，一般由开发人员进行使用,这种方式在加载配置文件时候就会把配置文件中的对象进行创建。(早加载)

| 特性             | `BeanFactory`                             | `ApplicationContext`                     |
| ---------------- | ----------------------------------------- | ---------------------------------------- |
| **层次关系**     | 最基本的容器接口                          | 是 `BeanFactory` 的子接口，扩展功能更强  |
| **加载方式**     | **懒加载**（延迟加载 Bean，使用时才创建） | **预加载**（启动时创建所有单例 Bean）    |
| **国际化支持**   | 不支持                                    | 支持（如 `getMessage()`）                |
| **事件发布机制** | 不支持                                    | 支持（如 `ApplicationEventPublisher`）   |
| **AOP 支持**     | 支持（需手动配置）                        | 更好支持，集成更方便                     |
| **资源访问**     | 基本无                                    | 提供资源加载（如 ClassPath、FileSystem） |
| **使用场景**     | 内存资源紧张、仅管理少量 Bean             | 常规开发中推荐使用                       |

<u>ApplicationContext 的常见实现类</u>：

| 实现类                                 | 说明                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| **ClassPathXmlApplicationContext**     | 从类路径下加载 XML 配置文件                                  |
| **FileSystemXmlApplicationContext**    | 从文件系统中加载 XML 配置文件                                |
| **AnnotationConfigApplicationContext** | 用于基于注解的配置（如 `@Configuration`）                    |
| **WebApplicationContext**              | 专用于 Web 应用（Spring MVC 中使用）                         |
| **ConfigurableWebApplicationContext**  | 扩展了 WebApplicationContext，它允许通过配置的方式实例化WebApplicationContext |

### 5. Spring 支持 bean 的作用域有几种？

Spring 支持的 bean 的作用域如下：

| 作用域        | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| `singleton`   | 默认的作用域，使用singleton定义的Bean在Spring容器中只有一个Bean实例。 |
| `prototype`   | Spring容器每次获取prototype定义的Bean，容器都将创建一个新的Bean实例。 |
| `request`     | 在一次HTTP请求中容器将返回一个Bean实例，不同的HTTP请求返回不同的Bean实例。仅在Web Spring应用程序上下文中使用。 |
| `session`     | 在一个HTTP Session中，容器将返回同一个Bean实例。仅在WebSpring应用程序上下文中使用。 |
| `application` | 为每个ServletContext对象创建一个实例，即同一个应用共享一个Bean实例。仅在Web Spring应用程序上下文中使用。 |
| `websocket`   | 为每个WebSocket对象创建一个Bean实例。仅在Web Spring应用程序上下文中使用。 |

### 6. Spring 有几种配置方式？（基于 xml、基于注解、基于 Java 配置）针对上述配置，请举例说明。

 <u>一、基于 XML 的配置方式</u>

 特点：

- 配置集中，清晰可见，适合配置大量 Bean。
- 不需要修改代码即可改变配置（例如数据源等）。

示例：

**beans.xml**：

```xml
<beans xmlns="http://www.springframework.org/schema/beans" ...>
    <bean id="userService" class="com.example.UserService">
        <property name="userRepository" ref="userRepository"/>
    </bean>

    <bean id="userRepository" class="com.example.UserRepository"/>
</beans>
```

**使用方式**：

```java
ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
UserService service = context.getBean("userService", UserService.class);
```

 <u>二、基于注解的配置方式（常用于 Spring Boot）</u>

特点：

- 使用注解减少 XML 配置，代码更紧凑。
- 与 Java 类紧耦合，适合小项目或组件内部。

示例：

```java
@Component
public class UserRepository {}

@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
}
```

**开启注解扫描**（XML 方式或 Java 配置方式）：

```xml
<context:component-scan base-package="com.example"/>
```

或

```java
@Configuration
@ComponentScan("com.example")
public class AppConfig {}
```

<u>三、基于 Java 配置类（JavaConfig）</u>

特点：

- 完全用 Java 代码代替 XML。
- 强类型检查，编译期就能发现错误。
- 适用于中大型项目、Spring Boot 推荐方式。

示例：

```java
@Configuration
@ComponentScan("com.example")
public class AppConfig {

    @Bean
    public UserRepository userRepository() {
        return new UserRepository();
    }

    @Bean
    public UserService userService() {
        return new UserService(userRepository());
    }
}
```

**使用方式**：

```java
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
UserService service = context.getBean(UserService.class);
```

### 7. 什么是基于 Java 的 Spring 注解配置? 请举例说明？

**<u>基于 Java 的 Spring 注解配置</u>（Java-based configuration）** 是指：使用普通的 Java 类和注解（如 `@Configuration`、`@Bean`、`@ComponentScan` 等）来代替传统的 XML 配置文件，完成 Spring 容器的初始化与 Bean 的定义与装配。这是 **Spring 3.0+** 引入的一种更灵活、类型安全的配置方式，也是 **Spring Boot 推荐的主流方式**。

<u>举例如下</u>：

1. 定义业务类和依赖

   ```java
   // 依赖类
   public class UserRepository {
       public void save() {
           System.out.println("UserRepository.save()");
       }
   }
   
   // 服务类
   public class UserService {
       private final UserRepository userRepository;
   
       public UserService(UserRepository userRepository) {
           this.userRepository = userRepository;
       }
   
       public void doSomething() {
           userRepository.save();
       }
   }
   ```

2. 配置类（JavaConfig）

   ```java
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   @Configuration
   public class AppConfig {
   
       @Bean
       public UserRepository userRepository() {
           return new UserRepository();
       }
   
       @Bean
       public UserService userService() {
           // 手动注入依赖
           return new UserService(userRepository());
       }
   }
   ```

3. 启动 Spring 容器并使用 Bean

   ```java
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.annotation.AnnotationConfigApplicationContext;
   
   public class MainApp {
       public static void main(String[] args) {
           ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
           UserService service = context.getBean(UserService.class);
           service.doSomething();
       }
   }
   ```

### 8. 什么是注解？Spring 的常用注解有哪些？怎样开启注解装配？

**<u>注解</u>** 是 Java 5 引入的一种 **元数据机制**，用于为程序中的类、方法、字段等添加额外的信息。它本质上是标记，用于被编译器或框架识别并执行特定逻辑。在 Spring 中，注解被广泛用于实现配置、依赖注入、面向切面编程等功能，是替代 XML 配置的主流方式。

---

<u>Spring 常用注解</u>：

1. 通用组件声明注解（用于标记为 Bean）

   | 注解          | 用途         | 描述                                   |
   | ------------- | ------------ | -------------------------------------- |
   | `@Component`  | 通用组件     | 表示一个通用 Bean，会被扫描到容器中    |
   | `@Service`    | 业务逻辑组件 | 表示 Service 层 Bean                   |
   | `@Repository` | 数据访问组件 | 表示 DAO 层 Bean，支持数据访问异常转换 |
   | `@Controller` | Web 控制器   | Spring MVC 控制器类                    |

2. 依赖注入相关注解

   | 注解                   | 用途               | 描述                                                |
   | ---------------------- | ------------------ | --------------------------------------------------- |
   | `@Autowired`           | 自动注入           | 默认按类型注入 Bean，可与 `@Qualifier` 配合指定名称 |
   | `@Qualifier`           | 指定 Bean 名称注入 | 配合 `@Autowired` 使用                              |
   | `@Value`               | 注入常量、配置值   | 可从 `application.properties` 注入值                |
   | `@Inject`（JSR-330）   | 自动注入（标准）   | 与 `@Autowired` 类似，但无 Spring 特有功能          |
   | `@Resource`（JSR-250） | 自动注入           | 默认按名称注入，也可按类型                          |

3. 配置相关注解

   | 注解              | 用途           | 描述                              |
   | ----------------- | -------------- | --------------------------------- |
   | `@Configuration`  | 配置类         | 等价于一个 XML 配置文件           |
   | `@Bean`           | 定义 Bean      | 在配置类中定义方法并返回 Bean     |
   | `@ComponentScan`  | 扫描组件       | 指定扫描的包路径                  |
   | `@Import`         | 导入其他配置类 | 用于模块化配置                    |
   | `@PropertySource` | 加载配置文件   | 例如加载 `application.properties` |

---

<u>开启注解装配的两种方法</u>：

1. 基于 Java 配置开启组件扫描：

   ```java
   @Configuration
   @ComponentScan(basePackages = "com.example")
   public class AppConfig {
   }
   ```

2. 基于 XML 开启组件扫描

   ```xml
   <context:component-scan base-package="com.example"/>
   ```

### 9. @Component, @Controller, @Repository, @Service 有何区别？

| 注解          | 适用层                   | 语义作用                    | 特殊支持                              |
| ------------- | ------------------------ | --------------------------- | ------------------------------------- |
| `@Component`  | 通用组件层               | 标准的组件类，无特定角色    | 无                                    |
| `@Service`    | **业务逻辑层**           | 表示这是一个 Service 组件   | 用于业务逻辑封装，可能与 AOP 搭配使用 |
| `@Repository` | **持久层（DAO）**        | 表示数据访问组件            | 自动转换数据库相关异常为 Spring 异常  |
| `@Controller` | **表现层（MVC 控制器）** | 表示控制器类，处理 Web 请求 | 配合 Spring MVC 处理前端请求          |

### 10. @Autowired 和@Resource 之间的区别，@Qualifier 注解有什么作用？

<u>@Autowired 和@Resource 之间的区别如下</u>：

| 对比项                      | `@Autowired`（Spring 提供）            | `@Resource`（JDK 提供，JSR-250） |
| --------------------------- | -------------------------------------- | -------------------------------- |
| 所属框架                    | Spring 专属注解                        | Java 标准注解                    |
| 依赖注入方式                | **默认按类型注入**（byType）           | **默认按名称注入**（byName）     |
| 是否支持按名称注入          | 可以搭配 `@Qualifier("beanName")` 实现 | 默认就按名称注入                 |
| 是否支持构造方法注入        | 支持                                   | 不支持（只能字段或 setter）      |
| 是否支持 `@Primary`         | 支持                                   | 不支持                           |
| 是否支持 `required = false` | 支持                                   | 不支持                           |
| 是否推荐                    | Spring 推荐使用                        | 兼容标准但功能有限               |

<u>@Qualifier 注解的作用如下</u>：

默认情况下，@Autowired 按类型装配 Spring Bean。当存在 **多个相同类型的 Bean** 时，`@Autowired` 无法唯一确定注入哪个 Bean，就会报错。

此时使用 `@Qualifier` 指定 **Bean 的名称**，用于明确选择哪一个 Bean。

### 11. Spring 支持两种类型的事务管理，分别是什么？如何实现的？你更倾向用那种事务管理类型？为什么？

<u>Spring 支持两种类型的事务管理</u>：

**1. 编程式事务管理（Programmatic Transaction Management）**

- 编程式事务管理是通过编写代码实现的事务管理。这种方式能够在代码中精确地定义事务的边界,我们可以根据需求规定事务从哪里开始,到哪里结束。

- **实现方式**：程序员通过代码显式地控制事务的开启、提交和回滚。通常通过 Spring 提供的 `PlatformTransactionManager` 和 `TransactionTemplate` 来实现。

- **代码示例**：

  ```java
  TransactionTemplate transactionTemplate = new TransactionTemplate(transactionManager);
  transactionTemplate.execute(status -> {
      // 业务代码
      return result;
  });
  ```

- **特点**：

  - 细粒度控制，灵活。
  - 代码侵入性强，业务代码和事务管理耦合度高。
  - 维护成本较高，不利于代码复用。

**2. 声明式事务管理（Declarative Transaction Management）**

- Spring声明式事务管理在底层采用了AOP技术，其最大的优点在于无须通过编程的方式管理事务，只需要在配置文件中进行相关的规则声明,就可以将事务规则应用到业务逻辑中。

- **实现方式**：通过配置（XML 或注解）声明事务的边界，Spring AOP（代理机制）自动为目标方法生成事务代理，无需在代码中手动控制事务。

- **常用实现**：

  - 使用 `@Transactional` 注解
  - XML 配置 `<tx:advice>` 和事务切面

- **示例**：

  ```java
  @Transactional
  public void someServiceMethod() {
      // 业务代码，Spring 自动管理事务
  }
  ```

- **特点**：

  - 代码与事务逻辑分离，业务代码更简洁。
  - 易于维护和管理。
  - 灵活配置事务属性（传播行为、隔离级别、超时等）。

------

我更倾向于使用**声明式事务管理**，原因如下：

1. **简洁清晰**：使用注解或配置即可实现事务管理，无需在业务代码中写大量事务控制逻辑。
2. **低耦合**：业务逻辑和事务控制分离，代码更易于维护和理解。
3. **灵活配置**：可以通过配置灵活调整事务属性，支持不同方法使用不同事务策略。
4. **Spring 推荐方式**：Spring 官方和社区普遍推荐声明式事务，适合绝大多数业务场景。

如果需要非常细粒度或动态的事务控制，或者在特定复杂场景下，才会考虑编程式事务管理。大多数日常开发推荐用声明式事务。

### 12. Spring 框架的事务管理有哪些优点？

<u>Spring 框架的事务管理有以下几个优点</u>：

1. **统一的事务管理接口**
   Spring 提供统一的 `PlatformTransactionManager` 接口，支持多种事务管理器（JDBC、JTA、Hibernate、JPA 等），方便开发者在不同持久化技术间切换而不改业务代码。
2. **声明式事务支持，简化开发**
   通过注解（如 `@Transactional`）或 XML 配置即可实现事务管理，业务代码无需显式管理事务，减少代码侵入，增强代码的可读性和维护性。
3. **灵活的事务配置**
   可以针对不同方法设置不同的事务属性，如传播行为（Propagation）、隔离级别（Isolation）、超时（Timeout）、只读（Read-only）等，满足复杂业务需求。
4. **支持多种事务传播行为**
   Spring 支持多种传播机制，比如 `REQUIRED`、`REQUIRES_NEW` 等，能够处理复杂的事务嵌套和事务边界，保证事务的一致性和完整性。
5. **跨多个资源的分布式事务支持**
   Spring 能集成 JTA 事务管理器，支持分布式事务，协调多个资源管理器（如多个数据库、消息队列等）的一致提交和回滚。
6. **良好的异常回滚机制**
   支持根据异常类型自动决定事务是否回滚，默认运行时异常触发回滚，也可以自定义规则，提高事务的可靠性。
7. **基于 AOP 实现，非侵入式**
   利用 Spring AOP 实现事务管理，不需要修改业务逻辑代码，做到关注点分离，提高代码整洁度。
8. **易于集成与扩展**
   Spring 事务管理与其他 Spring 组件（如 Spring Data、Spring MVC、Spring Boot）无缝集成，方便构建企业级应用。

### 13. 什么是 AOP，AOP 中有哪些关键术语，什么是切面（Aspect），什么是通知（Advice）？Spring 通知有哪些类型？

**<u>AOP</u>（Aspect-Oriented Programming，面向切面编程）** 是一种编程范式，它通过将横切关注点（cross-cutting concerns，如事务管理、日志、安全等）从业务逻辑中分离出来，实现关注点的模块化。
 简而言之，AOP 能帮助我们在不改变业务代码的情况下，把一些公共功能（如日志、事务）“切入”到程序的执行流程中。

<u>AOP 中的关键术语</u>：

- **切面（Aspect）**
  切面是指封装横切到系统功能（如事务处理）的类，是横切关注点的模块化表现，是通知和切点的结合体。它定义了具体在哪些位置（切点）执行什么样的额外行为（通知）。
- **连接点（Join point）**
  程序执行中的某个特定点，比如方法调用、方法执行、异常抛出等。在 Spring AOP 中，连接点通常指与方法执行相关的特定节点。
- **切入点（Pointcut）**
  切入点是对连接点的定义和筛选，是指那些需要处理的连接点的集合，告诉 AOP 框架在哪些连接点上应用通知。它通过表达式匹配特定的方法或类。
- **通知（Advice）**
  通知是由切面添加到特定的连接点（满足切入点规则）的一段代码，是切面中定义的具体动作，指在切点上要执行的代码，比如前置通知、后置通知等。
- **目标对象（Target Object）**
  被通知的业务对象，通知所作用的目标业务类就是目标对象。
- **代理对象（Proxy）**
  由 Spring AOP 创建的包装了目标对象的对象，通过代理实现对目标对象方法的增强。目标类被AOP织入增强后产生的一个结果类，这个结果类就是代理类，代理类融合了原类和增强的逻辑。
- **织入（Weaving）**
  织入是将通知添加到目标类具体连接点的过程，这些可以在编译时、类加载时和运行时完成。
- **引入（Introduction）**
  引介是一种特殊的通知，它为类添加一些属性和方法。即使一个业务类原本没有实现某一个接口，通过AOP的引介功能，也可以动态地为该业务类添加接口的实现逻辑，让业务类成为这个接口的实现类。

------

<u>Spring 通知的类型</u>：

- **环绕通知
  环绕通知是在目标方法执行前和执行后实施增强，可以应用于日志记录、事务处理等功能。

- **前置通知
  前置通知是在目标方法执行前实施增强，可应用于权限管理等功能。
- **后置通知**
  后置返回通知是在目标方法成功执行后实施增强，可应用于关闭流、删除临时文件等功能。
- **后置（最终）通知**
  后置通知是在目标方法执行后实施增强，与后置返回通知不同的是，不管是否发生异常都要执行该通知，可应用于释放资源。
- **异常通知
  异常通知是在方法抛出异常后实施增强，可以应用于处理异常、记录日志等功能。
- **引入通知**
  引入通知是在目标类中添加一些新的方法和属性，可以应用于修改目标类（增强类）。

### 14. Spring AOP and AspectJ AOP 有什么区别？AOP 有哪些实现方式？

<u>Spring AOP 与 AspectJ AOP 的区别</u>：

| 特点                     | Spring AOP                            | AspectJ AOP                              |
| ------------------------ | ------------------------------------- | ---------------------------------------- |
| **实现机制**             | 基于 **动态代理**（JDK 或 CGLIB）     | 基于 **编译时织入** 或 **类加载时织入**  |
| **织入时机**             | 运行时（Runtime）                     | 编译期或类加载期                         |
| **目标**                 | 主要用于方法级别增强                  | 支持方法、构造函数、字段等更细粒度的增强 |
| **配置复杂度**           | 简单（基于 Spring 配置或注解）        | 相对复杂（需要额外编译器或插件支持）     |
| **性能**                 | 性能较好（只增强 Spring 管理的 Bean） | 性能最佳（直接织入字节码）               |
| **灵活性与功能强大程度** | 功能相对有限（主要方法级别）          | 功能强大（支持更多连接点类型）           |
| **常见使用场景**         | 大多数企业开发中足够使用              | 对性能和织入粒度要求更高的项目中使用     |

<u>AOP 的实现方式</u>主要包括以下几种：

1. **基于代理的 AOP**

   - Spring AOP 的核心方式。

   - 使用 JDK 动态代理（针对接口）或 CGLIB 字节码生成（针对类）。

   - 运行时通过代理对象织入增强逻辑。

   - 优点：不修改原始类，Spring 自动管理；缺点：只能作用于 Spring 容器中的 Bean 且主要支持方法级别增强。

2. **基于字节码织入的 AOP（如 AspectJ）**

   - 在编译阶段或类加载阶段修改字节码，直接将增强代码嵌入到目标类中。

   - 支持更丰富的连接点（如字段、构造器）。

   - 优点：性能高，灵活性强；缺点：配置复杂，依赖 AspectJ 编译器或 LTW（Load-Time Weaving）机制。

3. **基于字节码操作框架（如 ASM、Javassist）**

   - 手动或通过框架修改类字节码，实现 AOP 功能。

   - 更底层，通常用于构建自己的 AOP 框架或进行底层性能优化。

   - 一般不推荐直接使用，除非有特殊需求。

4. **基于自定义注解 + 反射 +代理**

   - 使用自定义注解标记方法 + 反射拦截执行 + 动态代理注入增强逻辑。

   - 比较原始，但可以满足简单 AOP 需求，常用于学习或特定轻量场景。

### 15. 什么是 Spring MVC ？简单介绍下你对 springMVC 的理解

Spring MVC（Model-View-Controller）是 Spring 框架中的一个**基于请求驱动的 Web 框架**，用于构建**Web 应用程序**。它采用了经典的 MVC 设计模式，目的是将表现层（前端展示）和业务逻辑层（后端处理）解耦，使 Web 应用更清晰、更易于维护和扩展。

---

我的理解：Spring MVC 是一个轻量级、高扩展性的 Web 框架，能够快速开发清晰、模块化的 Web 应用，尤其适合构建 RESTful 接口。

------

1. **Spring MVC 的核心流程**

一次请求在 Spring MVC 中的处理流程如下：

```
客户端 --> DispatcherServlet --> HandlerMapping
             ↓
         Controller（处理器）  
             ↓
         返回 ModelAndView
             ↓
        ViewResolver（视图解析器）
             ↓
            渲染页面 / JSON
             ↓
           返回客户端
```

2. **核心组件说明**

| 组件                  | 作用                                                       |
| --------------------- | ---------------------------------------------------------- |
| **DispatcherServlet** | 前端控制器，接收请求并统一调度处理。                       |
| **HandlerMapping**    | 负责根据 URL 查找对应的控制器（Controller）。              |
| **Controller**        | 控制器，处理请求并返回数据或视图名。                       |
| **ModelAndView**      | 封装模型数据和视图名。                                     |
| **ViewResolver**      | 将逻辑视图名解析为实际视图（如 JSP、Thymeleaf、JSON 等）。 |
| **View**              | 负责展示页面或数据给用户。                                 |

3. **Spring MVC 的特点**

   - **基于注解的开发简洁高效**（如 `@Controller`, `@RequestMapping`, `@RestController`）

   - **支持 RESTful 风格 URL 路由**

   - **与 Spring 框架无缝整合**（如依赖注入、事务管理）

   - **灵活的视图解析**（支持 JSP、FreeMarker、Thymeleaf、JSON 等）

   - **拦截器支持**，可用于登录验证、权限控制等横切逻辑

   - **良好的扩展性与插件机制**

4. **使用场景**

   - 构建传统 Web 页面（如后台管理系统）

   - 构建 REST API 接口（结合 `@RestController` 和 JSON 序列化）

   - 与前端分离式开发（如前后端分离架构：Vue + SpringBoot）

### 16. SpringMVC 主要有几部分组成，简单描述 SpringMVC 的工作流程？

<u>SpringMVC 的主要组成部分</u>（7 个核心组件）：

| 组件名                                 | 简要说明                                         |
| -------------------------------------- | ------------------------------------------------ |
| **1. DispatcherServlet（前端控制器）** | 核心调度器，所有请求先到它，由它统一分发。       |
| **2. HandlerMapping（处理器映射器）**  | 根据请求 URL 匹配对应的控制器（Handler）。       |
| **3. Handler（处理器）**               | 即 `@Controller` 类中的方法，真正处理业务逻辑。  |
| **4. HandlerAdapter（处理器适配器）**  | 调用具体的 Handler 方法，并封装结果。            |
| **5. ModelAndView（模型和视图）**      | Handler 返回的对象，包含模型数据和视图名。       |
| **6. ViewResolver（视图解析器）**      | 将视图名解析为具体的视图（如 JSP 页面或 JSON）。 |
| **7. View（视图）**                    | 负责渲染页面或数据，最终返回给用户。             |

<u>Spring MVC的工作流程如下</u>：

1. **用户发起请求**：
   浏览器或客户端发送请求（如 `/user/list`）到服务器。
2. **DispatcherServlet 接收请求**：
   所有请求首先被 `DispatcherServlet` 拦截，这是 SpringMVC 的入口。
3. **查找 HandlerMapping**：
   `DispatcherServlet` 根据请求 URL，向 `HandlerMapping` 查找匹配的 `Controller` 方法。
4. **找到 Handler 并调用适配器**：
   找到目标方法后，通过 `HandlerAdapter` 调用对应的 `@Controller` 方法。
5. **执行 Controller 方法**：
   控制器执行业务逻辑，返回一个 `ModelAndView` 对象（包含数据和视图名）。
6. **视图解析器处理视图名**：
   `DispatcherServlet` 调用 `ViewResolver` 把逻辑视图名（如 `"userList"`）解析为具体视图（如 `/WEB-INF/views/userList.jsp` 或 JSON）。
7. **视图渲染并响应客户端**：
   由视图对象（如 JSP 引擎或 JSON 序列化）完成最终页面或数据渲染，响应返回给用户。

### 17. 到目前为止，你共学习了哪些注解？请总结列举，越多越好

#### 一、Spring 核心注解

| 注解             | 作用                                     |
| ---------------- | ---------------------------------------- |
| `@Component`     | 标注为组件类，由 Spring 容器管理         |
| `@Controller`    | 标注为控制器类，处理 Web 请求            |
| `@Service`       | 标注为服务层组件                         |
| `@Repository`    | 标注为持久层组件，自动处理异常转换       |
| `@Autowired`     | 自动注入依赖（按类型注入）               |
| `@Qualifier`     | 与 `@Autowired` 配合，按名称注入         |
| `@Value`         | 注入配置文件中的值                       |
| `@Bean`          | 定义一个 Bean 实例，常用于 Java 配置类中 |
| `@Configuration` | 表示当前类是配置类（替代 XML 配置）      |
| `@Import`        | 引入其他配置类                           |
| `@ComponentScan` | 指定要扫描的包路径                       |
| `@Lazy`          | 延迟初始化 Bean                          |
| `@Primary`       | 当有多个候选 Bean 时优先使用             |

#### 二、Spring Boot 注解

| 注解                                                        | 作用                                                         |
| ----------------------------------------------------------- | ------------------------------------------------------------ |
| `@SpringBootApplication`                                    | 包含 `@Configuration`、`@ComponentScan`、`@EnableAutoConfiguration` |
| `@EnableAutoConfiguration`                                  | 启用 Spring Boot 自动配置                                    |
| `@ConfigurationProperties`                                  | 将配置文件绑定为对象                                         |
| `@RestController`                                           | 相当于 `@Controller + @ResponseBody`，用于 REST 接口         |
| `@RequestMapping`                                           | 映射请求路径                                                 |
| `@GetMapping / @PostMapping / @PutMapping / @DeleteMapping` | 映射 HTTP 请求方式                                           |
| `@PathVariable`                                             | 获取 URL 中的路径参数                                        |
| `@RequestParam`                                             | 获取请求参数（如 ?id=1）                                     |
| `@RequestBody`                                              | 接收 JSON 数据并转换为 Java 对象                             |
| `@ResponseBody`                                             | 将方法返回值直接作为响应体                                   |
| `@CrossOrigin`                                              | 解决跨域请求问题                                             |

#### 三、Spring AOP / 事务相关注解

| 注解                                                         | 作用                                   |
| ------------------------------------------------------------ | -------------------------------------- |
| `@Transactional`                                             | 声明事务（可配置传播行为、隔离级别等） |
| `@EnableTransactionManagement`                               | 启用基于注解的事务管理                 |
| `@Aspect`                                                    | 声明一个切面类                         |
| `@Before / @After / @Around / @AfterReturning / @AfterThrowing` | AOP 通知类型                           |
| `@Pointcut`                                                  | 定义切点表达式                         |

#### 四、Spring Security 常用注解

| 注解                       | 作用                                 |
| -------------------------- | ------------------------------------ |
| `@EnableWebSecurity`       | 启用 Spring Security 安全配置        |
| `@PreAuthorize`            | 方法调用前进行权限校验（基于表达式） |
| `@Secured`                 | 基于角色的权限控制                   |
| `@RolesAllowed`            | 基于角色控制访问权限（JDK 标准）     |
| `@AuthenticationPrincipal` | 获取当前登录用户对象                 |

#### 五、JPA / Hibernate 注解（ORM 映射）

| 注解                                                      | 作用               |
| --------------------------------------------------------- | ------------------ |
| `@Entity`                                                 | 实体类映射数据库表 |
| `@Table(name="...")`                                      | 映射数据库表名     |
| `@Id`                                                     | 主键字段           |
| `@GeneratedValue`                                         | 主键生成策略       |
| `@Column(name="...")`                                     | 映射数据库列       |
| `@OneToMany` / `@ManyToOne` / `@OneToOne` / `@ManyToMany` | 关系映射注解       |
| `@JoinColumn`                                             | 外键关联字段       |
| `@Transient`                                              | 不映射到数据库字段 |
| `@Enumerated`                                             | 枚举映射           |

#### 六、MyBatis 注解

| 注解                                    | 作用                                 |
| --------------------------------------- | ------------------------------------ |
| `@Mapper`                               | 标记 Mapper 接口，让 Spring 容器识别 |
| `@Select / @Insert / @Update / @Delete` | 编写 SQL 语句                        |
| `@Results` / `@Result`                  | 结果映射关系                         |
| `@Param`                                | 为方法参数指定名称，用于 SQL 中引用  |

#### 七、验证注解（JSR 303 / Hibernate Validator）

| 注解                    | 作用                                       |
| ----------------------- | ------------------------------------------ |
| `@NotNull`              | 值不能为 null                              |
| `@NotBlank`             | 字符串不能为空（不为空且去空格后长度 > 0） |
| `@Size(min=, max=)`     | 字符串或集合长度限制                       |
| `@Email`                | 邮箱格式验证                               |
| `@Pattern`              | 正则表达式验证                             |
| `@Min` / `@Max`         | 最小/最大值限制                            |
| `@Valid` / `@Validated` | 触发对象级别的校验                         |

#### 八、其他常用注解

| 注解                 | 作用                                     |
| -------------------- | ---------------------------------------- |
| `@Slf4j`（Lombok）   | 自动生成 `log` 对象                      |
| `@Data`（Lombok）    | 自动生成 getter/setter、equals、toString |
| `@Builder`（Lombok） | 支持链式构建对象                         |
| `@PostConstruct`     | Bean 初始化后执行方法                    |
| `@PreDestroy`        | Bean 销毁前执行方法                      |
| `@Scheduled`         | 定时任务执行方法                         |
| `@Async`             | 异步执行方法                             |

