# spring Boot学习二
## 一. yaml相关

### 1. @Value获取值和@ConfigurationProperties获取值比较

- 功能： @Value只能单个注入，而后者是批量注入
- 松散绑定： 前者不支持松散绑定，后者支持
- SqEl： 前者支持，后者不支持。     
    e.g. @Value("#{11*2}")
- JSR303数据校验：前者不支持，后者支持。    
    e.g. @Validated @Email
- 复杂类型封装：前者不支持，后者支持。    
    e.g. List、Map

总结：在获取配置文件某个或几个值时使用@Value，如果我们编写了一个javaBean和配置文件进行映射使用@ConfigurationProperties

### 2. 配置文件注入-数据校验
```
    @Component // 容器
    @ConfigurationProperties(prefix = "persion")
    @Validated // 数据校验
    public class Persion {
        
        @Email
        private String name;
        
        private Integer age;
        private Map<String, String> maps;
        private List<String> lists;
        private TestUser users;
    }

```

### 3. @PropertySource  @ImportResource
- @PropertySource: 加载指定的配置文件、可以加载多个配置文件   
    a)语法:@PropertySource(value = {"classpath:..."})

- @ImportResource: 导入spring的配置文件，让配置文件的内容生效    
    a)spring boot 没有spring的相关配置文件，自己写的配置文件不会自动生效；但是这个只能配置单个文件   
    b)语法： @ImportResource(locations = {"classpath:......"}) 

### 4. spring boot推荐给容器添加组件的方式-推荐使用全注解模式

- 配置类======sprig配置文件
- 使用@Bean注解为容器添加组件

### 5. 配置文件占位符
- 随机数
```
${random.int}、${random.int(10)}、${random.value}......
```

- 占位符   
    1)获取配置文件中配置过的值
    2)使用:设置默认值
```
    persion:
      name: 张三${random.uuid}
      age: ${random.int}
      maps: {g: goods, o: goods, s: study}
      lists:
        + one
        + two
        + three
      users:
        username: ${persion.hello:sd}_little
        userage: ${persion.age}
```

### 6.Profile
    1) 多Profile文件   
        可以配置多个application-{profile}.properties文件，默认生效的是application.properties文件


    2）yml支持多文档块方式   

```
    
    # 激活yml文档块
    spring:
      profiles:
        active:
        - prod

    server:
      port: 8080

    persion:
      name: 张三${random.uuid}
      age: ${random.int}
      maps: {g: goods, o: goods, s: study}
      lists:
        - one
        - two
        - three
      users:
        username: ${persion.hello:sd}_little
        userage: ${persion.age}

    ---

    spring:
      profiles: dev

    server:
      port: 8081
      
    ---
    spring:
      profiles: prod

    server:
      port: 8082

```

    3）激活指定Profile   

a) 在配置文件中激活 e.g. spring.profiles.active = ${profile}    
b) 命令行方式 ：--spring.profiles.active=${profile}

- IDE工具中的Program arguments中配置命令
- 一个是打成jar包形式 使用命令运行

c) 在虚拟机中配置：
    VM options配置(-Dspring.profiles.active=${profile})
    
## 二、配置文件加载顺序
    
1. 默认扫描顺序(**优先级从低到高**，高优先级会**覆盖**低优先级配置内容)  
    spring boot 会从这四个地方加载所有的配置，互补配置：配置文件之间相互互补
  - file:./config/ 
  - file：./
  - classpath:/config/
  - classpath:./
2. 可以通过配置spring.config.location改变默认配置
  在项目运维时，在命令行进行配置（项目打包后），同样满足互补配置原则

## 三、外部配置加载顺序

具体可以查看spring boot的[中文官方文档](https://www.breakyizhan.com/springboot/3028.html) 






    



