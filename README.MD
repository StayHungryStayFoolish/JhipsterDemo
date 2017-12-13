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

    - JWT authentication (stateless, with a token)

        - J(son)W(eb)T(oken)(无状态)

    - ✅ HTTP Session Authentication (stateful, default Spring Security mechanism)

        - Spring Security默认机制 基于HTTP会话的认证方式(带状态)

    - OAuth 2.0 / OIDC Authentication (stateful, works with Keycloak and Okta)

        - OAuth2的认证实现(无状态)

11. Which *type* of database would you like to use? (Use arrow keys)

    - 选择数据库类型

    - ✅ SQL (H2, MySQL, MariaDB, PostgreSQL, Oracle, MSSQL)

    - MongoDB

    - Cassandra

    - [BETA] Couchbase

12. Which *production* database would you like to use? (Use arrow keys)

    - 选择数据库具体类型

    - ✅ MySQL

    - MariaDB

    - PostgreSQL

    - Oracle (Please follow our documentation to use the Oracle proprietary driver)

    - Microsoft SQL Server

13. Which *development* database would you like to use? (Use arrow keys)

    - 选择开发数据库

    - H2 with disk-based persistence

        - 数据存储在磁盘的H2

    - H2 with in-memory persistence

        - 数据存储在内存的H2

    - ✅ MySQL

14. Do you want to use Hibernate 2nd level cache? (Use arrow keys)

    - `No` Hibernate 二级缓存

15. Would you like to use Maven or Gradle for building the backend? (Use arrow key
    s)

    - 选择喜欢的版本依赖工具

    - ✅ Maven

    - Gradle

16. Which other technologies would you like to use? (Press <space> to select, <a>
    to toggle all, <i> to inverse selection)

    - 可选技术，使用空格多选

     - Social login (Google, Facebook, Twitter)

        - 第三方社交登录，基于OAuth2.0

     - Search engine using ElasticSearch

        - 集成搜索开源搜索引擎 ElasticSearch

     - Clustered HTTP sessions using Hazelcast

        - 使用Hazelcast管理http session集群

     - WebSockets using Spring Websocket

        - 使用Spring Websocket

     - [BETA] Asynchronous messages using Apache Kafka

        - Apache Kafka 是一种高吞吐量的分布式发布订阅消息系统

17. Which *Framework* would you like to use for the client? (Use arrow keys)

    - 选择喜欢的前端框架

    - ✅ Angular 5

    - AngularJS 1.x

18. Would you like to enable *SASS* support using the LibSass stylesheet prepro
    cessor? (y/N)

    - 建议使用gulp定时构建，将sass转换成css而不是运行时处理

    - y

19. Would you like to enable internationalization support? (Y/n)

    - 是否使用国际化

    - n

20.  Besides JUnit and Karma, which testing frameworks would you like to use? (P
     ress <space> to select, <a> to toggle all, <i> to inverse selection)

    - 选择压力测试

    - ✅ Gatling

        - 使用 Gatling 进行性能测试。

    - Cucumber

        - 使用 Cucumber 模拟用户行为

    - Protractor

        - 使用 Protractor 对 AngularJS 进行集成测试

21. Would you like to install other generators from the JHipster Marketplace? (
    y/N)

    - N

第一次比较慢。

如果项目报错，可能需要更改 application.yml 的数据库连接

scheme.sql 填写项目名字

22. ./mvnw 运行项目

    yarn start 开启后端开发