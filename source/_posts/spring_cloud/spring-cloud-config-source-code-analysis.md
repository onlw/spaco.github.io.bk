---
title: spring cloud config source code analysis
permalink: spring-cloud-config-source-code-analysis
categories:
- program
tags: 
- spring-cloud
comments: true
date: 2019-09-30
updated: 2019-09-30
---

# Spring Cloud Config Source code analysis

config client 请求 config server接口，config server 拉取 git repository 的代码，然后反馈给 config client

## Config Server

### 入口文件

```java

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.config.server.EnableConfigServer;

@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {

	public static void main(String[] args) {
		SpringApplication.run(ConfigServerApplication.class, args);
	}
}
```

`@EnableConfigServer` ：使服务成为配置中心

### EnableConfigServer

```java
package org.springframework.cloud.config.server;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.springframework.cloud.config.server.config.ConfigServerConfiguration;
import org.springframework.context.annotation.Import;

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import(ConfigServerConfiguration.class)
public @interface EnableConfigServer {

}
```

作用：主要是引入 `ConfigServerConfiguration.class`

### ConfigServerConfiguration.class

```java
package org.springframework.cloud.config.server.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ConfigServerConfiguration {

	@Bean
	public Marker enableConfigServerMarker() {
		return new Marker();
	}
	class Marker {
	}
}
```

config server 配置类

作用：装配 `Marker` Bean，开启 `ConfigServerAutoConfiguration`

### ConfigServerAutoConfiguration

```java
package org.springframework.cloud.config.server.config;

import org.springframework.boot.autoconfigure.condition.ConditionalOnBean;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

@Configuration
@ConditionalOnBean(ConfigServerConfiguration.Marker.class)
@EnableConfigurationProperties(ConfigServerProperties.class)
@Import({ EnvironmentRepositoryConfiguration.class, CompositeConfiguration.class,
		ResourceRepositoryConfiguration.class, ConfigServerEncryptionConfiguration.class,
		ConfigServerMvcConfiguration.class })
public class ConfigServerAutoConfiguration {
  
}
```

`@ConditionalOnBean(ConfigServerConfiguration.Marker.class)` 从这个可以看出，`ConfigServerAutoConfiguration`的前提条件：`ConfigServerConfiguration.Marker.class` Bean存在

引入多个配置类

作用：

- 引入`EnvironmentRepositoryConfiguration` ： 配置中心的关键 Configuration 类
- 引入其他（待续）

### EnvironmentRepositoryConfiguration.class

为什么说 `EnvironmentRepositoryConfiguration.class` 是关键配置类呢？

这个配置类中包含很多实现了`org.springframework.cloud.config.server.environment.EnvironmentRepository`接口的类，每个实现类都对应一种类型（git/svn/navtie/vault）的配置。 EnvironmentRepositoryConfiguration通过profile注解（对当前应用的环境）决定使用装配哪个`EnvironmentRepository` Bean。默认是`MultipleJGitEnvironmentRepository`

`EnvironmentRepository` 接口实现作用：根据 `application` `profile` `label` 找到对应的配置文件

作用：

- 引入Git Svn Native Vault等配置类
- 实现各个实现方式

```java
package org.springframework.cloud.config.server.config;

@Configuration
@EnableConfigurationProperties({ SvnKitEnvironmentProperties.class,
		CredhubEnvironmentProperties.class, JdbcEnvironmentProperties.class,
		NativeEnvironmentProperties.class, VaultEnvironmentProperties.class })
@Import({ CompositeRepositoryConfiguration.class, JdbcRepositoryConfiguration.class,
		VaultConfiguration.class, VaultRepositoryConfiguration.class,
		CredhubConfiguration.class, CredhubRepositoryConfiguration.class,
		SvnRepositoryConfiguration.class, NativeRepositoryConfiguration.class,
		GitRepositoryConfiguration.class, DefaultRepositoryConfiguration.class })
public class EnvironmentRepositoryConfiguration {
		//忽略部分代码
  @Configuration
	@ConditionalOnClass(TransportConfigCallback.class)
	static class JGitFactoryConfig {
		@Bean
		public MultipleJGitEnvironmentRepositoryFactory gitEnvironmentRepositoryFactory(
				ConfigurableEnvironment environment, ConfigServerProperties server,
				Optional<ConfigurableHttpConnectionFactory> jgitHttpConnectionFactory,
				Optional<TransportConfigCallback> customTransportConfigCallback) {
			return new MultipleJGitEnvironmentRepositoryFactory(environment, server,
					jgitHttpConnectionFactory, customTransportConfigCallback);
		}
	}
  //忽略部分代码
  @Configuration
	static class NativeFactoryConfig {
		@Bean
		public NativeEnvironmentRepositoryFactory nativeEnvironmentRepositoryFactory(
				ConfigurableEnvironment environment, ConfigServerProperties properties) {
			return new NativeEnvironmentRepositoryFactory(environment, properties);
		}
	}
  //忽略部分代码
}
```

#### 为什么默认是Git 仓库

```java
package org.springframework.cloud.config.server.config;

@Configuration
@ConditionalOnMissingBean(value = EnvironmentRepository.class, search = SearchStrategy.CURRENT)
class DefaultRepositoryConfiguration {

	@Bean
	public MultipleJGitEnvironmentRepository defaultEnvironmentRepository(
			MultipleJGitEnvironmentRepositoryFactory gitEnvironmentRepositoryFactory,
			MultipleJGitEnvironmentProperties environmentProperties) throws Exception {
		return gitEnvironmentRepositoryFactory.build(environmentProperties);
	}
}
```

可以看到，`@ConditionalOnMissingBean(value = EnvironmentRepository.class, search = SearchStrategy.CURRENT)`，此时，返回的是`MultipleJGitEnvironmentRepository`

### EnvironmentRepository

是一个配置管理仓库接口，抽象了获取配置的方法:

作用：根据 `application` `profile` `label` 找到对应的配置文件，所有获取配置的实现方式（git/svn/navtie/vault）都会实现此接口

```java
package org.springframework.cloud.config.server.environment;

import org.springframework.cloud.config.environment.Environment;
public interface EnvironmentRepository {
	Environment findOne(String application, String profile, String label);
}
```

它的实现类很多，如下图：

![](https://spaco.oss-cn-hangzhou.aliyuncs.com/spring-cloud/spring-cloud-config-server-repository.png)

从文件名中大概可以看出，这些类应该是用于加载不同类型的配置（后面会再介绍）。
有了获取配置的类，还差对外提供接口的类，就是`org.springframework.cloud.config.server.environment.EnvironmentController`

### EnvironmentController

`EnvironmentController`是`spring-cloud-config-server`包的一个控制器类

作用：提供对外接口，根据接口传递参数获取对应配置

```java
package org.springframework.cloud.config.server.environment;

@RestController
@RequestMapping(method = RequestMethod.GET, path = "${spring.cloud.config.server.prefix:}")
public class EnvironmentController {
  	private EnvironmentRepository repository;

	private ObjectMapper objectMapper;

	private boolean stripDocument = true;

	private boolean acceptEmpty = true;

	public EnvironmentController(EnvironmentRepository repository) {
		this(repository, new ObjectMapper());
	}

	public EnvironmentController(EnvironmentRepository repository,
			ObjectMapper objectMapper) {
		this.repository = repository;
		this.objectMapper = objectMapper;
	}
  
  // 省略部分代码
	@RequestMapping("/{name}/{profiles}/{label:.*}")
	public Environment labelled(@PathVariable String name, @PathVariable String profiles,
			@PathVariable String label) {
  	if (name != null && name.contains("(_)")) {
			// "(_)" is uncommon in a git repo name, but "/" cannot be matched
			// by Spring MVC
			name = name.replace("(_)", "/");
		}
		if (label != null && label.contains("(_)")) {
			// "(_)" is uncommon in a git branch name, but "/" cannot be matched
			// by Spring MVC
			label = label.replace("(_)", "/");
		}
		Environment environment = this.repository.findOne(name, profiles, label);
		if (!this.acceptEmpty
				&& (environment == null || environment.getPropertySources().isEmpty())) {
			throw new EnvironmentNotFoundException("Profile Not found");
		}
		return environment;
  }
  
  @RequestMapping("/{label}/{name}-{profiles}.properties")
	public ResponseEntity<String> labelledProperties(@PathVariable String name,
			@PathVariable String profiles, @PathVariable String label,
			@RequestParam(defaultValue = "true") boolean resolvePlaceholders)
			throws IOException {
    	// 省略部分代码
  }
  @RequestMapping("/{label}/{name}-{profiles}.json")
	public ResponseEntity<String> labelledJsonProperties(@PathVariable String name,
			@PathVariable String profiles, @PathVariable String label,
			@RequestParam(defaultValue = "true") boolean resolvePlaceholders)
			throws Exception {
    	// 省略部分代码
  }
}
```

以 `labelled` 方法为例，本质上，是通过 `EnvironmentRepository` （即我们上面说的配置仓库类）调用 `findOne`方法，得到对应的配置

### 小结

配置中心通过`org.springframework.cloud.config.server.environment.EnvironmentController`提供外部访问配置接口，通过实现 `org.springframework.cloud.config.server.environment.EnvironmentRepository`配置仓库接口类获取配置，默认是 Git