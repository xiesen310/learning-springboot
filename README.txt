遵循阿里巴巴 Java 开发手册中的基本结构，下面是一个常见的包名划分：
├── main
│   ├── java
│   │   └── com
│   │       └── bestpractice
│   │           ├── config
│   │           ├── constants
│   │           ├── controller
│   │           ├── dao
│   │           ├── exception
│   │           ├── filter
│   │           ├── manager
│   │           ├── model
│   │           │   ├── bo
│   │           │   ├── dto
│   │           │   └── vo
│   │           ├── service
│   │           │   ├── impl
│   │           ├── task
│   │           ├── utils
│   │           └── BestPracticeApplication.java
│   │
│   └── resources

其中，每个包下面存放的类或者接口的类型说明如下：

config：存放配置相关的类，例如采用 @Configuration 注解的类都建议放这里，包括但不限于 Redis 配置类，RestTemplate 配置类，Swagger 配置类等。
constants：存放常量类、枚举类等。
controller：存放控制器类，工程对外的 RESTful 接口定义都在这个包中。
dao：存放数据访问相关的类，例如与 MySQL、HBase、Elasticsearch 等的数据访问类。
exception：存放全局异常处理类（使用 @ControllerAdvice 注解修饰），以及自定义的业务相关的异常类。
manager：存放通用业务处理相关的类，例如对第三方系统接口的封装类、service 层通用能力的封装类（缓存方案等）、对 dao 层中多个类的组合复用等。
model：存放 bean 的定义，根据领域模型层次的不同，bean 又可以进一步分为 DO、DTO、BO、AO、VO 等，具体可以参见阿里巴巴 Java 开发手册中的定义。
service：存放业务逻辑相关的处理类，通常在 service 包下面会以 interface 的方式定义接口（例如 SmsService），然后在 service/impl 包下面实现对应的接口（例如 SmsServiceImpl），从而对 controller 层的类而言，看到的永远是接口，而不是具体的实现类，很好的实现层与层之间的解耦。
task：存放定时任务相关的类。
utils：存放工程中需要使用到的工具类。

我们开发的服务通常会部署在不同的环境中，例如开发环境、测试环境、预发布环境，生产环境等，而不同环境需要不同的配置，例如连接不同的 Redis、数据库、第三方服务等等。Spring Boot 默认的配置文件是 application.properties。那么如何实现不同的环境使用不同的配置文件呢？一个比较好的实践是为不同的环境定义不同的配置文件，如下所示：

开发环境：application-dev.properties
测试环境：application-test.properties
预发布环境：application-stg.properties
生产环境：application-prd.properties
然后在启动服务时通过增加 --spring.profiles.active 参数来指定要启动哪个环境即可，例如启动生产环境，命令如下所示：

java -jar sms.jar --spring.profiles.active=prd

