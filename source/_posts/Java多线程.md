---
title: Java 多线程
date: 2025-06-13 12:19:00
tags:
    - 后端
    - Java
    - Java多线程
categories:
    - 后端
---

## **Java多线程**

### Java SE中的多线程

Java SE中最基础的多线程方式（适用于任何Java程序）包括两种：

- **方法1：继承 Thread 类**

  ```java
  public class MyThread extends Thread {
      @Override
      public void run() {
          System.out.println("线程名：" + Thread.currentThread().getName());
      }
  }
  ```

  使用：

  ```java
  public class Main {
      public static void main(String[] args) {
          MyThread t1 = new MyThread();
          t1.start();  // 启动线程，run() 方法会被自动调用
      }
  }
  ```

- **方法2：实现 Runnable 接口**

  ```java
  public class MyRunnable implements Runnable {
      @Override
      public void run() {
          System.out.println("线程名：" + Thread.currentThread().getName());
      }
  }
  ```

  使用：

  ```java
  public class Main {
      public static void main(String[] args) {
          Thread t = new Thread(new MyRunnable());
          t.start();
      }
  }
  ```

---

此外，还可以使用线程池。Java 提供 `ExecutorService` 接口，Spring Boot 中也常用线程池来管理并发任务。

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ThreadPoolExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(3); // 创建固定线程数的线程池

        Runnable task = () -> {
            System.out.println("线程：" + Thread.currentThread().getName());
        };

        executor.submit(task);
        executor.shutdown(); // 关闭线程池（不立即关闭，会等待任务完成）
    }
}
```

### Spring Boot 中使用多线程

Spring Boot 提供了更加优雅的方式来使用多线程：

#### 方法1. 使用 `@Async` 异步方法（简单高效）

第一步：启动类或配置类上加 `@EnableAsync`

```java
@SpringBootApplication
@EnableAsync  // 启用异步支持
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
    }
}
```

第二步：在你要异步执行的方法上加 `@Async`

```java
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Service;

@Service
public class MyTaskService {

    @Async
    public void doTask() {
        System.out.println("开始执行任务，线程名：" + Thread.currentThread().getName());
    }
}
```

第三步：调用异步方法

```java
@RestController
public class TaskController {

    @Autowired
    private MyTaskService taskService;

    @GetMapping("/run")
    public String runTask() {
        taskService.doTask();  // 异步执行，不阻塞主线程
        return "任务已提交";
    }
}
```

#### 方法2. 自定义线程池配置（高级）

```java
@Configuration
@EnableAsync
public class AsyncConfig {

    @Bean(name = "customExecutor")
    public Executor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);       // 核心线程数
        executor.setMaxPoolSize(10);       // 最大线程数
        executor.setQueueCapacity(100);    // 队列容量
        executor.setThreadNamePrefix("MyExecutor-");
        executor.initialize();
        return executor;
    }
}
```

然后在 `@Async` 中指定线程池：

```java
@Async("customExecutor")
public void doTask() {
    System.out.println("执行任务，线程：" + Thread.currentThread().getName());
}
```

具体在我们的大作业中，我们配置了提交答案和状态更新两个线程池，保证大量并发请求的执行响应速度。

#### 线程池 vs @Async

| 场景                           | 推荐做法                     |
| ------------------------------ | ---------------------------- |
| 简单异步处理（发邮件、发短信） | `@Async`                     |
| 控制线程数、可复用线程资源     | 自定义线程池                 |
| 定时任务                       | 使用 `@Scheduled`            |
| 批量并发任务，需获取返回值     | `CompletableFuture + @Async` |
