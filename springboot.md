

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

## 静态资源配置原理

### 1、配置过程

#### 自动配置类加载

SpringBoot默认加载自动配置类：xxxAutoConfiguration

#### 自动配置类生效

例如SpringMVC功能的自动配置类 WebMVCAutoConfiguration生效条件

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
@AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
      ValidationAutoConfiguration.class })
public class WebMvcAutoConfiguration {
```

#### 给容器中配了些什么

```java
@SuppressWarnings("deprecation")
@Configuration(proxyBeanMethods = false)
@Import(EnableWebMvcConfiguration.class)
@EnableConfigurationProperties({ WebMvcProperties.class,
      org.springframework.boot.autoconfigure.web.ResourceProperties.class, WebProperties.class })
@Order(0)
public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer {
```

#### 配置文件的相关属性和配置文件中xxx进行了绑定

* 配置文件一般为application.yml
* WebMvcProperties.class === @ConfigurationProperties(prefix = "spring.mvc")
* ResourceProperties.class === @ConfigurationProperties(prefix = "spring.resources", ignoreUnknownFields = false)
* WebProperties.class ===@ConfigurationProperties("spring.web")

### 2、扩展

#### 2.1、配置类中只有一个有参构造器

```java
//所有参数都能够从容器中获取
//WebProperties webProperties
//WebMvcProperties mvcProperties
//ListableBeanFactory beanFactory
//HttpMessageConverters
//ResourceHandlerRegistrationCustomizer  找到资源处理器的自定义器
//DispatcherServletPath
//ServletRegistrationBean
public WebMvcAutoConfigurationAdapter(WebProperties webProperties, WebMvcProperties mvcProperties,
				ListableBeanFactory beanFactory, ObjectProvider<HttpMessageConverters> messageConvertersProvider,
				ObjectProvider<ResourceHandlerRegistrationCustomizer> resourceHandlerRegistrationCustomizerProvider,
				ObjectProvider<DispatcherServletPath> dispatcherServletPath,
				ObjectProvider<ServletRegistrationBean<?>> servletRegistrations) {
```



#### 2.2、资源处理的默认规则

```
@Override
protected void addResourceHandlers(ResourceHandlerRegistry registry) {
   super.addResourceHandlers(registry);
   if (!this.resourceProperties.isAddMappings()) {
      logger.debug("Default resource handling disabled");
      return;
   }
   ServletContext servletContext = getServletContext();
   addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
   addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
      registration.addResourceLocations(this.resourceProperties.getStaticLocations());
      if (servletContext != null) {
         registration.addResourceLocations(new ServletContextResource(servletContext, SERVLET_LOCATION));
      }
   });
}
```

#### 2.3、欢迎页处理规则

```
@Bean
public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext,
      FormattingConversionService mvcConversionService, ResourceUrlProvider mvcResourceUrlProvider) {
   WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(
         new TemplateAvailabilityProviders(applicationContext), applicationContext, getWelcomePage(),
         this.mvcProperties.getStaticPathPattern());
   welcomePageHandlerMapping.setInterceptors(getInterceptors(mvcConversionService, mvcResourceUrlProvider));
   welcomePageHandlerMapping.setCorsConfigurations(getCorsConfigurations());
   return welcomePageHandlerMapping;
}

要想使用欢迎页，静态资源路径前缀必须是/**,不能自定义前缀
WelcomePageHandlerMapping(TemplateAvailabilityProviders templateAvailabilityProviders,
			ApplicationContext applicationContext, Resource welcomePage, String staticPathPattern) {
		if (welcomePage != null && "/**".equals(staticPathPattern)) {
			logger.info("Adding welcome page: " + welcomePage);
			setRootViewName("forward:index.html");
		}
		else if (welcomeTemplateExists(templateAvailabilityProviders, applicationContext)) {
			logger.info("Adding welcome page template: index");
			setRootViewName("index");
		}
	}
```





## 请求参数处理

### 1、请求映射

#### 1、rest使用与原理

* xxxMapping
* Rest风格支持（使用http请求方式动词来表示对资源的处理）
  * GET-获取   POST-存储   DELETE-删除   PUT-修改
  * 核心Filter；HiddenHttpMethodFilter
    * 表单: method=POST，隐藏域 _method = put
    * 手动在SpringBoot中开启

```java
//默认关闭
@Bean
@ConditionalOnMissingBean(HiddenHttpMethodFilter.class)
@ConditionalOnProperty(prefix = "spring.mvc.hiddenmethod.filter", name = "enabled", matchIfMissing = false)
public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter() {
   return new OrderedHiddenHttpMethodFilter();
}
```

```yaml
手动开启
spring:
  mvc:
    hiddenmethod:
      filter:
        enabled: true
```

Rest原理（基于表单提交使用REST）

* 表单提交会带上_method=PUT
* 请求过来被HiddenHttpMethodFilter进行拦截
  * 判断请求是否正常，且请求方式必须是POST
    * 获取_method的值，值支持PATCH、PUT和DELETE
    * 原生request请求（post），包装模式requestWrapper重写了getMethod方法，返回_method传入的值
    * 过滤器链放行时，使用的时Wrapper。以后再调用时，使用的也是Wrapper的返回值。

Rest使用工具时（postMan）可以直接传入PUT、DELETE等请求方式

#### 2、请求映射原理

![image-20210314094907082](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20210314094907082.png)

doGet->processRequest->doService->doDispatch

### 2、



















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