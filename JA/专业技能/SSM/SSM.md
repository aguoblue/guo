简化开发、框架整合


# Spring
配置 -> 注解
## 家族
Spring Framework
Spring Boot
Spring Cloud


## Spring Framework 系统架构
![[Pasted image 20240317191743.png]]

## 学习路线
![[Pasted image 20240317191829.png]]

## 核心概念

### IoC  Inversion of Control  控制反转
起因    代码书写耦合度偏高
解决    使用对象时，程序中不主动使用new，转为外部提供，程序要提供入口
概念    对象的创建控制权由程序转移到外部
Spring提供了一个IoC容器，充当外部，容器负责对象的创建、初始化等，被创建或者管理的对象在IoC容器中统称为Bean
IoC案例
	导入Spring坐标
	定义Spring管理的类（接口）
	创建Spring配置文件XML，配置对应类作为Spring管理的bean
		bean id class
	初始化IoC容器，通过容器获取bean
	注意：此时还未解耦，程序里还是new
	此处验证：新建容器，从容器中得到容器自动创建的service等对象，调用该对象的方法。
### DI    Dependency Injection 依赖注入
在容器中建立bean和bean之间的依赖关系的整个过程
DI案例
	删除new 
	生成set方法，用来设置要使用的对象
	在配置文件中添加容器中bean的关系 property标签里的name ref
### bean
![[Pasted image 20240405002946.png]]
基础配置
	id  
	class 
	name别名 
	scope 默认单例singleton  prototype
![[Pasted image 20240405000400.png]]
实例化
	构造方法
		使用无参构造
	静态工厂
		配置文件对应的工厂对象设置factory-method属性
	实例化工厂
		factory-method  factory-bean
	spring提供  bean类需要实现FactoryBean<>接口
		getObject()    返回创建的实现对象
		getObjectType()  指定类型
		isSingleton()  是否单例
生命周期
	关容器前触发bean的销毁
	![[Pasted image 20240405001234.png]]
setter注入
	简单类型  value
	引用类型  
构造器注入
集合注入

### 依赖自动装配 
配置文件里面bean的属性autoware 按类型、名字
![[Pasted image 20240405001651.png]]
![[Pasted image 20240405001856.png]]
### 创建容器
![[Pasted image 20240405002453.png]]
### 获取Bean
![[Pasted image 20240405002538.png]]


## 注解开发

### 注解开发定义Bean
@Component("name") 代替< Bean   /Bean>，并在xml文件中补充上扫描位置
![[Pasted image 20240405003059.png]]
![[Pasted image 20240405003220.png]]

@Scope("prototype") 修改为非单例，不加这个注解，Bean默认单例


### 纯注解开发，其他
#### 配置类
@Configuration代替xml，用于设定当前类为配置类
@ComponentScan代替了 < context:component-scan  ···  /> ，用于设定扫描路径，此注解只能添加一次，多个数据用数组格式
![[Pasted image 20240405003859.png]]
![[Pasted image 20240405003537.png]]

#### 方法调用顺序
方法上注解
	@PostConstruct 构造方法后
	@PreDestroy   销毁前
#### 依赖注入，自动装配
##### Bean注入装配，默认按类型

@Autowired  可以不要set注入(入口)，暴力反射
@Qyalifer("BeanName")  假如有多个类型相同的Bean，可以给Bean起名字BeanName，并在注入地方加上@Qyalifer("BeanName")
![[Pasted image 20240405093737.png]]![[Pasted image 20240405093918.png]]

##### 简单类型注入
@Value("")
###### 程序中指定
![[Pasted image 20240405094139.png]]
###### 外部文件指定
@PropertSource   加载外部文件，多文件使用数组格式，不能使用通配符*
![[Pasted image 20240405094741.png]]
![[Pasted image 20240405094806.png]]

### 第三方Bean
@Bean 注解一个方法，方法返回想要放入容器的对象，默认单例
#### 直接在配置类中添加@Bean
配置类中的方法可以使用`@Bean`注解，这表明该方法将返回一个对象，该对象要注册为Spring应用上下文中的bean。这些bean可以被Spring容器自动检测和管理。
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

    @Bean
    public MyBean myBean() {
        // 返回MyBean的实例
        return new MyBean();
    }
}

//  ----------------   或者  -------------- 

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import javax.sql.DataSource;
import org.apache.commons.dbcp2.BasicDataSource;

@Configuration
public class DatabaseConfig {
    
    @Bean
    public DataSource dataSource() {
        BasicDataSource dataSource = new BasicDataSource();
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/mydb");
        dataSource.setUsername("user");
        dataSource.setPassword("password");
        return dataSource;
    }
}
```

#### 在其他类中定义，在配置类中导入
##### 两种导入
![[Pasted image 20240405102735.png]]
![[Pasted image 20240405102745.png]]
虽然这些没有`@Configuration`注解的类中的@Bean方法可以被注册，但是这些类不会被Spring容器视为完全的配置类，这意味着其中@Bean方法返回的bean不会享有某些`@Configuration`类提供的高级特性，如CGLIB代理，这可能会影响到如@Scope("prototype")这样的注解的行为。
##### 第三方Bean依赖注入及自动装配
外部文件
![[Pasted image 20240405103108.png]]
根据类型
![[Pasted image 20240405103128.png]]


## Spring整合Mybatis
![[Pasted image 20240405205120.png]]
![[Pasted image 20240405204952.png]]
![[Pasted image 20240405205009.png]]
![[Pasted image 20240405204922.png]]

## AOP
### 概念和流程
![[Pasted image 20240406094849.png]]
![[Pasted image 20240406095533.png]]
1、导坐标
2、在配置类开启
![[Pasted image 20240406095946.png]]
3、定义通知类，通知，切入点，把切入点和通知绑定 
4、把通知类放入容器，注解该类为aop
![[Pasted image 20240406100119.png]]![[Pasted image 20240406191340.png]]
![[Pasted image 20240406191355.png]]
### 切入点表达式
![[Pasted image 20240406191725.png]]
### 通知类型
![[Pasted image 20240406192425.png]]
![[Pasted image 20240406193054.png]]
![[Pasted image 20240406195452.png]]
![[Pasted image 20240406193130.png]]
![[Pasted image 20240406195444.png]]
![[Pasted image 20240406193417.png]]
![[Pasted image 20240406193427.png]]
![[Pasted image 20240406195118.png]]

### 获取参数
#### 调用参数
![[Pasted image 20240406195953.png]]
#### 返回参数
![[Pasted image 20240406200038.png]]
![[Pasted image 20240406200056.png]]
#### 一起用
![[Pasted image 20240406200112.png]]

### 用途
测效率、时间
校验

## 事务
@Transactional
![[Pasted image 20240406204654.png]]
![[Pasted image 20240406204706.png]]
@EnableTransactionManagement
![[Pasted image 20240406204728.png]]

### 事务角色
![[Pasted image 20240406205022.png]]

### 事务相关配置
![[Pasted image 20240406205204.png]]

### 事务传播行为
![[Pasted image 20240406210746.png]]
![[Pasted image 20240406210754.png]]


# SpringMVC
![[Pasted image 20240406222144.png]]


## bean控制加载

![[Pasted image 20240406232444.png]]
![[Pasted image 20240406232456.png]]
![[Pasted image 20240406232420.png]]

## 请求与响应
### 请求路径
![[Pasted image 20240406232614.png]]
### 请求方式


### 请求参数

![[Pasted image 20240406232800.png]]
![[Pasted image 20240406232835.png]]
![[Pasted image 20240406232859.png]]
![[Pasted image 20240406232911.png]]
![[Pasted image 20240406232921.png]]
![[Pasted image 20240406232946.png]]
![[Pasted image 20240406233010.png]]

### json数据传递参数
@EnableWebMvc
![[Pasted image 20240406233159.png]]
![[Pasted image 20240406233208.png]]
![[Pasted image 20240406233233.png]]
![[Pasted image 20240406233253.png]]

### 日期
![[Pasted image 20240406233317.png]]

### 响应
![[Pasted image 20240406233545.png]]
没有@ResponseBody，则跳转
![[Pasted image 20240406233733.png]]
![[Pasted image 20240406233827.png]]
@ResponseBody把对象、集合等响应转json，依赖了json转换
![[Pasted image 20240406233635.png]]
![[Pasted image 20240406233558.png]]

## Rest风格
![[Pasted image 20240406232325.png]]
![[Pasted image 20240406232313.png]]

![[Pasted image 20240407092639.png]]
![[Pasted image 20240407092629.png]]
![[Pasted image 20240407092546.png]]
@RestController
![[Pasted image 20240407093200.png]]
`@RequestMapping` 的快捷方式变体
![[Pasted image 20240407093224.png]]
静态页面的访问放行  需要被SpringMvcConfig加载到
![[Pasted image 20240408095334.png]]
## SSM整合
1、jdbc、mybatis、servlet、spring、springmvc配置类
![[Pasted image 20240407100329.png]]
2、controller  restful风格、service  接口、service实现类、 dao 接口 由mybatis自动代理
依赖注入和自动装配
3、统一数据返回结果类
4、异常处理器 
![[Pasted image 20240407101435.png]]
![[Pasted image 20240407101257.png]]
5、异常分类
6、前后台协议联调

## 拦截器
### 简介
![[Pasted image 20240408093814.png]]
![[Pasted image 20240408093805.png]]
1、访问SpringMvc
![[Pasted image 20240408093857.png]]
2、访问静态资源还是走controller，如果走静态资源，则不走controller
3、如果走controller，可以在这之前添加自定义的拦截器，并指定拦截器的拦截规则
![[Pasted image 20240408104138.png]]
addInterceptors 添加拦截器
addResourceHandlers 指定静态资源的位置以及对应的URL模式
![[Pasted image 20240408104221.png]]
上面两个都需要被SpringMvcConfig扫描加载到
![[Pasted image 20240408104537.png]]
另一种直接与SpringMvcConfig绑定
![[Pasted image 20240408104426.png]]


### 拦截器参数
![[Pasted image 20240408105452.png]]
![[Pasted image 20240408105509.png]]
![[Pasted image 20240408105515.png]]


### 拦截器链
先加载先拦截
![[Pasted image 20240408105750.png]]

# Maven
分模块

引入模块作为依赖

install


依赖传递
![[Pasted image 20240408112206.png]]
![[Pasted image 20240408112342.png]]

可选依赖   自己模块里的依赖不想给其他模块使用
![[Pasted image 20240408112508.png]]
排除依赖      从依赖的模块去掉一些不需要的依赖
![[Pasted image 20240408112608.png]]

聚合

继承

# SpringBoot
1、执行maven package，对boot项目打包
2、
![[Pasted image 20240408153320.png]]

## 概述
![[Pasted image 20240408153457.png]]

yaml格式
![[Pasted image 20240408200245.png]]
多环境
![[Pasted image 20240408200506.png]]

