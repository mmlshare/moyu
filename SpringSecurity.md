# Spring Security`5.3.5.RELEASE` 指南

## Spring Security

`Spring Security` 是一个提供身份认证、授权功能以防止常见的黑客技术攻击的框架。

凭借其对于命令式和响应式应用提供一流的支持，成为当前保护基于Spring的应用的标准方案。

## 前言

本节讨论Spring Security 的逻辑。

### 1. 必要条件

`Spring Security`依赖`Java8`及以上的运行环境。由于`Spring Security`目标是以 `self-contained`的方式实现，所以你不需要添加任何特殊的配置文件，所有必须的文件都在应用程序中，这种设计为应用部署提供良好的部署灵活性，因为你只需要把应用程序从一个系统复制到另一个系统就可以马上投入使用。

### 2. `Spring Security`社区

欢迎来到 Spring Security 社区! 本节讨论如何充分利用我们庞大的社区。



### 3. `Spring Security 5.3`新特性

Spring Security 5.3提供了许多新特性。



### 10. 身份认证

Spring Security 框架主要的组件：

|                  组件                  |                             说明                             |
| :------------------------------------: | :----------------------------------------------------------: |
|         SecurityContextHolder          |    用于存储已经认证过的用户信息`SecurityContext`的容器。     |
|            SecurityContext             |      存储已经认证过的用户`Authentication`的信息的容器。      |
|             Authentication             | 可作为`AuthenticationManager`的输入来验证用户凭据或者用于在`SecurityContext`中代表当前的用户。 |
|            GrantedAuthority            |               赋予`Authentication`权限的角色。               |
|         AuthenticationManager          |      定义如何验证对`Authentication`执行身份验证的API。       |
|            ProviderManager             |             `AuthenticationManager`的常用实现。              |
|         AuthenticationProvider         |         被`ProviderManager`用来执行特殊的身份验证。          |
|        AuthenticationEntryPoint        | 用于从客户端请求凭据（即，重定向到登录页面，发送WWW-Authenticate响应等）。 |
| AbstractAuthenticationProcessingFilter | 用于身份认证的基础的过滤器实现，帮助用户实现特定高级身份验证。 |

mvn dependency:resolve -Dclassifier=sources

