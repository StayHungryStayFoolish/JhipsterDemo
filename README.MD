# JHipster 搭建微服务应用

## 项目启动

1. jhipster-registry

    - ./mvnw -Pprod package

      该命令没有测试，只解释页面空白原因

      可以启动，如果没有 -Pprod package ，页面 localhost:8761 页面空白，是因为 jhipster 还没有导入由模板生成的前段页面

2. gateway / microservice 需要配置数据库连接信息。

   如果想让项目启动时，自动穿件表，需要在 application-dev.yml 中加入: `spring.jpa.generate-ddl:true`

   当遇到控制台 `liqubase` 有问题是，如果不需要 `liqubase` ，在 `application-dev.yml` 中加入 `spring.profiles.include:no-liqubase,swagger`

## 基于 JHipster 4.12.0 版本 创建综合应用

#### 涉及到实体类的时候，需要先去创建当前项目名称的数据库 Database，并在 schema 中选择。

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
    
        - 不支持分页

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

## JHipster 创建 Entity

`假设：author 作者、 book 书 `

`一个作者可以有多本书，一本书只能有一个作者`

`则：作者 1 --> 书 N   书 N --> 作者 1`

`下边选项会出现 relationship 选项`

**author 实体和下边步骤一样，当前 author 已创建**

1. cd app 项目目录创建实体 book

    - yo jhipster:entity book

2. Do you want to add a field to your entity? (Y/n)

    - 是否添加 `域`

    - y

3. What is the name of your field?

    - 定义 `域名`

    - name

4. What is the type of your field? (Use arrow keys)

   - 定义 `域` 属性

   - ✅ String

   - Integer

   - Long

   - Float

   - Double

   - BigDecimal

   - LocalDate

   - ...

5. Do you want to add validation rules to your field? (y/N)

    - 是否给 `域` 增加规则

    - n

================= Book =================

Fields

name (String)

6. Generating field #2

   Do you want to add a field to your entity? (Y/n)

   - 是否再添加 `域`

   - Y

7. What is the name of your field?

    - 定义 `域名`

    - birthDate

8. What is the type of your field? (Use arrow keys)

    - 定义 `域` 属性

    - LocalDate

9. Do you want to add validation rules to your field?(y/N)

    - N

================= Book =================

Fields

name (String)

birthDate (LocalDate)

10. Do you want to add a field to your entity? (Y/n)

    - N 停止创建 `域`

11. Do you want to add a relationship to another entity? (Y/n)

    - 是否创建实体类之间的关联关系

    - Y

12. What is the name of the other entity?

    - 输入关联关系的实体类名

13. What is the name of the relationship?

    - 输入关联关系的实体类名

14. When you display this relationship with Angular, which field from 'author' do
    you want to use? This field will be displayed as a String, so it cannot be a Blo
    b (id)

    - 关联 id

    - id

15. Do you want to add any validation rules to this relationship? (y/N)

    - 是否需要添加规则

    - N

================= Book =================

Fields

name (String)

birthDate (LocalDate)

price (BigDecimal)

Relationships

author (Author) many-to-one

16. Do you want to add a relationship to another entity? (Y/n)

    - n

17. Do you want to use a Data Transfer Object (DTO)? (Use arrow keys)

    - ✅ No, use the entity directly

        - 是否创建DTO?

    - [BETA] Yes, generate a DTO with MapStruct

18. Do you want to use separate service class for your business logic? (Use arrow keys)

    - ✅ No, the REST controller should use the repository directly

        - 对于业务逻辑是否使用分离的服务类,使用REST controller和repository

    - Yes, generate a separate service class

    - Yes, generate a separate service interface and implementation

19. Do you want pagination on your entity?

    - 是否分页

    - ✅ No

    - Yes, with pagination links

    - Yes, with infinite scroll

20. 自动显示

        Everything is configured, generating the entity...

           create .jhipster/Book.json
           create src/main/resources/config/liquibase/changelog/20171214154350_added_entity_Book.xml
           create src/main/resources/config/liquibase/changelog/20171214154350_added_entity_constraints_Book.xml
           create src/main/java/com/jhipster/demo/domain/Book.java
           create src/main/java/com/jhipster/demo/repository/BookRepository.java
           create src/main/java/com/jhipster/demo/web/rest/BookResource.java
           create src/test/java/com/jhipster/demo/web/rest/BookResourceIntTest.java
         conflict src/main/resources/config/liquibase/master.xml

Overwrite src/main/resources/config/liquibase/master.xml? (ynaxdH)

  - y) overwrite

  - n) do not overwrite

  - ✅ a) overwrite this and all others

  - x) abort

  - d) show the differences between the old and the new

  - h) Help, list all options

  Answer: 输入 a

21. ✨  Done in 26.85s

#### 创建实体类，推荐使用 JDL Studio  https://start.jhipster.tech/jdl-studio/

![](img/jdl.jpg)

![](img/u356.svg)
![](img/u395.svg)
![](img/u428.svg)
![](img/u461.svg)






