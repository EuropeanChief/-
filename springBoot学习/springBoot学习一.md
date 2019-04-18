# 一. spring boot 入门
## 前情提要
spring boot是对spring框架的再封装，也是对J2EE开发的一站式解决方案，spring boot的设计目的是用来简化spring应用的初始搭建以及开发过程
### 优点：
1.大量的自动配置，简化了开发过程.
2. 不需要配置大的xml文件，也没有代码生成
2. 使用了内嵌的serlvet容器，应用打包成jar就可以直接运行。
3. starters自动依赖与版本控制
4. 在运维期间可以进行应用监控
5. 与云计算天然的集成

## 使用 spring initializer 创建spring boot项目

### 1.默认生成的spring boot项目结构
  
  resources
    - static： 一般存放静态文件，如：js、images、css或者一些静态的html文件
    - templates： 存放模板页面
    - application.properties: 是spring boot 默认配置文件，不可更换名字。

### 2.spring boot 配置

1) 配置文件

  - spring boot 使用一个全局的配置文件
      application.properties 和 application.yml
  - yml：yml是YAML语言的文件，以数据为中心

2) YAML语法   
      
  a. 基本语法

- k: v: 值
- 属性和值区分大小写
- 以空格的缩进表示层级：port和path表示同一级

```
    server:
    port: 8089
    path: /TestUser
```


b. 值的写法

- 对象/Map

    键值对写法
```
    TestUser:
      username: goods
      userage: 20
```
          
  行内写法

```
              TestUser: {username: goods,userage: 20}
```
   
   - 数组
    
    用 - 值表示
```
    List:
      - one
      - two
      - three
```

行内写法
          
```
    List: [one,two,three]
```
  
  3) 配置文件值注入

  pom.xml文件导入相关启动器（个启动器是辅助我们开发的，导入配置文件处理器后在配置文件中进行相关绑定会有提示，不配置也可以）

```
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-configuration-processor</artifactId>
      <optional>true</optional>
    </dependency>
```

  yaml文件：

```
      persion:
        name: lily
        age: 20
        maps: {g: goods, o: goods, s: study}
        lists:
          - one
          - two
          - three
        users:
          username: little
          userage: 20
```
  
  bean文件:

```
@Component // 容器
@ConfigurationProperties(prefix = "persion")
public class Persion {

  private String name;
  private Integer age;

  private Map<String, String> maps;
  private List<String> lists;
  private TestUser users;

}
```

  

## pom.xml文件

1. 父项目

```
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.19.RELEASE</version>
    <relativePath /> <!-- lookup parent from repository -->
  </parent>

  <!-- spring boot 自动配置的依赖默认版本在这个父项目中 -->
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>1.5.19.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
  </parent>

```

2. 部分starter解析

```
    <!-- web项目主要的依赖 -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- 进行单元测试的模块 -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>

    <!-- 热部署-在完整打包环境下会被禁用 -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-devtools</artifactId>
      <optional>true</optional>
    </dependency>

    <!-- 动态html访问路径 -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>

    <!-- 可以将应用打包成jar包的插件 -->
    <build>
      <plugins>
        <plugin>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
      </plugins>
    </build>
```