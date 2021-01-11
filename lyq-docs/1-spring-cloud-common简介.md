# 1-spring-cloud-common简单

为了书写方便，我们约定`spring-cloud-common`简写为scc。如果有特殊情况，我会特别说明。

## 官网定义



官网地址:https://spring.io/projects/spring-cloud-commons。其对scc的定义如下：

> Spring Cloud Commons delivers features as two libraries: Spring Cloud Context and Spring Cloud Commons. Spring Cloud Context provides utilities and special services for the ApplicationContext of a Spring Cloud application (bootstrap context, encryption, refresh scope and environment endpoints). Spring Cloud Commons is a set of abstractions and common classes used in different Spring Cloud implementations (eg. Spring Cloud Netflix vs. Spring Cloud Consul).

翻译过来有几个关键点：

- scc同两个模块组成：Spring Cloud Context和Spring Cloud Common。
- Spring Cloud Context 为Spring Cloud应用提供了统一的ApplicationContext管理。具体实现是Spring Cloud Context作为Spring Application Context的父Context。在引导时初始化加载。
- Spring Cloud Common提供了一系列的抽象和通用的实现，方便开发者切换底层的不同具体实现。

因此，我们要自已实现一些功能 ，需要与Spring Cloud的集成  ，就需要参考Spring cloud Common给我们预留的扩展点和相应的具体规范。



## Spring Cloud Context特点

- Bootstrap Context
  - 引导Context，作为应用的context的父context。
- `TextEncryptor` beans
  - TODO： 应该是配置属性加密的Bean
- Refresh Scope
  - 扩展 了Spring Bean 的作用域，默认有single,global,session,request等。现在添加了一个Refresh Scope，表示可以刷新的Bean。
- Spring Boot Actuator endpoints for manipulating the `Environment`
  - Acturator端点，用来控制Spring cloud应用的Environment，即配置项。

## Spring cloud common 特点

- `DiscoveryClient` interface
  - 服务发现抽象 ，统一了服务发现编程模型
- `ServiceRegistry` interface
  - 服务注册抽旬，统一了服务注册编程模型
- Instrumentation for `RestTemplate` to resolve hostnames using `DiscoveryClient`
  - 集成了RestTemplate组件，可以通过DiscoverClient配置，支持hostname实现服务调用，从而屏蔽具体的ip:port这种模式。