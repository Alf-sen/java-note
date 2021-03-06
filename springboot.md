# Spring Boot

## 容器功能

### 组件装配到容器中

* @Configuration

  ```xml
  将类定义为配置类
  proxyBeanMethods:
   1、默认为true，为full模式，调用组件时都要检查容器中是否已经存在该组件，如果存在则直接调用，不存在时需要新创建一个。
   2、设置为false时为lite模式，每次调用时都新创建一个，无需检查，速度快。
   3、当配置组件之间没有依赖时，使用lite模式，速度快；当配置组件之间有依赖关系，方法会调用得到之前的单实例组件，用full模式
  ```

* @Bean、@Component、@Service、@Controller、@Repository

  * 注解的派生性 ` @Component`` 
    * ` @Service` 服务层
    * `@Controller` 控制层
    * `@Repository` 数据层

* 

  `@Bean标记方法,其余的标记类，将类标记为组件并装配到容器中`

* @Import

  ``@Import({AutoConfigurationImportSelector.class})将AutoConfigurationImportSelector组件导入容器中 ``

* @Conditional

  `条件装配：当满足conditional指定的条件时，才会进行组件进行注入`

* @ImportResource

  `可将原生的xml配置的组件导入到容器中`

* @ConfigurationProperties配置绑定(首先要将配置的类注入到容器中)

  * ```
    @EnableConfigurationProperties(User.class) + @ConfigurationProperties(prefix = "mouser") + application.yml
    @EnableConfigurationProperties要在配置类中使用，两个作用：
     1、为User类开启配置绑定功能
     2、将User类注入到容器中
    @ConfigurationPropertie作用再User类上
    “mouser”与application.yml中的某个配置的前缀名相同。
    ```

  * ```
     @Component+@ConfigurationProperties(prefix = "mouser") + application.yml
    ```

    

## Spring Boot自动装配

### 底层实现@SpringBootApplication

#### 1、@SpringBootConfiguration

​	将类定义为配置类，等同于spring的Configuration。

#### 2、@ComponentScan

​	指定扫描那些组件

#### 3、@EnableAutoConfiguration

```java
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
```

##### 3.1、@AutoConfigurationPackage

```java
自动配置包
    @Import({Registrar.class})
    利用Registrar批量给容器导入主类所在包下的所有组件
```

##### 3.2、@Import(AutoConfigurationImportSelector.class)

```java
1、public String[] selectImports(AnnotationMetadata annotationMetadata)给容器导入组件
2、List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);获取到所有需要加载的组件
3、利用工厂加载private static Map<String, List<String>> loadSpringFactories(ClassLoader classLoader) 需要的组件
4、从META-INF/spring.factories 位置加载文件
```



#### Spring工厂加载机制

* 实现类 ：`SpringFactoriesLoader`

* 配置资源：`META-INF/spring.factories`


### 实现方法

1. 激活自动装配：`@EnableAutoConfiguration`

2. 实现自动装配：`xxxAutoConfiguration`

3. 配置自动装配实现：`META-INF/spring.factories`

   









## Servlet三大组件

### servlet组件

+ 实现

  + ```
    @WebServlet
    ```

  + implements HttpServlet

+ URL映射

  + ```
    urlPatterns = "/my/servlet"
    ```

+ 注册

  + ```
    @ServletComponentScan
    ```

## 开发二部svn地址：svn://172.24.177.38/dev2_out/dev