# Ioc学习
## 一、ApplicationContext 的应用
  ### 1.ApplicationContext的获取方式
#### 1) 通过ClassPathXMLApplicationContext类获得(一般适用于测试)
         `ApplicationContext context = new ClassPathXmlApplicationContext("*.xml");`

#### 2) 通过WebApplicationUtils工具类获取(依赖Servlet容器)。
         `ApplicationContext ap = WebApplicationUtils.getWebApplicationContext(Servlet容器参数)`

#### 3) 创建自己的工具类。
         用自己创建的工具类实现Spring的ApplicationContextAware接口。
        最后在Spring配置文件中注册自己的工具类。

        `<bean id="springContextHolder" class="com.fubo.utils.spring.SpringContextHolder" lazy-init="false"/>`

        lazy-inti="false" 表示立即加载。 lazy-inti="true"  表示延时加载。使用后者bean将会在第一次向容器通过getBean方法时被实例化。

## 二、Ioc容器

  ### 1.容器实现

  BeanFactory提供了Ioc容器的基本功能，ApplicationContext增加了更多企业级功能支持
- XmlBeanFactory：BeanFactory实现，可以从classpath或文件系统获取资源       
```
        File file = new File("*.xml");
        Resource resource = new FileSystemResource(file);
        BeanFactory beanFactory = new XmlBeanFactory(resource);
```

        或者是
```
        Resource resource = new  ClassPathResource("*.xml");                 
        BeanFactory beanFactory = new XmlBeanFactory(resource);
```

- classpathXmlApplicationContext：ApplicationContext实现，从Classpath获取配  置文件。`BeanFactory resource = new ClassPathResource("*.xml");`

- FileSystemXmlAApplicationContext:ApplicationContext实现，从文件系统获取配置文件。`BeanFactory b = new FileSystemXmlApplicationContext("*.xml")`

   ### 2.实例化Bean
     #### 1) 使用构造器实例化
      在id存在时，id是标识符，name是别名，id不存在时name是标识符，name存在多个值时(,、;隔开)若id不存在，则第一个值是标识符其他是别名，反之则都是别名。id和name都是唯一值,
         
         a.空构造器

         `<bean id="A" name="B;c,d" class="包名.类名" />`
         b.有参构造器

```
         <bean id="A1" class="包名.类名">
            <!-- 指定参数 index是位置 value是指定参数值 -->
            <constructor-arg index="0" value="123" />
         </bean>
```

     #### 2) 使用静态工厂实例化

```
         <bean id="A2" class="包名.类名" factory-method="类名中需要用到的方法">
         </bean>
```

     #### 3) 使用实例工厂实例化

           先定义实例工厂

`<bean id="A3" class="包名.类名" />`

           在使用定义好的实例工厂创建bean
```
          <bean id="A4" factory-bean="A3" factory-method="类名中需要用到的方法">
            <!-- 需要参数就定参数，不需要就不用写 -->
          </bean>
 ```
