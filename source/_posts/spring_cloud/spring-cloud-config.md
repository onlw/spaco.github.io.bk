---
title: Spring Cloud Config
permalink: spring-cloud-config
categories:
- program
tags: 
- spring-cloud
comments: true
date: 2019-09-01
updated: 2019-09-01
---

# Spring Cloud Config 

## Introduction

Spring Cloud Config为分布式系统中的外部化配置提供服务器端和客户端支持。使用Config Server，您可以在所有环境中管理应用程序的外部属性。客户端和服务器上的概念映射与Spring Environment和PropertySource抽象，因此它们非常适合Spring应用程序，但可以与任何语言运行的任何应用程序一起使用。当应用程序通过部署管道从开发到测试再到生产时，您可以管理这些环境之间的配置，并确保应用程序具有迁移时需要运行的所有内容。服务器存储后端的默认实现使用git，因此它可以轻松支持配置环境的标签版本，以及可用于管理内容的各种工具。添加替代实现并使用Spring配置插入它们很容易。

## Development Environment

- Spring Boot : 2.1.7
- gradle : 5.6
- Java : jdk1.8

## Key Words

- Config Server
- Config Client
- bootstrap.[yml | properties]
- zookeeper
- vault
- consul

## Build Config Server

### 配置Gradle

```bash
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.cloud:spring-cloud-config-server'
    implementation 'org.springframework.cloud:spring-cloud-function-web'
    implementation 'org.springframework.cloud:spring-cloud-starter'
    implementation 'org.springframework.cloud:spring-cloud-starter-task'
    compileOnly 'org.projectlombok:lombok'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    annotationProcessor 'org.springframework.boot:spring-boot-configuration-processor'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
```

### 配置 application.properties

```properties
server.port=8081
spring.cloud.config.server.git.uri=https://github.com/spaco/spring-cloud-config-repo-samples
```

git仓库文件如下：

```bash

```

### 配置启动器

`主要是@EnableConfigServer注解`

```java
@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {

	public static void main(String[] args) {
		SpringApplication.run(ConfigServerApplication.class, args);
	}

}
```

### 启动服务

### Test Cases

在测试之前，开发者需要知道Config Server寻找配置文件的规则

开发者请求 Config Server 的端点可以获取配置文件的内容，请求地址与配置文件的映射如下：

```bash
/{application}/{profile}[/{label}]
/{application}-{profile}.yml
/{label}/{application}-{profile}.yml
/{application}-{profile}.properties
/{label}/{application}-{profile}.properties
```

application  profile  label分别对应着什么呢？

- application 意味着项目名称，可以通过`spring.cloud.config.name`配置修改
- profile 意味着项目环境，测试服可以配置为`dev`，正式服可以配置为`pro`，可以通过`spring.cloud.config.profile`配置修改
- label 意味着git 分支版本，如果是使用的配置文件是本地，那么是无效的，可以通过`spring.cloud.config.label`配置修改，默认是 `master`

**获取配置文件我们采用两种方式，一种@Value,一种Environment**

#### Test 1 ：请求 application+profile

`curl localhost:8081/config-client/dev`

```properties
application = config-client
profile = dev
```

请求结果如下

```json
{
    "name": "config-client",
    "profiles": [
        "dev"
    ],
    "label": null,
    "version": "dd39228cf1b62a2a12f6afb26fc49dda0d8cd451",
    "state": null,
    "propertySources": [
        {
            "name": "https://github.com/spaco/spring-cloud-config-repo-samples/config-client-dev.properties",
            "source": {
                "filename": "config-client-dev.properties",
                "config-client-dev.properties": "config-client-dev.properties",
                "config-client-dev.unique": "config-client-dev.properties"
            }
        },
        {
            "name": "https://github.com/spaco/spring-cloud-config-repo-samples/config-client-dev.yml",
            "source": {
                "filename": "config-client-dev.yml",
                "config-client-dev.yml": "config-client-dev.yml",
                "config-client-dev.unique": "config-client-dev.yml"
            }
        },
        {
            "name": "https://github.com/spaco/spring-cloud-config-repo-samples/config-client.properties",
            "source": {
                "filename": "config-client.properties",
                "config-client.properties": "config-client.properties",
                "config-client.unique": "config-client.properties"
            }
        },
        {
            "name": "https://github.com/spaco/spring-cloud-config-repo-samples/config-client.yml",
            "source": {
                "filename": "config-client.yml",
                "config-client.yml": "config-client.yml",
                "config-client.unique": "config-client.yml"
            }
        }
    ]
}
```

总结：共获得 `config-client-dev.properties` `config-client-dev.yml` `config-client.properties` `config-client.yml` 的配置信息，不仅会得到配置信息，还存在 git 版本信息

#### Test 2 ：请求 application+profile指定文件 .yml | .properties

```bash
curl localhost:8081/config-client-dev.yml
```

请求结果如下：

```bash
config-client-dev:
  properties: config-client-dev.properties
  unique: config-client-dev.properties
  yml: config-client-dev.yml
config-client:
  properties: config-client.properties
  unique: config-client.properties
  yml: config-client.yml
filename: config-client-dev.properties

```

总结： 共获得 `config-client-dev.properties` `config-client-dev.yml` `config-client.properties` `config-client.yml` 的配置信息。**.properties 会覆盖 .yml 配置信息**，**请求指定文件功能无效**，当然，一般来说，不会存在 .yml 与 .properties共存的结果

## Build Config Client

当`application.properties`配置`spring.profiles.active=dev` 并且本地配置文件`application-dev.properties`存在，`application-dev.properties`配置的优先级是低于远程配置的，即会被覆盖

### 配置Gradle

```
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.cloud:spring-cloud-function-web'
    implementation 'org.springframework.cloud:spring-cloud-starter'
    implementation 'org.springframework.cloud:spring-cloud-starter-config'
    implementation 'org.springframework.cloud:spring-cloud-starter-task'
    compileOnly 'org.projectlombok:lombok'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    annotationProcessor 'org.springframework.boot:spring-boot-configuration-processor'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```



### 配置 application.properties

```properties
server.port=8082
```

### 配置 bootstrap.properties

**重点**：**关于 Spring Cloud Config 的配置需要在 `bootstrap.properties`中配置**

```properties
# 配置中心的具体地址，即 config-server
spring.cloud.config.uri=http://localhost:8081
# 对应 {application} 部分
spring.cloud.config.name=config-client
# 对应 {profile} 部分
spring.cloud.config.profile=dev
# 对应 {label} 部分，即 Git 的分支。如果配置中心使用的是本地存储，则该参数无用
spring.cloud.config.label=master
```

原因：

```
而现在我们把获取配置内容的 config client 相关配置放在了 application.properties 中，这样是不是就有点不对了，因为这里会出现一个先后顺序的问题。如果我们把配置放在了 application.properties 中，那么它会先去加载 bootstrap.properties中 的配置属性(如果没有，会加载默认配置)，假如我们在 application.properties 中配置的 uri 端口是 8081，那么它将不会被应用，还是默认的 8888 端口
```

```
Spring Cloud 有一个 引导上下文 的概念，它是主应用程序的父上下文，这个引导上下文负责从外部源（配置服务器，如Config Server）加载配置属性，及解密外部配置文件中的属性。主应用程序加载的是 application.(properties/yml) 中的属性，引导上下文加载 bootstrap.(properties/yml) 中的属性。这两个上下文共享一个Environment，但配置在 boostrap.* 中的属性有更高的优先级，因此默认情况下不能被本地配置覆盖。

设置 spring.cloud.bootstrap.enabled=false 可以禁用引导过程
```

**不在bootstrap.properties配置的错误示例**

```properties
# 将所有配置项 写入application.properties中
server.port=8082

# 配置中心的具体地址，即 config-server
spring.cloud.config.uri=http://localhost:8081
# 对应 {application} 部分
spring.cloud.config.name=config-client
# 对应 {profile} 部分
spring.cloud.config.profile=dev
# 对应 {label} 部分，即 Git 的分支。如果配置中心使用的是本地存储，则该参数无用
spring.cloud.config.label=master
```

项目编译时会报错：找不到目标文件，默认启动8888端口

```
c.c.c.ConfigServicePropertySourceLocator : Fetching config from server at : http://localhost:8888
c.c.c.ConfigServicePropertySourceLocator : Connect Timeout Exception on Url - http://localhost:8888. Will be trying the next url if available
c.c.c.ConfigServicePropertySourceLocator : Could not locate PropertySource: I/O error on GET request for "http://localhost:8888/config-client/dev/master": Connection refused (Connection refused); nested exception is java.net.ConnectException: Connection refused (Connection refused)
c.s.c.ConfigClientApplication: No active profile set, falling back to default profiles: default
```

### 控制器

```java
package com.spaco.configclient;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.core.env.Environment;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class IndexController {
    @Value("${filename}")
    private String filename;

    private final Environment environment;

    public IndexController(Environment environment) {
        this.environment = environment;
    }

    @GetMapping("/filename")
    public String filename(){
        return "filename is :"+filename;
    }
    @GetMapping("/filename-env")
    public String filename_env(){
        return environment.getProperty("filename","undefine");
    }
}
```


### 启动服务 

启动内容：

```bash
Fetching config from server at : http://localhost:8081
Located environment: name=config-client, profiles=[dev], label=master, version=dd39228cf1b62a2a12f6afb26fc49dda0d8cd451, state=null
Located property source: CompositePropertySource {name='configService', propertySources=[MapPropertySource {name='configClient'}, MapPropertySource {name='https://github.com/spaco/spring-cloud-config-repo-samples/config-client-dev.properties'}, MapPropertySource {name='https://github.com/spaco/spring-cloud-config-repo-samples/config-client-dev.yml'}, MapPropertySource {name='https://github.com/spaco/spring-cloud-config-repo-samples/config-client.properties'}, MapPropertySource {name='https://github.com/spaco/spring-cloud-config-repo-samples/config-client.yml'}]}
No active profile set, falling back to default profiles: default
```

### Test Cases

```bash
curl localhost:8082/filename
```

```bash
filename is :config-client-dev.properties
```

```bash
curl localhost:8082/filename-env
```

```bash
config-client-dev.properties
```

## Config Refresh ： Manual

Spring Cloud config强大的地方就在可以动态刷新服务的配置信息。然而，Spring Boot应用只能在服务启动的时候才能加载它们的配置信息，所以在Config Server对服务的配置做出的变更，Config Client 服务没办法主动去获取新的配置。所以Spring Boot提供一个@RefreshScope注解来解决这一问题，使用该注解后，开发团队可以使用`/actuator/refresh`来让应用主动重新读取配置信息。下面是@RefreshScope注解的使用方法：

### 配置Gradle

```
# 增加依赖 spring-boot-starter-actuator
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
}
```

### 配置 @RefreshScope 注解：于 Config Client Controller

```java
package com.spaco.configclient;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.core.env.Environment;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.cloud.context.config.annotation.RefreshScope;

@RestController
@RefreshScope
public class IndexController {
    @Value("${filename}")
    private String filename;

    private final Environment environment;

    public IndexController(Environment environment) {
        this.environment = environment;
    }

    @GetMapping("/filename")
    public String filename(){
        return "filename is :"+filename;
    }
    @GetMapping("/filename-env")
    public String filename_env(){
        return environment.getProperty("filename","undefine");
    }
}
```

### 启动服务



### Test Cases

在不改变Git配置的情况下，调取refeash接口

```bash
curl -X POST http://localhost:8082/actuator/refresh
```

```bash
[]
```

结果：空数据，无Git配置修改

调取 `curl localhost:8082/filename`

```bash
filename is :config-client-dev.properties
```



更新`config-client-dev.properties`配置文件，更新`filename`属性

```bash
filename=config-client-dev.properties updated
```

调取refeash接口:

```bash
curl -X POST http://localhost:8082/actuator/refresh
```

```bash
["config.client.version","filename"]
```

结果：返回修改的参数名

调取 `curl localhost:8082/filename`

```bash
filename is :config-client-dev.properties updated
```

结果：在不重启服务的情况下，更新的配置已生效



## References

- [Actuator endpoints](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-endpoints.html)
- 

