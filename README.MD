# JHipster 搭建微服务应用

## JHipster 操作步骤

1. mkdir jhipster

2. cd jhipster

3. mkdir app

4. cd app

5. yo jhipster

6. 进入创建应用

7. Which *type* of application would you like to create? (Use arrow keys)

    - 选择创建应用的类型

    - Monolithic application (recommended for simple projects)

        - 综合应用

    - Microservice application

        - 微服务应用

    - Microservice gateway

        - 微服务网关

    - JHipster UAA server (for microservice OAuth2 authentication)

        - OAuth2 验证

8. What is the base name of your application? (app)

    - 选择应用的名字

9. What is your default Java package name?

    - 选择 src 包内下的包路径，一般为域名反写 `com.jhipster.demo`

10. Do you want to use the JHipster Registry to configure, monitor and scale your
    application?

    - 是否使用 JHipster 注册吧来配置，监控和扩展应用

    - ✅ JWT authentication (stateless, with a token)

        - J(son)W(eb)T(oken)(无状态)

    - HTTP Session Authentication (stateful, default Spring Security mechanism)

        - Spring Security默认机制 基于HTTP会话的认证方式(带状态)

    - OAuth 2.0 / OIDC Authentication (stateful, works with Keycloak and Okta)

        - OAuth2的认证实现(无状态)