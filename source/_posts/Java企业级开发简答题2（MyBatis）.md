---
title: Java企业级开发简答题2（MyBatis）
date: 2025-07-11 15:11:42
tags:
  - Java
  - Spring
  - 后端
categories:
  - Java企业级开发
---

# 作业 2：简答题

> 22371495 吴自强

[TOC]

### 1. 简述 MyBatis 的工作原理

<u>MyBatis 的**工作原理**</u>可以概括如下：

**一、配置阶段**

1. **加载配置文件**
   MyBatis 启动时会加载 `mybatis-config.xml` 配置文件，其中包括数据库连接信息、Mapper 映射文件路径等。
2. **构建 SqlSessionFactory**
   通过 `SqlSessionFactoryBuilder` 读取配置文件并构建 `SqlSessionFactory`。这个工厂负责创建 `SqlSession`。

**二、运行阶段**

1. **创建 SqlSession**
   应用程序通过 `SqlSessionFactory` 创建一个 `SqlSession` 对象，用于执行 SQL。
2. **执行 SQL 映射**
   - 使用接口绑定（Mapper 接口）或者 XML 映射文件中的 SQL 语句。
   - MyBatis 会根据 `Mapper.xml` 中的 SQL 映射，将参数传入、生成最终 SQL，并执行。
3. **执行 JDBC 操作**
   MyBatis 底层仍然使用 JDBC 执行 SQL，通过 JDBC 与数据库交互。
4. **结果映射**
   SQL 执行后返回结果，MyBatis 会将结果集根据 `resultMap` 或自动映射机制转换为 Java 对象。

**三、关闭资源**

`SqlSession` 使用完毕后要关闭，以释放数据库连接资源。

### 2. 简述 MyBatis 和 Spring 的整合过程

1. 添加依赖（Maven）

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.3.1</version>
</dependency>
```

2. 配置数据库连接（`application.yml`）

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/testdb
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
```

3. 配置 MyBatis（可选）

```yaml
mybatis:
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.example.model
  configuration:
    map-underscore-to-camel-case: true
```

4. 编写 Mapper 接口 + XML 或注解

接口：

```java
@Mapper
public interface UserMapper {
    User selectById(int id);
}
```

或使用 `@MapperScan` 注解批量扫描：

```java
@SpringBootApplication
@MapperScan("com.example.mapper")
public class MyApp {}
```

XML（位于 `resources/mapper/UserMapper.xml`）：

```xml
<select id="selectById" resultType="User">
  SELECT * FROM users WHERE id = #{id}
</select>
```

### 3. 除 MyBatis 持久化框架外，您还知道哪些持久化框架？并说明他们的特点

#### 1. **Hibernate**

> 全功能的 ORM 框架，JPA 的重要实现。

特点：

- **全自动 ORM 映射**：可以自动将数据库表映射成 Java 对象。
- **支持 HQL（Hibernate Query Language）**：比 SQL 更面向对象。
- **支持懒加载、缓存、事务管理等高级特性**。
- 与 Spring 整合紧密，支持 JPA 注解。

适用场景：

- 需要复杂对象关系映射、大量实体管理的企业系统。

#### 2. **JPA（Java Persistence API）**

> Java 官方定义的 ORM 标准接口，Hibernate、EclipseLink 都是其实现。

特点：

- **标准统一接口**，降低耦合度。
- 提供 **注解驱动的数据映射**。
- 支持 JPQL 查询语言。
- **易与 Spring Data JPA 整合**。

适用场景：

- 需要与不同 ORM 框架解耦或追求官方规范兼容性的项目。

#### 3. **Spring Data JPA**

> 是对 JPA 的进一步封装，属于 Spring Data 家族。

特点：

- **简化 DAO 层开发**：只写接口就能自动实现 CRUD。
- 支持方法名自动解析为查询语句（如 `findByUsernameAndAge`）。
- 集成分页、排序、动态查询等功能。

适用场景：

- 业务模型以实体类为主、CRUD 操作多的项目。

#### 4. **Apache iBATIS（MyBatis 前身）**

> 早期的 SQL 映射框架，MyBatis 是它的分支。

特点：

- 允许开发者手写 SQL，控制力强。
- 已较少使用，建议用 MyBatis 代替。

#### 5. **jOOQ**

> 面向 SQL 的类型安全 DSL（领域特定语言）框架。

特点：

- 将 SQL 编写为 Java 代码，具有编译时语法检查。
- 适合复杂查询，类型安全。
- 更贴近数据库开发人员思维。

适用场景：

- 注重 SQL 灵活性与类型安全的系统，或需要动态构造复杂 SQL 的项目。

#### 6. **Spring Data JDBC**

> 类似 JPA，但更轻量，基于 JDBC + Spring。

特点：

- 不使用复杂的 ORM 功能，仅支持简单的实体关系。
- 更易调试，性能更可控。
- 避免了懒加载和复杂映射问题。

适用场景：

- 项目简单，追求可预测行为、轻量的系统。

---

总结对比（核心点）：

| 框架                 | 是否 ORM | 控制力 | 上手难度 | 自动化程度 | 适合场景                           |
| -------------------- | -------- | ------ | -------- | ---------- | ---------------------------------- |
| **Hibernate**        | ✅        | 中     | 中等     | 高         | 复杂业务模型，标准 ORM             |
| **JPA**              | ✅        | 中     | 中等     | 高         | 遵循标准规范的企业系统             |
| **Spring Data JPA**  | ✅        | 低     | ✅ 简单   | ✅ 很高     | 快速开发、常规 CRUD 项目           |
| **MyBatis**          | ❌        | ✅ 高   | 中等     | 中         | 需要手写 SQL，适合灵活控制         |
| **jOOQ**             | ❌        | ✅ 高   | 高       | 中         | 动态 SQL、类型安全、数据库驱动项目 |
| **Spring Data JDBC** | ❌        | ✅ 高   | ✅ 简单   | 中         | 简单系统、避免 ORM 复杂性的项目    |

### 4. MyBatis 实现查询时，返回的结果集有几种常见的存储方式？请举例说明。

##### 1. 返回 **单个对象**

**说明：**

当查询结果只有一条记录时，可以直接映射成一个 Java 对象。

**示例：**

```java
User getUserById(int id);
<select id="getUserById" resultType="com.example.User">
    SELECT id, username, email FROM users WHERE id = #{id}
</select>
```

##### 2. 返回 **对象列表（List）**

**说明：**

当查询结果是多条记录时，可以映射为一个对象列表。

**示例：**

```java
List<User> getAllUsers();
<select id="getAllUsers" resultType="com.example.User">
    SELECT id, username, email FROM users
</select>
```

##### 3. 返回 **Map**

**方式一：一行结果映射为一个 Map（列名 → 值）**

```java
List<Map<String, Object>> getAllUsersAsMap();
<select id="getAllUsersAsMap" resultType="map">
    SELECT id, username, email FROM users
</select>
```

返回结果示例：

```java
[
  {id=1, username="Alice", email="alice@example.com"},
  {id=2, username="Bob", email="bob@example.com"}
]
```

**方式二：多行结果以某列为 key 映射成一个大 Map**

```java
@MapKey("id")
Map<Integer, User> getUsersMappedById();
<select id="getUsersMappedById" resultType="com.example.User">
    SELECT id, username, email FROM users
</select>
```

返回结果示例：

```java
{
  1: User{id=1, username="Alice", ...},
  2: User{id=2, username="Bob", ...}
}
```

##### 4. 返回 **嵌套对象（多表联合查询）**

**说明：**

用于一对一、一对多等嵌套结构，需要使用 `resultMap`。

**示例：用户和地址一对一**

```java
User getUserWithAddress(int id);
<resultMap id="userMap" type="User">
    <id property="id" column="id"/>
    <result property="username" column="username"/>
    <association property="address" javaType="Address">
        <result property="city" column="city"/>
        <result property="zipcode" column="zipcode"/>
    </association>
</resultMap>

<select id="getUserWithAddress" resultMap="userMap">
    SELECT u.id, u.username, a.city, a.zipcode
    FROM users u
    JOIN address a ON u.id = a.user_id
    WHERE u.id = #{id}
</select>
```

##### 5. 返回 **基本类型或包装类型**

**说明：**

当查询结果只有单个字段时，可直接映射为 `String`、`Integer` 等基本类型。

**示例：**

```java
List<String> getAllUsernames();
<select id="getAllUsernames" resultType="string">
    SELECT username FROM users
</select>
```

### 5. 在 MyBatis 中针对不同的数据库软件，\<insert>元素如何将主键回填？

#### 一、使用 `useGeneratedKeys`

推荐用于 MySQL、H2 等支持 JDBC 自动生成主键的数据库,适用于：

- MySQL、SQL Server（部分版本）、H2
- 要求数据库驱动支持 `getGeneratedKeys()`

示例：

```xml
<insert id="insertUser" useGeneratedKeys="true" keyProperty="id">
    INSERT INTO users (username, email)
    VALUES (#{username}, #{email})
</insert>
```

参数说明：

- `useGeneratedKeys="true"`：表示使用 JDBC 的自动生成主键功能。
- `keyProperty="id"`：表示将生成的主键回填到 Java 对象的 `id` 属性中。

Java 实体类：

```java
public class User {
    private Integer id;  // 数据库自增主键
    private String username;
    private String email;
    // ...getter/setter...
}
```

#### 二、使用 `selectKey` 子元素（适用于 Oracle、PostgreSQL、DB2 等）

适用于：

- 不支持 JDBC `getGeneratedKeys()` 的数据库
- 通常使用数据库函数/序列，如 Oracle 的 `SEQUENCE`

**Oracle 示例（插入前获取主键）：**

```xml
<insert id="insertUser" parameterType="User">
    <selectKey keyProperty="id" resultType="int" order="BEFORE">
        SELECT user_seq.NEXTVAL FROM dual
    </selectKey>

    INSERT INTO users (id, username, email)
    VALUES (#{id}, #{username}, #{email})
</insert>
```

- `keyProperty="id"`：回填到 Java 对象的 `id` 字段
- `order="BEFORE"`：在执行 `INSERT` 语句 **之前** 先执行 `selectKey`

**PostgreSQL 示例（插入后获取主键）：**

```xml
<insert id="insertUser" parameterType="User">
    INSERT INTO users (username, email)
    VALUES (#{username}, #{email});

    <selectKey keyProperty="id" resultType="int" order="AFTER">
        SELECT currval('users_id_seq')
    </selectKey>
</insert>
```

### 6. 在 MyBatis 中，如何给 SQL 语句传递参数？

##### 1. **单个参数**

**示例：**

Mapper 接口：

```java
User getUserById(int id);
```

对应 Mapper XML：

```xml
<select id="getUserById" resultType="User">
    SELECT * FROM users WHERE id = #{id}
</select>
```

> 使用 `#{id}` 绑定参数，MyBatis 会自动将方法参数与 SQL 中的占位符绑定。

##### 2. **多个参数**

Java 方法的多个参数，MyBatis 默认会使用 **param1、param2、...** 来命名，或者你可以使用 `@Param` 注解命名。

```java
User getUserByUsernameAndEmail(@Param("username") String username,
                               @Param("email") String email);
<select id="getUserByUsernameAndEmail" resultType="User">
    SELECT * FROM users
    WHERE username = #{username} AND email = #{email}
</select>
```

> 💡 `#{}` 中的名字必须和 `@Param("xxx")` 对应。

##### 3. **传递 JavaBean 或 Map 类型参数**

方式一：JavaBean

```java
int insertUser(User user);
<insert id="insertUser">
    INSERT INTO users (username, email)
    VALUES (#{username}, #{email})
</insert>
```

> 直接使用 JavaBean 的属性名作为占位符：`#{属性名}`

------

方式二：Map

```java
User getUser(Map<String, Object> params);
<select id="getUser" resultType="User">
    SELECT * FROM users WHERE username = #{username} AND email = #{email}
</select>
```

> 用法与 JavaBean 一致，都是 `#{key}`

##### 4. **集合参数（用于 `IN` 查询）**

当传入的是集合或数组，用于 SQL 的 `IN` 子句时，需要使用 `foreach` 标签。示例：

```java
List<User> getUsersByIds(@Param("ids") List<Integer> ids);
<select id="getUsersByIds" resultType="User">
    SELECT * FROM users
    WHERE id IN
    <foreach collection="ids" item="id" open="(" separator="," close=")">
        #{id}
    </foreach>
</select>
```

##### 5. `#{} ` vs `${}` 的区别

| 写法  | 说明                        |
| ----- | --------------------------- |
| `#{}` | 安全预编译参数，防 SQL 注入 |
| `${}` | 字符串拼接，容易 SQL 注入   |

### 7. Maven 和 ANT 的区别

Maven 的特点：

- **约定优于配置**：只要遵循 Maven 的目录规范，就能减少配置。
- **内置生命周期**：比如 `compile`, `test`, `package`, `install`，统一标准。
- **依赖管理强大**：自动下载并维护依赖的版本、传递依赖。
- **适合大型项目/多模块项目**。

Ant 的特点：

- **自由度高**：构建过程由开发者完全控制。
- **像写脚本一样灵活**：适合非标准构建流程。
- **需要手动管理依赖**，如拷贝 `.jar` 文件到 `lib/`。
- **适合简单项目或自定义流程场景**。

| 维度               | Maven                                  | Ant                                  |
| ------------------ | -------------------------------------- | ------------------------------------ |
| **构建方式**       | 声明式（Declarative）                  | 命令式（Procedural）                 |
| **配置文件**       | `pom.xml`（使用 XML 描述项目结构）     | `build.xml`（使用 XML 编写构建脚本） |
| **依赖管理**       | 自动依赖管理（通过 Maven 仓库）        | 无内建依赖管理（需手动下载 JAR 包）  |
| **标准化目录结构** | 有固定的标准结构（如 `src/main/java`） | 无标准结构，自定义项目目录           |
| **插件支持**       | 丰富的插件体系（生命周期集成）         | 也有插件，但通常是手动调用任务       |
| **构建生命周期**   | 内置生命周期（compile, test, package） | 无生命周期概念，任务完全手动组合     |
| **学习曲线**       | 相对陡峭，依赖抽象概念                 | 相对平缓，像写脚本一样简单           |
| **可维护性**       | 高（统一结构、自动化强）               | 低（构建流程复杂时难维护）           |
| **扩展能力**       | 强，社区生态活跃                       | 较弱，需要自定义任务实现             |

### 8. 什么是 Spring Boot Stater ？

**Spring Boot Starter** 是一种 **自动化配置依赖模块**，它封装了一组相关功能所需的依赖，开发者只需引入一个 Starter，就能快速拥有该功能。Starter 的本质就是一个 **Maven 依赖包（POM 文件）**，内部通过 `<dependency>` 聚合多个相关依赖，起到了“模块入口”的作用。

**Spring Boot Starter 是一种约定好的依赖集合**，通过它可以“一键接入”各种 Spring 功能，省去了繁琐的配置和依赖管理，是 Spring Boot 实现自动配置与快速开发的基础设施。

### 9. Springboot 如何集成 MyBatis？

Spring Boot 集成 MyBatis 通常通过官方提供的 `mybatis-spring-boot-starter` 来简化配置。完整的集成步骤和示例如下：

##### 1. 添加依赖

在 `pom.xml` 中加入 MyBatis Starter 和数据库驱动依赖（以 MySQL 为例）：

```xml
<dependencies>
    <!-- Spring Boot Starter -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>

    <!-- MyBatis Spring Boot Starter -->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>2.3.0</version> <!-- 根据实际版本调整 -->
    </dependency>

    <!-- MySQL 驱动 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

##### 2. 配置数据源

在 `application.properties` 或 `application.yml` 配置数据库连接：

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/testdb?useSSL=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

##### 3. 编写 Mapper 接口和 XML

- **Mapper 接口**

```java
@Mapper
public interface UserMapper {
    User getUserById(Integer id);
}
```

- **Mapper XML**（放在 `resources/mapper/UserMapper.xml`）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.example.mapper.UserMapper">
    <select id="getUserById" resultType="com.example.model.User">
        SELECT id, username, email FROM users WHERE id = #{id}
    </select>
</mapper>
```

##### 4. Mapper 扫描配置（可选）

如果 Mapper 接口上没有使用 `@Mapper` 注解，可以在主启动类或者配置类添加：

```java
@MapperScan("com.example.mapper")
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

##### 5. 使用 Mapper

在 Service 或 Controller 中注入并调用：

```java
@Service
public class UserService {
    @Autowired
    private UserMapper userMapper;

    public User getUserById(Integer id) {
        return userMapper.getUserById(id);
    }
}
```

##### 6. 常用配置（application.properties）

```properties
# 指定 Mapper XML 文件路径（默认是 classpath:mapper/*.xml）
mybatis.mapper-locations=classpath:mapper/*.xml

# 是否打印 SQL
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl

# 驼峰命名转换
mybatis.configuration.map-underscore-to-camel-case=true
```

