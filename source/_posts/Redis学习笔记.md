---
title: Redis学习笔记
date: 2025-06-13 12:17:00
tags:
    - 后端
    - Redis
    - Java
categories:
    - 后端
---

# Redis学习笔记

## **Redis**

### Redis简介

Redis（**Remote Dictionary Server**）是一种**开源的、基于内存的键值对（key-value）数据库**，可以用作数据库、缓存和消息中间件。它以**极高的性能、丰富的数据结构和多种实用功能**而著称，被广泛用于高并发、低延迟的应用场景中，如电商秒杀、社交应用、排行榜、会话缓存等。

#### Redis特点

| 特点                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| **高性能**             | 所有数据操作都在内存中完成，读写速度极快（百万级QPS）。      |
| **丰富数据结构**       | 支持多种数据类型：字符串（string）、列表（list）、集合（set）、有序集合（zset）、哈希表（hash）、位图、HyperLogLog、地理位置等。 |
| **持久化**             | 虽然是内存数据库，但支持将数据持久化到磁盘，防止宕机数据丢失（RDB快照和AOF日志）。 |
| **支持事务**           | 可以一次执行多个命令，具备基本的事务特性（MULTI/EXEC）。     |
| **支持发布/订阅**      | 可实现消息系统的 pub/sub 通信模型。                          |
| **支持主从复制与集群** | 具备高可用（哨兵）与分布式（Redis Cluster）能力。            |
| **原子操作**           | 所有单条 Redis 命令都是原子的，适合做计数器、分布式锁等。    |

#### Redis的典型应用场景

- 页面缓存（如商品详情页、热点新闻）
- Session存储（分布式登录状态维护）
- 秒杀系统（抢购计数、库存控制）
- 排行榜系统（ZSet）
- 实时消息推送（Pub/Sub）
- 分布式锁（基于setnx等命令）

#### NoSQL

NoSQL（**Not Only SQL**）是一类**非关系型数据库系统**的统称，设计初衷是为了解决传统关系型数据库（如MySQL、Oracle）在高并发、大数据量、分布式存储等场景下的性能瓶颈。

NoSQL包括以下类型：

| 类型                  | 代表数据库        | 描述                                   | 典型用途                     |
| --------------------- | ----------------- | -------------------------------------- | ---------------------------- |
| **键值（Key-Value）** | **Redis**, Riak   | 以 key 映射 value，结构简单，性能极高  | 缓存、会话存储               |
| **文档型**            | MongoDB, CouchDB  | 类似 JSON 的文档结构，灵活支持嵌套字段 | 内容管理、用户信息存储       |
| **列族型（宽列）**    | HBase, Cassandra  | 按列存储数据，适合大数据分析           | 日志系统、数据仓库           |
| **图数据库**          | Neo4j, JanusGraph | 用节点和边表示数据及其关系             | 社交网络、知识图谱、推荐系统 |

### Redis 数据类型

| 数据类型         | 说明                                                         | 常见用途             |
| ---------------- | ------------------------------------------------------------ | -------------------- |
| String（字符串） | 基本的数据存储单元，可以存储字符串、整数或者浮点数。         | 缓存、计数器         |
| Hash（哈希）     | 一个键值对集合，可以存储多个字段。                           | 用户信息、配置项     |
| List（列表）     | 一个简单的列表，可以存储一系列的字符串元素。                 | 队列、任务流         |
| Set（集合）      | 一个无序集合，可以存储不重复的字符串元素。                   | 标签、去重、社交关系 |
| ZSet（有序集合） | 类似于集合，但是每个元素都有一个分数与之关联。               | 排行榜、评分系统     |
| Bitmap（位图）   | 基于字符串类型，可以对每个位进行操作。                       | 签到、状态记录       |
| HyperLogLog      | 用于基数统计，可以估算集合中的唯一元素数量。                 | UV、去重统计         |
| GEO              | 用于存储地理位置信息。                                       | 附近的人、位置服务   |
| Pub/Sub          | 一种消息通信模式，允许客户端订阅消息通道，并接收发布到该通道的消息。 | 通知系统、聊天室     |

#### 1. String（字符串）

- **最基础的数据类型**，一个键对应一个字符串值，可以是文本或二进制数据（如图片、JSON、数字等）。

  > 一个键最大能存储 512MB

- 操作命令：

  ```bash
  SET key value	# 设置键的值
  GET key	# 获取键的值
  INCR key	# 将键的值加 1
  DECR key	# 将键的值减 1
  APPEND key value	# 将值追加到键的值之后
  ```

- **应用场景**：

  - 缓存文本信息
  - 计数器（如访问量）
  - 存储JSON（如用户token）

#### 2. Hash（哈希）

- 类似 Java 中的 Map 或 Python 的字典，一个键对应一个字段集合，每个字段有自己的值。

- 操作命令：

  ```bash
  HSET key field value	# 设置哈希表中字段的值。
  HGET key field	# 获取哈希表中字段的值。
  HGETALL key	# 获取哈希表中所有字段和值。
  HDEL key field	# 删除哈希表中的一个或多个字段。
  ```

- **应用场景**：

  - 存储对象（如用户、商品属性）
  - 节省内存（相比多个字符串键）

#### 3. List（列表）

- 一个键对应一个链表结构，支持两端插入（可作队列或栈）。

- 操作命令：

  ```bash
  LPUSH key value	# 将值插入到列表头部。
  RPUSH key value	# 将值插入到列表尾部。
  LPOP key	# 移出并获取列表的第一个元素。
  RPOP key	# 移出并获取列表的最后一个元素。
  LRANGE key start stop	# 获取列表在指定范围内的元素。
  ```

- **应用场景**：

  - 消息队列
  - 任务列表（如评论流、延迟队列）

#### 4. Set（集合）

- 无序、**不重复**的字符串集合。

- 操作命令：

  ```bash
  SADD key value	# 向集合添加一个或多个成员。
  SREM key value	# 移除集合中的一个或多个成员。
  SMEMBERS key	# 返回集合中的所有成员。
  SISMEMBER key value	# 判断值是否是集合的成员。
  ```

- **应用场景**：

  - 标签系统
  - 共同好友计算（交集）
  - 去重（如IP集合）

#### 5. Sorted Set（有序集合，ZSet）

- 类似 Set，但每个元素带一个分数（score，double型），按分数排序。

- 操作命令：

  ```bash
  ZADD key score value	# 向有序集合添加一个或多个成员，或更新已存在成员的分数。
  ZRANGE key start stop [WITHSCORES]	# 返回指定范围内的成员。
  ZREM key value	# 移除有序集合中的一个或多个成员。
  ZSCORE key value	# 返回有序集合中，成员的分数值。
  ```

- **应用场景**：

  - 排行榜（积分、排名）
  - 优先级队列
  - 带权重排序的数据列表

#### 6. Bitmap（二进制位图）

- 用二进制位表示布尔状态，每个位可设置为0或1，适用于大量布尔信息存储。

- 操作命令：

  ```bash
  SETBIT online_users 1234 1
  GETBIT online_users 1234
  BITCOUNT online_users
  ```

- **应用场景**：

  - 用户签到记录（某天是否在线）
  - 活跃用户统计
  - 登录状态记录

#### 7. HyperLogLog

- 用于**统计唯一元素个数**（近似值），但占用内存极小（12KB 固定）。

- 操作命令：

  ```bash
  PFADD uv_counter user1 user2
  PFCOUNT uv_counter
  ```

- **应用场景**：

  - 网站 UV 统计
  - 去重计数但不要求精确

#### 8. GEO（地理空间坐标）

- 存储经纬度坐标，并支持附近查询等操作。

- 操作命令：

  ```bash
  GEOADD locations 116.40 39.90 "Beijing"
  GEODIST locations "Beijing" "Shanghai"
  GEORADIUS locations 116.40 39.90 100 km
  ```

- **应用场景**：

  - 附近的人、附近的店
  - LBS（基于地理位置服务）

#### 9. Pub/Sub（发布订阅）

- Redis 的**消息系统**，支持发布-订阅机制。

- 操作命令：

  ```bash
  SUBSCRIBE news
  PUBLISH news "Redis 6.0 released"
  ```

- **应用场景**：

  - 实时消息推送
  - 聊天室系统
  - 日志采集系统

### Redis键

通用命令如下：

| 命令                 | 作用                        |
| -------------------- | --------------------------- |
| `SET key value`      | 设置键的值                  |
| `GET key`            | 获取键的值                  |
| `DEL key`            | 删除键                      |
| `EXISTS key`         | 判断键是否存在              |
| `EXPIRE key seconds` | 设置键的过期时间            |
| `TTL key`            | 查看剩余存活时间            |
| `KEYS pattern`       | 按模式查找键（如 `user:*`） |
| `TYPE key`           | 返回键的数据类型            |

键管理命令：

| 命令                       | 说明                 |
| -------------------------- | -------------------- |
| `EXPIRE key seconds`       | 设置过期时间（秒）   |
| `PEXPIRE key milliseconds` | 设置过期时间（毫秒） |
| `TTL key`                  | 查看剩余生存时间     |
| `PERSIST key`              | 移除过期时间         |
| `RENAME old new`           | 重命名键             |

### Redis事务

单个 Redis 命令的执行是原子性的，但 Redis 没有在事务上增加任何维持原子性的机制，所以 Redis 事务的执行并不是原子性的。

事务可以理解为一个打包的批量执行脚本，但批量指令并非原子化的操作，中间某条指令的失败不会导致前面已做指令的回滚，也不会造成后续的指令不做。

| 命令        | 说明                         |
| ----------- | ---------------------------- |
| `MULTI`     | 开启事务                     |
| `EXEC`      | 提交事务                     |
| `DISCARD`   | 放弃事务                     |
| `WATCH key` | 监视键值是否被其他客户端修改 |

### Redis连接与服务器

| 命令            | 说明                        |
| --------------- | --------------------------- |
| `AUTH password` | 验证密码是否正确            |
| `ECHO message`  | 打印字符串                  |
| `PING`          | 查看服务器是否运行          |
| `QUIT`          | 关闭当前连接                |
| `SELECT index`  | 切换到指定的数据库          |
| `INFO`          | 查看服务器信息和统计        |
| `MONITOR`       | 实时查看操作命令            |
| `FLUSHDB`       | 清空当前数据库              |
| `FLUSHALL`      | 清空所有数据库              |
| `DBSIZE`        | 返回当前数据库中 key 的数量 |

### Redis数据备份与恢复

| 命令           | 说明                 |
| -------------- | -------------------- |
| `SAVE`         | 同步保存快照（阻塞） |
| `BGSAVE`       | 异步保存快照         |
| `BGREWRITEAOF` | 重写 AOF 文件        |
| `LASTSAVE`     | 上次保存的时间       |

### Java（Spring Boot）中使用Redis

以我们组的大作业项目为例，我们采用Redis实现试卷数据的缓存，使得试卷在毫秒内加载完成。

#### 1. 添加依赖

在 pom.xml 文件中添加 Redis 相关依赖

```xml
<dependencies>
  <!-- Spring Data Redis -->
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
  </dependency>
</dependencies>
```

#### 2. 配置Redis连接信息

在`application.yml`中添加连接信息：

```yaml
spring:
  # ……其他配置
  # Redis配置
  data:
    redis:
      host: 127.0.0.1
      port: 6379
      lettuce:
        pool:
          max-active: 50  # 匹配线程池大小
          max-idle: 20
          min-idle: 5
```

#### 3. 使用RedisTemplate进行操作

创建配置类，其作用是定义一个自定义的 `RedisTemplate<String, Object>` Bean，指定序列化方式，以便在项目中可以更方便地使用 Redis 存储和读取对象数据。

```java
package com.buaa.javahuikao.config;

import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

import java.lang.runtime.ObjectMethods;

@Configuration
public class RedisConfig {
    @Bean
    public RedisTemplate<String,Object> redisTemplate(RedisConnectionFactory factory){
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(factory);
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new GenericJackson2JsonRedisSerializer());
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashValueSerializer(new GenericJackson2JsonRedisSerializer());
        redisTemplate.afterPropertiesSet();
        return redisTemplate;
    }
}
```

在`ExamService.java`中注入：

```java
@Resource
private RedisTemplate<String, Object> redisTemplate;
```

使用`redisTemplate.opsForValue().get(cacheKey)`从缓存中获取数据、`redisTemplate.opsForValue().set(cacheKey,jsonData)`向缓存中写入数据。此处使用Jackson的序列化工具`ObjectMapper`将对象与Json字符串相互转换，redis缓存中存的字符串值为序列化后的Json字符串，读取缓存后也需要反序列化为Java对象。

```java
package com.buaa.javahuikao.service;

import com.buaa.javahuikao.dto.StudentExamQuestionDTO;
import com.buaa.javahuikao.mapper.ExamQuestionMapper;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import jakarta.annotation.Resource;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisCallback;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Service;
import java.util.concurrent.TimeUnit;

@Service
public class ExamQuestionService {
    private static final String EXAM_QUESTIONS_KEY_PREFIX = "exam:questions:";
    private static final long CACHE_EXPIRE_HOURS = 3; // 缓存过期时间(小时)

    @Autowired
    private ExamQuestionMapper examQuestionMapper;
    @Resource
    private RedisTemplate<String, Object> redisTemplate;
    @Autowired
    private ObjectMapper objectMapper; //json处理器

    public StudentExamQuestionDTO getExamQuestions(int examId){
        String cacheKey = EXAM_QUESTIONS_KEY_PREFIX + examId;
        //尝试从缓存获取
        try{
            String cachedData = (String) redisTemplate.opsForValue().get(cacheKey);
            if(cachedData != null) {
                return objectMapper.readValue(cachedData,StudentExamQuestionDTO.class);
            }
        }
        catch(Exception e){
            System.out.println("redis缓存错误："+ e);
        }

        //缓存未命中
        StudentExamQuestionDTO questions = examQuestionMapper.getExamQuestions(examId);
        //写入缓存
        if (questions!=null){
            try{
                String jsonData = objectMapper.writeValueAsString(questions);
                redisTemplate.opsForValue().set(
                        cacheKey,
                        jsonData,
                        CACHE_EXPIRE_HOURS,
                        TimeUnit.HOURS
                );
            }
            catch(JsonProcessingException e){
                System.out.println("写入缓存错误："+ e);
            }
        }
        return questions;
    }

}
```
