---
title: Spring boot 起步
date: '2018-12-27'
tags:
  - Java
  - spring-boot
  - Gradle
category: backend
---

# 前言


## 安装 Gradle

从[这里](https://gradle.org/next-steps/?version=5.0&format=all)下载最新版的 Gradle，解压，把`path/to/Gradle/bin`加入环境变量。

测试是否安装成功。

```shell
gradle -v
 ```

## 使用 wrapper

Gradle wrapper 官方推荐使用[^2]
```shell
 gradle wrapper
 ```


 [^1]: https://spring.io/guides/gs/spring-boot/
 [^2]: https://docs.gradle.org/5.0/userguide/gradle_wrapper.html


## 简单的 web 应用

`src/main/java/hello/HelloController.java`

```java
package hello;

import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;

@RestController
public class HelloController {

    @RequestMapping("/")
    public String index() {
        return "Greetings from Spring Boot!";
    }

}
```

`src/main/java/hello/Application.java`

```java
package hello;

import java.util.Arrays;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    public CommandLineRunner commandLineRunner(ApplicationContext ctx) {
        return args -> {

            System.out.println("Let's inspect the beans provided by Spring Boot:");

            String[] beanNames = ctx.getBeanDefinitionNames();
            Arrays.sort(beanNames);
            for (String beanName : beanNames) {
                System.out.println(beanName);
            }

        };
    }

}
```

运行

```shell
./gradlew build && java -jar build/libs/gs-spring-boot-0.1.0.jar
```

访问`localhost:8080`就会收到`Greetings from Spring Boot!`消息。
