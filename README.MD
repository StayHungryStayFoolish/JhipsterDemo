# JHipster 搭建微服务应用

## 基于 JHipster 4.12.0 版本 创建综合应用

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

10. Do you want to use the JHipster Registry to configure, monitor and scale your application?

    - 是否使用 JHipster 注册来配置，监控和扩展应用

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

15. Would you like to use Maven or Gradle for building the backend? (Use arrow keys)

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

18. Would you like to enable *SASS* support using the LibSass stylesheet preprocessor? (y/N)

    - 建议使用gulp定时构建，将sass转换成css而不是运行时处理

    - y

19. Would you like to enable internationalization support? (Y/n)

    - 是否使用国际化

    - n

20. Besides JUnit and Karma, which testing frameworks would you like to use?
     (Press <space> to select, <a> to toggle all, <i> to inverse selection)

    - 选择压力测试

    - ✅ Gatling

        - 使用 Gatling 进行性能测试。

    - Cucumber

        - 使用 Cucumber 模拟用户行为

    - Protractor

        - 使用 Protractor 对 AngularJS 进行集成测试

21. Would you like to install other generators from the JHipster Marketplace? (y/N)

    - N

第一次比较慢。

如果项目报错，可能需要更改 application.yml 的数据库连接

scheme.sql 填写项目名字

22. ./mvnw 运行项目

    yarn start 开启后端开发

## 基于 JHipster 4.12.0 版本 创建微服务身份认证中心 UAA 应用

1. mkdir uaa

2. cd uaa

3. yo jhipster

4. Which *type* of application would you like to create? (Use
   arrow keys)

   - ✅ JHipster UAA server (for microservice OAuth2 authentication)

5. What is the base name of your application? (uaa)

   - uaa

6. As you are running in a microservice architecture, on which
    port would like your server to run? It should be unique to a
   void port conflicts. (9999)

   - 设置运行端口

   - 9999

7. What is your default Java package name? (com.jhipster.demo)

   - 设置 src 目录下的包路径

   - com.jhipster.demo

8. Which service discovery server do you want to use?

   - 选择注册服务中心

   - ✅JHipster Registry (uses Eureka, provides Spring Cloud Config support and monitoring dashboards)

        - Eureka 注册服务中心

   - Consul

        - Consul

   - No service discovery

9. Which *type* of database would you like to use? (Use arrow keys)

   - ✅ SQL

10. Which *production* database would you like to use? (Use arrow keys)

   - 生产环境，选择数据库产品

   - ✅ MySQL

11. Which *development* database would you like to use? (Use arrow keys)

   - 开发环境，选择数据库产品

   - ✅ H2 with disk-based persistence

       - 数据存储在磁盘的H2

12. Do you want to use Hibernate 2nd level cache? (Use arrow keys)

   - 是否使用 Hibernate 二级缓存

   - Yes, with ehcache (local cache, for a single node)

       - 本地缓存方案 ehcache

   - ✅ Yes, with HazelCast (distributed cache, for multiple nodes)

       - 集群缓存方案，多节点缓存，适合多微服务的分布式环境

   - [BETA] Yes, with Infinispan (hybrid cache, for multiple nodes)

       -  ????

   - No

13. Would you like to use Maven or Gradle for building the backend? (Use arrow keys)

   - 选择构建工具

   - ✅ Maven

   - Gradle

14. Which other technologies would you like to use? (Press <space> to select, <a> to toggle all, <i> to inverse selection)

   - 选择其他技术，空格选择

   - ✅ Search engine using Elasticsearch

        - 搜索引擎

   - API first development using swagger-codegen

        - 开发

   - Asynchronous messages using Apache Kafka

        - Kafka 消息队列

15. Would you like to enable internationalization support? (Y/n)

   - 是否支持国际化

   - Y

16. Besides JUnit and Karma, which testing frameworks would you like to use?

   - 选择压力测试

   - ✅ Gatling

        - 使用 Gatling 进行性能测试。

   - Cucumber

        - 使用 Cucumber 模拟用户行为

17. Would you like to install other generators from the JHipster Marketplace? (y/N)

   - 是否需要安装其他生成器

   - N

18. ./mvnw 启动项目

## 基于 JHipster 4.12.0 版本 创建微服网关

1. mkdir gateway

2. cd gateway

3. yo jhipster

4. Which *type* of application would you like to create? (Use arrow keys)

    - ✅ Microservice gateway

        - 微服务网关

5. What is the base name of your application? (gateway)

    - 设置应用名字

    - gateway

6. As you are running in a microservice architecture, on which port would like your server to run?
   It should be unique to avoid port conflicts. (8080)

    - 设置一个端口号

    - 8080

7. What is your default Java package name?(com.jhipster.demo)

    - src 包路径

    - com.jhipster.demo

8. Which service discovery server do you want to use? (Use arrow keys)

    - 选择注册服务中心

    - ✅ JHipster Registry (uses Eureka, provides Spring Cloud Config support and monitoring dashboards)

9. Which *type* of authentication would you like to use?

    - 选择身份验证中心

        - ✅ JWT authentication (stateless, with a token)

            - J(son)W(eb)T(oken)(无状态)

        - Authentication with JHipster UAA server (the server must be generated separately)

            - JHipster UAA 身份校验中心（服务器必须单独生成）

        - OAuth 2.0 / OIDC Authentication (stateful, works with Keycloak and Okta)

            - OAuth2的认证实现(无状态)

10. Which *type* of database would you like to use? (Use arrow keys)

    - 选择数据库类型

    - MySQL

11. Which *production* database would you like to use? (Use arrow keys)

    - 选择生产环境的数据库

    - MySQL

12. Which *development* database would you like to use? (Use arrow keys)

    - 开发环境，选择数据库产品

       - ✅ H2 with disk-based persistence

           - 数据存储在磁盘的H2

13.  Would you like to use Maven or Gradle for building the backend? (Use arrow keys)

    - 选择构建工具

    - ✅ Maven

    - Gradle

14. Which other technologies would you like to use? (Press <space> to select, <a>to toggle all, <i> to inverse selection)

    - 可选技术，使用空格多选

         - Search engine using Elasticsearch

            - 搜索引擎

         - Clustered HTTP sessions using Hazelcast

            - 使用Hazelcast管理http session集群

         - WebSockets using Spring Websocket

            - 使用Spring Websocket

         - API first development using swagger-codegen

            - ???

         - [BETA] Asynchronous messages using Apache Kafka

            - Apache Kafka 是一种高吞吐量的分布式发布订阅消息系统

15. Which *Framework* would you like to use for the client? (Use arrow keys)

    - 选择前端框架

    - ✅ Angular 5

    - AngularJS 1.x

16. Would you like to enable *SASS* support using the LibSass stylesheet preprocessor? (y/N)

    - 建议使用gulp定时构建，将sass转换成css而不是运行时处理

    - y

17. Would you like to enable internationalization support? (Y/n)

    - 是否选用国际化支持

    - Y

18. Please choose the native language of the application (Use arrow keys)

    - Chinese (Simplified)

19. Please choose additional languages to install

    - 任意选

20. Besides JUnit and Karma, which testing frameworks would you like to use?
    (Press <space> to select, <a> to toggle all, <i> to inverse selection)

    - 选择压力测试

        - ✅ Gatling

            - 使用 Gatling 进行性能测试。

        - Cucumber

            - 使用 Cucumber 模拟用户行为

        - Protractor

            - 使用 Protractor 对 AngularJS 进行集成测试

21. Would you like to install other generators from the JHipster Marketplace? (y/N)

    - 是否从 JHipster 市场安装其他应用

    - N

22. ./mvnw 启动应用

    yarn start 启动开发

## 基于 JHipster 4.12.0 版本 创建微服务应用

1. mkdir microservice

2. cd microservice

3. yo jhipster

4. Which *type* of application would you like to create? (Use arrow keys)

    - Microservice application  微服务应用

5. What is the base name of your application? (microservice)

    - microservice

6. As you are running in a microservice architecture, on which port would like your server to run?
   It should be unique to avoid port conflicts. (8081)

    - 设置运行微服务应用的端口号

    - 8081

7. What is your default Java package name? (com.jhipster.demo)

    - src 包路径

    - com.jhipster.demo

8. Which service discovery server do you want to use? (Use arrow keys)

    - 选择注册服务中心

    - ✅ JHipster Registry (using Eureka and Spring Cloud Config)

9. Which *type* of authentication would you like to use? (Use arrow keys)

    - 选择身份认证

    - ✅ Authentication with JHipster UAA server (the server must be generated separate
      ly)

        - JHipster UAA 身份校验中心（服务器必须单独生成）

10. What is the folder path of your UAA application? (../uaa)

     - 设置 uaa 身份验证程序的 的路径

     - ../uaa

11. Which *type* of database would you like to use? (Use arrow keys)

     - 选择数据库类型

     - ✅ SQL

12. Which *production* database would you like to use? (Use arrow keys)

    - 选择生产环境数据库

    -MySQL

13. Which *development* database would you like to use? (Use arrow keys)

    - 开发环境，选择数据库产品

        - ✅ H2 with disk-based persistence

            - 数据存储在磁盘的H2

14. Do you want to use Hibernate 2nd level cache? (Use arrow keys)

    - Yes, with ehcache (local cache, for a single node)

        - 本地缓存方案 ehcache

    - Yes, with HazelCast (distributed cache, for multiple nodes)

        - 集群缓存方案，多节点缓存，适合多微服务的分布式环境

    - [BETA] Yes, with Infinispan (hybrid cache, for multiple nodes)

        - ????

    - No

15. Would you like to use Maven or Gradle for building the backend? (Use arrow keys)

    - 选择构建工具

    - ✅ Maven

    - Gradle

16. Which other technologies would you like to use?
    (Press <space> to select, <a>to toggle all, <i> to inverse selection)

    - 选择其他应用技术

    - Search engine using Elasticsearch

        - 集成搜索开源搜索引擎

    - API first development using swagger-codegen

        -  ？？？

    - Asynchronous messages using Apache Kafka

        - 高吞吐量的分布式发布订阅消息系统

17. Would you like to enable internationalization support? (Y/n)

    - 是否支持国际化（多版本语言）

    - n

18. Besides JUnit and Karma, which testing frameworks would you like to use?
    (Press <space> to select, <a> to toggle all, <i> to inverse selection)

    - 选择测试框架

    - ✅ Gatling

        - 使用 Gatling 进行性能测试。

    - Cucumber

        - 使用 Cucumber 模拟用户行为

19. Would you like to install other generators from the JHipster Marketplace? (y/N)

    - 是否从 JHipster 市场安装其他应用

    - N

20. ./mvnw 启动应用

## JHipster Registry

- GitHub Clone JHipster Registry 项目

    - git clone https://github.com/jhipster/jhipster-registry.git

- 修改 jhipster-registry -> src -> main -> resources -> config -> application.yml

        添加
        security:
                authentication:
                    jwt:
                        secret: test

        最后显示：

        jhipster:
            swagger:
                default-include-pattern: /(api|config)/.*
                title: JHipster Registry API
                description: JHipster Registry API documentation
                version: ${info.project.version}
            security:
                authentication:
                    jwt:
                        secret: test

- Download https://github.com/jhipster/jhipster-registry/releases  对应版本的 jhipster-registry-3.2.3.war

    **放入项目根目录，与 central-config (配置中心平级)**

- cd 到项目根目录

- 运行

        ./jhipster-registry-3.2.3.war --security.user.password=admin --jhipster.security.authentication.jwt.secret=test --spring.cloud.config.server.native.search-locations=file:./central-config

- http:localhost:8761

   - admin 用户名

   - admin 密码


