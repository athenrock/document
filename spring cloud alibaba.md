### Spring Cluod Alibaba架构分析

[TOC]

### 1. 安装配置Nacos注册发现服务

### 2. 创建服务

#### 1. 创建项目

引用各种需要的包

dependencyManagement包引用管理，可以在父工程中配置，也可以在子工程中配置

下面这四个都用就完了：

+ spring-cloud-alibaba-dependencies

  alibaba出的springcloud依赖管理，要使用alibaba的全家桶这个肯定少不了

+ spring-cloud-dependencies

  springcloud官方的依赖，微服务架构下肯定都得用

+ spring-boot-dependencies

  springboot的官方依赖，我们项目就是基于springboot开发，这个肯定少不了

+ lombok

  简约化编程你离不了它，反正大家都在用

```xml
<dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>2.2.3.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR8</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.3.4.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>1.18.12</version>
                <optional>true</optional>
            </dependency>
        </dependencies>

    </dependencyManagement>
```



#### 2. 使用mysql

>  2.1 mysql链接驱动

```xml
<!--mysql 链接驱动-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
<!-- 采用最简单的JDBC链接-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

```yml
<!-- 数据库配置-->
spring:  
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/tk_user?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
    username: root
    password: 123456
```

```java
@RestController
public class IndexController {
    @Autowired
    JdbcTemplate jdbcTemplate;
    
    @GetMapping("/jdbc")
    public Object jdbcStart(){
        return jdbcTemplate.queryForList("select * from user_info");
    }
}
```

```json
# 查询结果
[
    {
        "id": 1,
        "name": "张三",
        "sex": 1,
        "create_time": "2020-10-19T16:18:48.000+00:00",
        "cretae_user": "sys",
        "update_time": "2020-10-19T16:19:05.000+00:00",
        "update_user": "sys",
        "data_status": 1
    }
]
```

>  2.2 采用阿里的druid链接池

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.10</version>
 </dependency>
```

```yml
<!-- 数据库配置-->
spring:  
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/tk_user?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
    username: root
    password: 123456
    type: com.alibaba.druid.pool.DruidDataSource
```

打开http://localhost:{port}/driud,可以看到数据库使用情况

durid数据库连接池，主要是做连接池最大化限制



#### 3. 使用mybaits-plus实现数据库访问

为什么选择的是mybaits-plus，为什么不选择mybaits

>  3.1 ``代码生成器``

> 3.2 日志

> 3.3 MybitasPulsConfig

这个配置可以做很多事情，第一个事情，就是可以把**@MapperScan** 从项目的**Application**启动项拿走

```java
@Configuration
@MapperScan("com.threeking.service.user.mapper")
public class MybatisPlusConfig {
}
```





#### 4. 使用swagger2

##### 4.1 旧时代额swagger

选择适用swagger做RESTful api文档接口

```xml
 <dependency>
     <groupId>io.springfox</groupId>
     <artifactId>springfox-swagger2</artifactId>
     <version>2.6.1</version>
</dependency>
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>swagger-bootstrap-ui</artifactId>
    <version>1.9.6</version>
</dependency>
```

使用swagger-bootstrao-ui做交互界面

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.threeking.service.user"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("swagger-bootstrap-ui RESTful APIs")
                .description("swagger-bootstrap-ui")
                .termsOfServiceUrl("http://localhost:6601/")
                .contact(new Contact("ah","doc.html",""))
                .version("1.0")
                .build();
    }
}
```

##### 4.2新时代的swagger，主要是swagger3.0之后的

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-boot-starter</artifactId>
    <version>3.0.0</version>
<dependency>
```

```java
@EnableOpenApi
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

}
```

访问：http://localhost:8080/swagger-ui/index.html  即可，方便简洁

如果想要继续使用`swagger-bootstrap-ui`,在pom文件中添加

```xml
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>swagger-bootstrap-ui</artifactId>
    <version>1.9.6</version>
</dependency>
```

访问：http://localhost:8080/doc.html 即可



### 3. 构建微服务体系

Nacos,是alibaba公司出的一款集服务发现、配置、管理现代应用架构[nacos官网](https://nacos.io/)

我们不去管怎么架设Nacos，只管怎么讲服务注册到Nacos，并实现服务之间的相互调用即可

#### 1. 注册到Nacos上

在nacos上创建各个微服务对应的配置文件

因为是基础spring cloud来构建的体系，所以直接进行SpringCloud模式下进行实现

引用的包

- 通过 Nacos Server 和 spring-cloud-starter-alibaba-nacos-config 实现配置的动态变更。
- 通过 Nacos Server 和 spring-cloud-starter-alibaba-nacos-discovery 实现服务的注册与发现。

**pom.xml**文件中添加以下引用

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
    <version>${latest.version}</version>
</dependency>
```

**bootstrap.yml**文件中添加nacos的配置

```yml
spring:
  cloud:
    nacos:
      config:
        file-extension: yml
        namespace: f9432bb6-6ba5-4f5a-998b-c2b2e7815528
        group: DEFAULT_GROUP
      server-addr: localhost:8848
```



**appliaction.class**启动项增加@EnableDiscoveryClient注解

```java
@EnableDiscoveryClient
@SpringBootApplication
public class ThreekingUserApplication {

    public static void main(String[] args) {
        SpringApplication.run(ThreekingUserApplication.class, args);
    }

}
```

服务列表中即可看到服务

![image-20201021160823315](C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201021160823315.png)







主要解决：nacos配置实现热加载（@value取值的问题）,下面的方式不是最优，想办法解决

方案一：

```java
@Autowired
ConfigurableApplicationContext configurableApplicationContext;

@RestController
public class testController {
   
 public String getCommon(){
	return configurableApplicationContext.getEnvironment().getProperty("common");     
 }
}

```

方案二：在类上增加@RefreshScope注册

```java
@RefreshScope
@RestController
public class testController {
   
   @Value("${common}")
   private String common;   
}

```



#### 2. 各微服务之前相互调用

 使用feign进行微服务之间的调用

为什么使用feign，其他的还有哪些

Feign是一种负载均衡的HTTP客户端, 使用Feign调用API就像调用本地方法一样，从避免了 调用目标微服务时，需要不断的解析/封装json 数据的繁琐。

引包：

**pom.xml**添加

```xml
 <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-openfeign</artifactId>
     <version>2.0.1.RELEASE</version>
</dependency>
```

新增一个接口类

UserFeign.java

```java
@FeignClient(value = "threeking-user")
public interface UserFeign {

    @GetMapping("/hello")
    String hello();
}
```

暴露一个接口

```java
@RestController
public class IndexController {

    @Autowired
    UserFeign userFeign;

    @GetMapping("/")
    public String hello(){
        String u = userFeign.hello();
        return "hello! this is content server, and user=" +u ;
    }
}
```

**@FeignClient(value = "threeking-user")**中的**threeking-user**对服务提供方，在nacos控制面板中可以找到

![image-20201021164556009](C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201021164556009.png)

浏览地址http://localhost:6602/

![image-20201021164737791](C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201021164737791.png)



结合swagger

接口介绍：

<img src="C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201021164815197.png" alt="image-20201021164815197" style="zoom:67%;" />

![image-20201021164841888](C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201021164841888.png)



#### 3.共用配置文件

配置文件扩展，修改各个微服务的配置文件

将bootstrap.yml进行修改,注意是bootstrap.yml，而不能用application.yml

```yml
spring:
  application:
    name: threeking-user
  cloud:
    nacos:
      server-addr: localhost:8848
      config:
        file-extension: yml
        namespace: f9432bb6-6ba5-4f5a-998b-c2b2e7815528
        group: DEFAULT_GROUP
        extension-configs:
          - data-id: common-config.yml
            group: GLOBAL_GROUP  # 不指定默认为DEFAULT_GROUP
            refresh: true
          - data-id: database-config.yml
            group: GLOBAL_GROUP
            refresh: true
```

**file-extension:** 服务默认配置文件，{application-name}.{file-extension} : threeking-user.yml

**extension-configs：** 配置文件扩展组

<u>common-config.yml</u> , 我将一些公共配置，如日志管理，常量设置的放置在这个配置中

```yml
#配置日志
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl

timeout: 20
```

<u>database-config.yml</u> 将一些如数据库、MQ、redis等服务器配置放置在这里

```yml
 spring:
    datasource:
        driver-class-name: com.mysql.jdbc.Driver
        url: jdbc:mysql://127.0.0.1:3306/tk_user?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
        username: root
        password: 123456
        type: com.alibaba.druid.pool.DruidDataSource
```



nacos上配置项如此：

![image-20201021173026061](C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201021173026061.png)



### 4.快速搭建一个vue项目

#### 1.使用vue-cli来构建一个项目，

> vue.js有著名的全家桶系列，包含了vue-router，vuex， vue-resource，再加上构建工具vue-cli，就是一个完整的vue项目的核心构成。
>
> vue-cli这个构建工具大大降低了webpack的使用难度，支持热更新，有webpack-dev-server的支持，相当于启动了一个请求服务器，给你搭建了一个测试环境，只关注开发就OK
>
> 1.安装vue-cli
>
> 2.用vue-cli来构建项目
>
> 3.启动项目

以上是一些大佬们总结出来的搭建方式，前端的小伙伴都会

下面这个是具体是具体命令

```bash
$ npm install -g @vue/cli
$ vue create my-project
$ cd my-project
$ npm run serve
```

 `npm install -g @vue/cli` 为vue cli3，配合`vue create my-project`使用，而之前的是`npm install -g vue-cli`为vue cli2，配合`vue init webpack my-project`,这里使用vue cli3

![image-20201021183957658](C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201021183957658.png)



创建项目`vue create my-project`时会有这个选项，Manually是手动配置，不知道选啥参考 [vue cli参考blog](https://www.cnblogs.com/zhoulifeng/p/11690799.html)、[vue-cli使用推荐](https://www.jb51.net/article/150901.htm)，不过这些都是3.0一下的，如果英语好的可以去官网上看



![image-20201021185007304](C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201021185007304.png)



![image-20201022142320982](C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201022142320982.png)

创建完成后，出现这么这个提示

```bash
 $ cd my-project
 $ npm run serve
```

按照命令执行后，打开链接http://localhost:8080/

<img src="C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201022092242643.png" alt="image-20201022092242643" style="zoom:67%;" />

至此完成一个vue项目的搭建

#### 2.使用vue项目图形化界面

`vue ui` 命令，全局使用，执行后即可打开http://localhost:8000/project/select

点击导入项目。将自己的项目导入进来，进行管理

![image-20201022145739589](C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201022145739589.png)

点击可以查看项目的插件、依赖等情况

![image-20201022145804448](C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201022145804448.png)

#### 3.解决跨域问题，并尝试调用服务接口

因为调用后台接口的时候，基本上都需要解决跨域问题，所以vue-cli给出了解决方案 ，[参考github](https://github.com/staven630/vue-cli4-config#top)

在项目根目录下面创建**<u>vue.config.js</u>**文件

```js
module.exports = {
    devServer: {
      // overlay: { // 让浏览器 overlay 同时显示警告和错误
      //   warnings: true,
      //   errors: true
      // },
      // open: false, // 是否打开浏览器
      // host: "localhost",
      // port: "8080", // 代理断就
      // https: false,
      // hotOnly: false, // 热更新
      proxy: {
        "/api": {
          target: process.env.VUE_APP_API, // "http://localhost:6601", // 目标代理接口地址
          secure: false,
          changeOrigin: true, // 开启代理，在本地创建一个虚拟服务端
          // ws: true, // 是否启用websockets
          pathRewrite: {
            "^/api": "/"
          }
        }
      }
    }
  };
```

修改helloWorld.vue

```vue
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>{{ content}}</p>
  </div>
  <button id='btnTest' @click="btnClick()">点一下</button>
</template>

<script>
import axios from "axios";
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
   data() {
    return {
      content:''
    } 
  },
  mounted(){    
     // js代码中使用环境变量    
    axios.get("/api/hello").then(reg=>{   
            this.content = reg.data  
          }).catch(function(error){
            console.log(error);
          })
  },
  methods:{
    btnClick (){
      axios.get("/api/jdbc").then(reg=>{   
            this.content = reg.data  
          }).catch(function(error){
            console.log(error);
          })
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

```

页面效果就出来了

![image-20201022184908264](C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201022184908264.png)

至此，我们讲微服务与前端完全打通，可以进行实际业务开发了





### 5.增加网关Gateway

实际开发过程中，前端有很多调用方，不可能实际调用每一个底层服务，spring cloud之前采用netfix出的zuul来解决，后来因为开源问题，spring boot团队自己开发了Gateway来代替之前的zuul。

#### 1.创建独立的Gateway网关项目

引用spring cloud gateway包，

```xml
 <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```

因为我们使用nacos，所以将`spring-cloud-starter-alibaba-nacos-discovery`也引进来

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

修改驱动项增加`@EnableDiscoveryClient`

```java
@EnableFeignClients
@EnableDiscoveryClient
@SpringBootApplication
public class ThreekingGatewayApplication {

    public static void main(String[] args) {
        SpringApplication.run(ThreekingGatewayApplication.class, args);
    }

}
```



bootstrap.yml文件中添加配置

```yml
spring:
  application:
    name: threeking-gateway
  cloud:
    nacos:
      discovery:
        namespace: f9432bb6-6ba5-4f5a-998b-c2b2e7815528 #nacos分组，不配置默认为public
        password: nacos
        server-addr: 127.0.0.1:8848
        username: nacos
    gateway:
      discovery:
        locator:
          enabled: true #locator 采用路由匹配
      routes:
        - id: threeking-user
          uri: lb://threeking-user  #服务项目名，和nacos上服务对应
          predicates:
            #- Method=GET,POST
            - Path=/user/**		#匹配路由规则
          filters:
            - StripPrefix=1  #截取地址一节
server:
  port: 6600
```

nacos上的展示效果

![image-20201023153636235](C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201023153636235.png)

访问地址：http://localhost:6600/user/hello

解析：

+ `http://localhost:6600` 为网关地址
+ `/use`为路由指定标识
+ `/hello`为user服务hello接口

#### 2.全局过滤

[`GatewayFilter` Factories](https://docs.spring.io/spring-cloud-gateway/docs/2.2.5.RELEASE/reference/html/#gatewayfilter-factories)

[Global Filters](https://docs.spring.io/spring-cloud-gateway/docs/2.2.5.RELEASE/reference/html/#global-filters)

#### 3.网关限流

3.1 使用Gateway进行限流，三种方式

+ url限流
+ 参数限流
+ IP限流

[`RequestRateLimiter` `GatewayFilter` Factory](https://docs.spring.io/spring-cloud-gateway/docs/2.2.5.RELEASE/reference/html/#the-requestratelimiter-gatewayfilter-factory)

需要使用redis 所以要添加`spring-boot-starter-data-redis-reactive`

```xml
<!-- redis依赖包-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
</dependency>
<!--commons-pool2 对象依赖池 -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
</dependency>
```

3.2 使用Sentinel

[Sentinel中文文档](https://github.com/alibaba/Sentinel/wiki/%E4%BB%8B%E7%BB%8D)

[spring cloud alibaba sentinel](https://github.com/alibaba/spring-cloud-alibaba/wiki/Sentinel) 和spring cloud gateway整合

#### 4.GateWay整合swagger

swagger常用的有两种UI，一种是swagger-ui、swagger-bootstrap-ui

+ swagger-ui  官方的，没啥不好，可以用
+ swagger-bootstrap-ui 国内大佬开发的，很多人都在用

swagger常用的有两种UI，一种是swagger-ui、swagger-bootstrap-ui

+ swagger-ui  官方的，没啥不好，可以用
+ swagger-bootstrap-ui 国内大佬开发的，很多人都在用

引包

```xml
<!-- swagger2 start -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>3.0.0</version>
</dependency>
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>swagger-bootstrap-ui</artifactId>
    <version>1.9.6</version>
</dependency>
<!-- swagger2 end -->
```

为啥要单独说gateway和swagger2整合，是因为swagger的资源接口支持的是spring MVC模式，而Gateway采用是webflux,因此，3.0.0之前都需要自己手动写配置

##### 4.1旧版解决方案

扩展SwaggerResourcesProvider，`springfox-swagger`包可以试`3.0.0`之下的依赖

```java
package com.threeking.gateway.swagger;

import lombok.AllArgsConstructor;
import org.springframework.cloud.gateway.config.GatewayProperties;
import org.springframework.cloud.gateway.route.RouteLocator;
import org.springframework.cloud.gateway.support.NameUtils;
import org.springframework.context.annotation.Primary;
import org.springframework.stereotype.Component;
import springfox.documentation.swagger.web.SwaggerResource;
import springfox.documentation.swagger.web.SwaggerResourcesProvider;

import java.util.ArrayList;
import java.util.List;

@Component
@Primary
@AllArgsConstructor
public class SwaggerProvider implements SwaggerResourcesProvider {
 
    public static final String API_URI = "/v2/api-docs";
 
    private final RouteLocator routeLocator;
 
    private final GatewayProperties gatewayProperties;
 
    @Override
    public List<SwaggerResource> get() {
        List<SwaggerResource> resources = new ArrayList<>();
        List<String> routes = new ArrayList<>();
 
        // 取出gateway的route
        routeLocator.getRoutes().subscribe(route -> routes.add(route.getId()));
        // 结合配置的route-路径(Path)，和route过滤，只获取有效的route节点
        // 打开下面注释可以自动扫描接入gateway的服务，为了演示，只扫描system
        gatewayProperties.getRoutes().stream().filter(routeDefinition ->  routes.contains(routeDefinition.getId()))
                .forEach(routeDefinition ->
                         routeDefinition.getPredicates().stream()
                        .filter(predicateDefinition -> ("Path").equalsIgnoreCase(predicateDefinition.getName()))
                        .forEach(predicateDefinition -> resources
                                .add(swaggerResource(routeDefinition.getId(), predicateDefinition.getArgs()
                                        .get(NameUtils.GENERATED_NAME_PREFIX + "0").replace("/**", API_URI)))));
        return resources;
    }
 
    private SwaggerResource swaggerResource(String name, String location) {
        SwaggerResource swaggerResource = new SwaggerResource();
        swaggerResource.setName(name);
        swaggerResource.setLocation(location);
        swaggerResource.setSwaggerVersion("2.0");
        return swaggerResource;
    }
}
```

```java
package com.threeking.gateway.swagger;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import reactor.core.publisher.Mono;

import springfox.documentation.swagger.web.*;
import java.util.Optional;

@RestController
@RequestMapping("/swagger-resources")
public class SwaggerHandler {
 
    @Autowired(required = false)
    private SecurityConfiguration securityConfiguration;
 
    @Autowired(required = false)
    private UiConfiguration uiConfiguration;
 
    private final SwaggerResourcesProvider swaggerResources;
 
    @Autowired
    public SwaggerHandler(SwaggerResourcesProvider swaggerResources) {
        this.swaggerResources = swaggerResources;
    }
 
    @GetMapping("/configuration/security")
    public Mono<ResponseEntity<SecurityConfiguration>> securityConfiguration() {

        return Mono.just(new ResponseEntity<>(
                Optional.ofNullable(securityConfiguration).orElse(SecurityConfigurationBuilder.builder().build()),
                HttpStatus.OK));
    }
 
    @GetMapping("/configuration/ui")
    public Mono<ResponseEntity<UiConfiguration>> uiConfiguration() {
        return Mono.just(new ResponseEntity<>(
                Optional.ofNullable(uiConfiguration).orElse(UiConfigurationBuilder.builder().build()
                        ), HttpStatus.OK));
    }


 
    @SuppressWarnings("rawtypes")
    @GetMapping("")
    public Mono<ResponseEntity> swaggerResources() {
        return Mono.just((new ResponseEntity<>(swaggerResources.get(), HttpStatus.OK)));
    }
}
```



![image-20201024145448128](C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201024145448128.png)

##### 4.2新版解决方案

因为swagger2之前的不支持webflux,所以3.0之后解决了这个问题，我们不用自己再转换一套`swagger-resources`

直接创建一个`SwaggerResourcesProvider`即可

```java

package com.threeking.gateway.swagger;

import lombok.AllArgsConstructor;
import org.springframework.cloud.gateway.config.GatewayProperties;
import org.springframework.cloud.gateway.route.RouteLocator;
import org.springframework.cloud.gateway.support.NameUtils;
import org.springframework.context.annotation.Primary;
import org.springframework.stereotype.Component;
import springfox.documentation.swagger.web.SwaggerResource;
import springfox.documentation.swagger.web.SwaggerResourcesProvider;

import java.util.*;

///**
// * 以gateway方式聚合各个服务的swagger接口文档（还可以加入healthy，去逐个匹配healthy，判断是否存活，活的话加入SwaggerResource列表，否则不加入）
// *
// */
@Component
@Primary
@AllArgsConstructor
public class Swagger3Provider implements SwaggerResourcesProvider {

    public static final String API_URI = "/v2/api-docs";

    private final RouteLocator routeLocator;

    private final GatewayProperties gatewayProperties;

    @Override
    public List<SwaggerResource> get() {
        List<SwaggerResource> resources = new ArrayList<>();
        List<String> routes = new ArrayList<>();

        // 取出gateway的route
        routeLocator.getRoutes()
                .filter(route -> route.getUri().getHost() != null)
                .filter(route -> Objects.equals(route.getUri().getScheme(), "lb"))
            	// .filter(route -> !self.equals(route.getUri().getHost()))
                .subscribe(route -> routes.add(route.getUri().getHost()));
        // 结合配置的route-路径(Path)，和route过滤，只获取有效的route节点
        // 打开下面注释可以自动扫描接入gateway的服务，为了演示，只扫描system
        gatewayProperties.getRoutes().stream().filter(routeDefinition ->  routes.contains(routeDefinition.getId()))
                .forEach(routeDefinition ->
                        routeDefinition.getPredicates().stream()
                                .filter(predicateDefinition -> ("Path").equalsIgnoreCase(predicateDefinition.getName()))
                                .forEach(predicateDefinition -> resources
                                        .add(swaggerResource(routeDefinition.getId(), predicateDefinition.getArgs()
                                                .get(NameUtils.GENERATED_NAME_PREFIX + "0").replace("/**", API_URI)))));
        return resources;
    }

    private SwaggerResource swaggerResource(String name, String location) {
        SwaggerResource swaggerResource = new SwaggerResource();
        swaggerResource.setName(name);
        swaggerResource.setLocation(location);
        swaggerResource.setSwaggerVersion("2.0");
        return swaggerResource;
    }
}
```

这种是使用Gateway方式，读取yml文件中的配置，我只识别了开启动态创建路由的方式,如果使用未使用RouteLocator的`.filter(route -> !self.equals(route.getUri().getHost()))`改用这个过滤器即可

### 6. 架构中的细节干货

#### 1.gateway使用断言都要干啥

1.1限制应用端时访问，每个端都有标识，否则不予响应

#### 2.gateway使用过滤器要干啥

##### 2.1 鉴权

> 用户登录认证

使用全局过滤器，通过验证token，或者别的标识，和redis中做对比，完成鉴权

> 非过滤地址忽略

在实际应用场景中，会碰到一些接口，不需要登录，也不需要任何标识就能访问，比如一般新闻相关，还有商品信息等，这时候我们希望网关必要对访问进行额外的处理，我们就需要在网关做一些配置

注意：实际开发中需要登录才可访问的接口远远大于不需要登录的接口，因此一般都是配置不需要登录的接口

创建在resource文件下创建`manage.properties`文件

```properties
#1.第一种书写方式
manage.excludes[0] = ^/user/v2/api-docs$
manage.excludes[1] = ^/user/hello$
manage.excludes[2] = ^/user/cp$

#1.第二种书写方式，用“,”分隔，方便维护，建议采用第二种
manage.excludes2 = ^/user/v2/api-docs$，\
  ^/user/hello$,\
  ^/user/cp$
```

创建一个`ManageExcludeConfig.java`

```java
package com.threeking.gateway.common;

import lombok.AllArgsConstructor;
import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.core.io.support.DefaultPropertySourceFactory;
import org.springframework.core.io.support.PropertySourceFactory;
import org.springframework.stereotype.Component;

import java.util.List;
import java.util.regex.Pattern;

/**
 * 排查参数
 * @Author: A.H
 * @Date: 2020/10/28 17:22
 */
@Data
@AllArgsConstructor
@Component
@PropertySource(value = {"classpath:conf/manage.properties"})
@ConfigurationProperties(prefix = "manage")
public class ManageExcludeConfig {
    //排除列表
    private List<String> excludes;

    /**
     * 验证是否排除
     * @return
     */
    public boolean isExclude(String uri) {
        return urlFilter(excludes, uri);
    }

    /**
     * 验证是否过滤
     * @param patternStrs
     * @param uri
     * @return
     */
    private boolean urlFilter(List<String> patternStrs, String uri) {
        for (String str : patternStrs) {
            if (Pattern.compile(str).matcher(uri).find()) {
                return true;
            }
        }

        return false;

    }
}

```

创建一个全局的过滤器

```java
@Slf4j
@Component
public class DomainGlobalFilter implements GlobalFilter, Ordered {

    @Autowired
    ManageExcludeConfig manageExcludeConfig;

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        ServerHttpRequest request = exchange.getRequest();
        HttpMethod method = request.getMethod();

        String url = request.getURI().getPath();
        if(manageExcludeConfig.isExclude(url)){
            log.info("非过滤地址，直接跳过.....");
            return chain.filter(exchange);
        }
		// TODO: 其他判断

        return null;
    }
```

当然，也可以用yml文件，或者直接用项目配置文件，这个单独拎出来只是为了方便管理



##### 2.2 参数传递

 	用户封装在gateway实现转发，前端不需用传用的的id，code等主键信息，所有的以登录信息为准，接上一个GlobalFilter



```java
@Slf4j
@Component
public class DomainGlobalFilter implements GlobalFilter, Ordered {

    @Autowired
    ManageExcludeConfig manageExcludeConfig;

    @Autowired
    AuthService authService;

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        ServerHttpRequest request = exchange.getRequest();
        HttpMethod method = request.getMethod();

        String url = request.getURI().getPath();
        if(manageExcludeConfig.isExclude(url)){
            log.info("非过滤地址，直接跳过.....");
            return chain.filter(exchange);
        }

        URI uri = exchange.getRequest().getURI();
        StringBuilder query = new StringBuilder();
        String originalQuery = uri.getRawQuery();
        if (StringUtils.hasText(originalQuery)) {
            query.append(originalQuery);
            if (originalQuery.charAt(originalQuery.length() - 1) != '&') {
                query.append('&');
            }
        }
        // 模拟从前端获取到的用户标识，可以是token，session，或者其他约定的参数
        String token = "123";
        // 从身份类里面获取用户信息
        query.append(authService.getQueryUserInfo(token));

        try {
            URI newUri = UriComponentsBuilder.fromUri(uri).replaceQuery(query.toString()).encode(StandardCharsets.UTF_8).build(true).toUri();
            ServerHttpRequest newRequest = exchange.getRequest().mutate().uri(newUri).build();
            return chain.filter(exchange.mutate().request(newRequest).build());
        } catch (RuntimeException var9) {
            var9.printStackTrace();
            throw new IllegalStateException("Invalid URI query: \"" + query.toString() + "\"");
        }

    }


    @Override
    public int getOrder() {
        return 1;
    }
}
```



创建一个AuthService.java服务类

```java
@Slf4j
@Service
public class AuthService {



    /**
     * 根据前端标识获取用户信息，可以是从redis，db等中获取
     * @param code
     * @return
     */
    public String getQueryUserInfo(String code){

        //TODO: 根据code从redis或者数据库中获取用户信息

        //测试
        UserInfo userInfo = new UserInfo();
        userInfo.setUserId("123");
        userInfo.setUserName("奥特曼打小怪兽");

        log.info(userInfo.toString());
        return userInfo.toQuery();

    }


    @Data
    @NoArgsConstructor
    public static class UserInfo{

        private String userId;

        private String userName;

        /**
         * 重写一下toString方案，使其返回&参数拼接的字符串，方便使用
         * @return
         */
        public String toQuery(){

            String query = "userId" +
                    '=' +
                    this.getUserId() +
                    '&' +
                    "userName" +
                    '=' +
                    this.getUserName();
            // 注意一定要使用URLEncoder.encode转码，否则传值会有编码问题
            return URLEncoder.encode(query, StandardCharsets.UTF_8);
        }
    }

}
```



在底层服务里面写一个接口

```java

	@PostMapping("/ccp")
    public String ccp(UserVo userVo)
    {
        ServletRequestAttributes requestAttributes = (ServletRequestAttributes)RequestContextHolder.getRequestAttributes();
        assert requestAttributes != null;
        HttpServletRequest request = requestAttributes.getRequest();

        System.out.println(request.getQueryString());

        return userVo.toString();
    }
```



![image-20201102141916561](C:\Users\wanghui\AppData\Roaming\Typora\typora-user-images\image-20201102141916561.png)



GET方法同样可以获得想要传递的参数



------

从下面章节开始，一些具体的微服务

### 7.用户服务

#### 1.用户表设计

#### 2.用户注册

+ 账号密码注册
+ 手机短信注册
  + 直接注册
  + 登录并注册
+ 第三方授权注册
  + 微信、QQ
  + 其他

#### 3.用户登录