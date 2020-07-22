# 加载应用上下文

1. AnnotationConfigApplicationContext

从一个或多个基于java的配置类中加载Spring应用上下文

2. AnnotationConfigWebApplicationContext

从一个或多个基于java的配置类中加载Spring Web应用上下文

3. ClassPathXmlApplicationContext

从类路径下的一个或多个XML配置文件中加载上下文定义，把应用上下文的定义文件作为类资源

4. FileSystemXmlApplicationContext

从文件系统中的一个或多个XML配置文件中加载上下文定义

5. XmlWebApplicationContext

从Web应用下的一个或多个XML配置文件中加载上下文定义

# java类配置bean

## 自动装配

在需要创建Bean的类上使用注解@Component,在需要使用Bean的地方使用注解@Autowired,创建java 配置类如下：

```
@Configuration
@ComponentScan
public class SpringConfig {

}

```

@ComponentScan 默认只扫描SpringConfig所在当前包，可以为其指定报名如：

@ComponentScan(basePackages="com.chen.aa")   按包名，可以是数组

@ComponentScan(basePackageClasses={AA.class, BB.class})     按类名，可以是数组

@Autowired(required=true) 不知道装配哪个Bean时抛 BeanCreationException 异常

@Autowired(required=false) 不知道装配哪个Bean时抛空指针异常

不知道装配哪个Bean时，使用@Qualifier可以指定注入Bean的名称

_Spring Boot里入口类的@SpringBootApplication默认只扫描当前类所在的包及子包，将入口类放置在所有子包外面以保证默认扫描到Bean，如果需要指定扫描包可使用 @SpringBootApplication(scanBasePackages="com.example")配置_

## 配置Bean

使用注解@Bean配置Bean, @Bean只能作用在方法上

```
// CarConfig.class 配置类
@Configuration
public class CarConfig {
    
    @Bean
    public Car car() {
        return new BmwCar();
    }
    
    @Bean
    public People people() {
        People people = new People();
        // 依赖注入，People类需要有Car的setter方法
        people.setCar(car());
        return people;
    }
}
```

```
// People.class People类 不需要使用@Autowired
public class People {
    
    private Car car;
    
    public void setCar(Car car) {
        this.car = car;
    }

    public void buyCar() {
        car.drive();
        System.out.println("buy");
    }
}
```

# Spring Boot

查看自动配置需要加载的默认值  在启动应用程序时加入 --debug 参数

使用 ` @EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class}) `排除自动配置类

## 指定加载配置文件

@ConfigurationProperties(prefix = "test", locations = "classpath:xxxx.properties")

# Spring Web

# Java Config 配置类替代Web.xml(只支持Servlet 3.0以上版本)
在 Servlet 3.0 环境下，Servlet 容器会在 classpath 下搜索实现了 javax.servlet 
.ServletContainerInitializer 接口的任何类，找到之后用它来初始化 Servlet 容器。

Spring 实现了以上接口，实现类叫做 SpringServletContainerInitializer， 它会依次搜寻实现了 WebApplicationInitializer的任何类，并委派这个类实现配置。之后，Spring 3.2 开始引入一个简易的 WebApplicationInitializer 实现类，这就是 AbstractAnnotationConfigDispatcherServletInitializer。

所以 SpittrWebAppInitializer 继承 AbstractAnnotationConfigDispatcherServletInitializer之后，也就是间接实现了 WebApplicationInitializer，在 Servlet 3.0 容器中，它会被自动搜索到，被用来配置 servlet 上下文。

可以直接实现 WebApplicationInitializer ，也可以实现AbstractAnnotationConfigDispatcherServletInitializer

```

package spittr.config;

import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

public class SpittrWebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer  {

    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class<?>[] {RootConfig.class};
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class<?>[] {WebConfig.class};
    }

    @Override
    protected String[] getServletMappings() {
        return new String[] {"/"};
    }

}

```

```

package spittr.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.
DefaultServletHandlerConfigurer;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.
WebMvcConfigurerAdapter;
import org.springframework.web.servlet.view.
InternalResourceViewResolver;

@Configuration
@EnableWebMvc
@ComponentScan("spittr.web")
public class WebConfig extends WebMvcConfigurerAdapter {

    @Bean
    public ViewResolver viewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".jsp");
        resolver.setExposeContextBeansAsAttributes(true);
        return resolver;
    }

    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }
}

```

```

package spittr.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.ComponentScan.Filter;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;

@Configuration
@ComponentScan(basePackages={"spittr"},
excludeFilters={
    @Filter(type=FilterType.ANNOTATION, value=EnableWebMvc.class)
})

public class RootConfig {}

```

# Spring Security

开启Spring Security，在配置类上注解  `@EnableWebSecurity`

## HttpSecurity
配置类继承 WebSecurityConfigurerAdapter 重写protected void configure(HttpSecurity http)

```

protected void configure(HttpSecurity http) throws Exception {
    http
        .authorizeRequests()
            .anyRequest().authenticated()
            .and()
        .formLogin()
            .loginPage("/login")//第1
            .permitAll();       //第2
}

```

第1 指定登录页面
第2 formLogin().permitAll()方法允许所有用户无需授权就能访问登录页面

## 自定义验证类

实现 UserDetailsService 的 loadUserByUsername 方法

```

@Bean
public SpringDataUserDetailsService springDataUserDetailsService() {
    return new SpringDataUserDetailsService();
}

// 密码加密算法
@Bean
public BCryptPasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}

```
