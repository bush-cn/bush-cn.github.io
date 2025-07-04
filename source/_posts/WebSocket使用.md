---
title: WebSocket使用
date: 2025-06-13 12:23:13
tags:
    - 后端
    - WebSocket
    - Java
    - js
categories:
    - 后端
---

## **WebSocket**

WebSocket 是一种**基于 TCP 协议**的**全双工通信协议**，它允许客户端和服务器之间建立持久的、双向的通信连接。相比传统的 HTTP 请求 - 响应模式，WebSocket 提供了实时、低延迟的数据传输能力。通过 WebSocket，客户端和服务器可以**随时发送数据**，而不需要每次都重新建立连接。实现**实时更新和即时通信**的功能。

WebSocket 协议经过了多个浏览器和服务器的支持，成为了现代 Web 应用中常用的通信协议之一。它广泛应用于聊天应用、实时数据更新、多人游戏等场景，为 Web 应用提供了更好的用户体验和更高效的数据传输方式。

| 特点                 | 描述                                           |
| -------------------- | ---------------------------------------------- |
| **持久连接**         | 建立一次连接后，客户端和服务器之间保持连接状态 |
| **全双工通信**       | 客户端和服务器都可以主动发送消息               |
| **低延迟、实时性强** | 适合聊天、游戏、股票、实时推送等应用           |
| **节省资源**         | 与传统 HTTP 请求相比，减少了请求头等开销       |
| **基于 TCP 协议**    | 和 HTTP 一样基于 TCP，但是一个独立协议         |

### Spring Boot中使用WebSocket

以我们组的大作业为例，在学生答题端和教师监考端之间建立了WebSocket连接，实现答题进度及考试状态实时通信。具体步骤如下：

#### 1. 引入依赖

在`pom.xml`中引入依赖：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-websocket</artifactId>
    </dependency>
</dependencies>
```

#### 2. 创建配置类

```java
package com.buaa.javahuikao.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.messaging.simp.config.MessageBrokerRegistry;
import org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker;
import org.springframework.web.socket.config.annotation.StompEndpointRegistry;
import org.springframework.web.socket.config.annotation.WebSocketMessageBrokerConfigurer;

@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

    @Override
    public void configureMessageBroker(MessageBrokerRegistry config) {
        config.enableSimpleBroker("/topic"); // 客户端订阅地址前缀
        config.setApplicationDestinationPrefixes("/app"); // 服务端接收消息地址前缀
    }

    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/ws") // WebSocket连接端点
                .setAllowedOriginPatterns("*")
                .withSockJS();
    }
}
```

- `enableSimpleBroker("/topic")`：启用了一个 **简单的内存消息代理（message broker）**，当客户端订阅 `/topic/*` 开头的地址时，消息将会被推送到这些订阅者。

- `setApplicationDestinationPrefixes("/app")`：客户端向服务端 **发送消息的路径前缀**。只有以 `/app` 开头的消息，才会被 `@MessageMapping` 注解的方法接收处理。

- `addEndpoint("/ws")`：注册一个 **WebSocket 连接端点**，客户端将通过这个地址发起连接（例如：`ws://localhost:8080/ws`）。

- `setAllowedOriginPatterns("*")`：允许所有来源跨域连接（生产环境建议指定具体域名）。

  `withSockJS()`：启用 **SockJS 兼容模式**，保证在 WebSocket 不可用时（如浏览器不支持）可以回退到 AJAX 长轮询。

#### 3. 在REST接口类中使用WebSocket

首先注入 `SimpMessagingTemplate`：

```java
private final SimpMessagingTemplate messagingTemplate;

@Autowired
public InvigilationController(SimpMessagingTemplate messagingTemplate) {
    this.messagingTemplate = messagingTemplate;
}
```

- `SimpMessagingTemplate` 是 Spring 提供的一个工具类，用于在服务端向客户端 **发送 STOMP 消息**。
- 可以理解为是一个“消息发送器”，配合前面的 `WebSocketConfig` 使用。

接着，使用`messagingTemplate.convertAndSend(destination, payload)`向服务端推送消息。此处，

#### 4. 前端订阅消息并更新页面

```js
//建立WebSocket连接
const socketProgress = new SockJS("http://localhost:8081/ws");
const stompClientProgress = Stomp.over(socketProgress);
stompClientProgress.connect({}, () => {
    console.log("WebSocket Progress 连接成功！");
    const examId = exam_id
    const subscription = stompClientProgress.subscribe(
        `/topic/exam/${examId}/progress`,
        (message) => {
            const data = JSON.parse(message.body);
            console.log("收到进度更新:", data);
            updateStudentProgress(data.studentId, data.progress);
        }
    );
});

//建立WebSocket连接
const socketStatus = new SockJS("http://localhost:8081/ws");
const stompClientStatus = Stomp.over(socketStatus);
stompClientStatus.connect({}, () => {
    console.log("WebSocket Status 连接成功！");
    const examId = exam_id
    const subscription = stompClientStatus.subscribe(
        `/topic/exam/${examId}/status`,
        (message) => {
            const data = JSON.parse(message.body);
            console.log("收到状态更新:", data);
            updateStudentStatus(data.studentId, data.status, data.description);
        }
    );
});
```

