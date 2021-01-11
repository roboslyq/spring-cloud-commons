# spring-cloud-common 之Context

## 引导应用程序上下文BootStrap Application

# 切入点

我们都知道，spring cloud 的项目都是基于spring boot来实现的。因此，spring boot是spring cloud项目的基石。而spring boot提供了一种facorty扩展机制，刚好spring cloud使用此扩展机制。因此要分析spring cloud,可以从从spring.facorties配置一步步入手。

我们先来看`spring-cloud-context`项目的factories配置

```properties
# AutoConfiguration（自动化配置）
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.cloud.autoconfigure.ConfigurationPropertiesRebinderAutoConfiguration,\
org.springframework.cloud.autoconfigure.LifecycleMvcEndpointAutoConfiguration,\
org.springframework.cloud.autoconfigure.RefreshAutoConfiguration,\
org.springframework.cloud.autoconfigure.RefreshEndpointAutoConfiguration,\
org.springframework.cloud.autoconfigure.WritableEnvironmentEndpointAutoConfiguration
# Application Listeners(事件监听器)
org.springframework.context.ApplicationListener=\
org.springframework.cloud.bootstrap.BootstrapApplicationListener,\
org.springframework.cloud.bootstrap.LoggingSystemShutdownListener,\
org.springframework.cloud.context.restart.RestartListener
# Spring Cloud Bootstrap components(BootStrap引导配置)
org.springframework.cloud.bootstrap.BootstrapConfiguration=\
org.springframework.cloud.bootstrap.config.PropertySourceBootstrapConfiguration,\
org.springframework.cloud.bootstrap.encrypt.EncryptionBootstrapConfiguration,\
org.springframework.cloud.autoconfigure.ConfigurationPropertiesRebinderAutoConfiguration,\
org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration,\
org.springframework.cloud.util.random.CachedRandomPropertySourceAutoConfiguration
# Spring Boot Bootstrappers
org.springframework.boot.Bootstrapper=\
org.springframework.cloud.bootstrap.TextEncryptorConfigBootstrapper
# Environment Post Processors( env的处理器)
org.springframework.boot.env.EnvironmentPostProcessor=\
org.springframework.cloud.bootstrap.encrypt.DecryptEnvironmentPostProcessor
```

> 上面的org.springframework.boot.xxx配置，均是spring.boot的相关配置。

## Application Context继承

### Spring Cloud Context简介

Spring Cloud ApplicationContext的实现，是基于spring framework的Context有父子Context这种设计。比如说，spring webmvc除了Spring Bean Context外，还有WebApplicationContext。

**TODO**

而spring cloud applicationConext正是基于这样的设计出现的。

Spring cloud application Context又叫"BootStrap Application Context"，是应用程序application Context的parent。详情见官网解释：

> A Spring Cloud application operates by creating a “bootstrap” context, which is a **parent context for the main application**. This context is responsible for loading configuration properties from the external sources and for decrypting properties in the local external configuration files. The two contexts share an `Environment`, which is the source of external properties for any Spring application. By default, bootstrap properties (not `bootstrap.properties` but properties that are loaded during the bootstrap phase) are added with high precedence, so they cannot be overridden by local configuration.

Spring Cloud Application Context主要功能是从外部资源(除了付统的配置文件，还包括**分布式配置中心**)加载配置，并且这种配置支持加/解密功能。

spring cloud application Context和应用主context共享一个Enviroment。

并且，通过配置中心加载的properties优先级较高。默认情况下，应用的本地配置不能覆盖配置中心的配置。当然，我们可以调用调整参数，使用其可以被覆盖。这个我们后面再说。

因为bootstap context的主要职责是加载配置，所以对于bootstrap.yml的核心参数就是指定配置中心，示例如下：

```yml
spring:
  application:
    name: foo
  cloud:
    config:
      uri: ${SPRING_CONFIG_URI:http://localhost:8888}
```

> 注意，此处一定要指明spring.application.name属性，这样才可能从配置中心获取对应的应用的配置。同时spring.application.name也是应用 context的 ID

### Spring Cloud Context继承

如果我们是通过`SpringApplication` 或者  `SpringApplicationBuilder`来创建的，那么Bootstrap context 将是Application Context的父Context。





### 相关的抽象

- `PropertySourceLocators`
- `CompositePropertySource`
- 





# 参考资料

https://blog.csdn.net/ttyy1112/article/details/103162952

