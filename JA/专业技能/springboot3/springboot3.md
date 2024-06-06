# 基础篇
## 起步依赖

## 配置文件
![[Pasted image 20240409155750.png]]
## yml配置文件获取
![[Pasted image 20240409155931.png]]
## Bean扫描
![[Pasted image 20240409155523.png]]
## Bean注册

![[Pasted image 20240409190749.png]]
## 注册条件
当配置文件里没有该属性时找不到报错，如何处理
![[Pasted image 20240409191100.png]]
条件
![[Pasted image 20240409191046.png]]

## 自动配置原理

@SpringBootApplication
组合注解
	@EnableAutoConfiguration
		@Import(AutoConfigurationImportSelector.class)
			AutoConfigurationImportSelector间接实现了 `ImportSelector` 接口，找配置文件（2.7前META-INF/spring.factories，3.0后AutoConfiguration.imports）里面的类
			配置文件里的类需要有@AutoConfiguration，这个注解组合了@Configuration，也是配置类，以及一些其他的注解比如@ConditionalOnClass
			

![[Pasted image 20240410135312.png]]
![[Pasted image 20240410135423.png]]![[Pasted image 20240410135623.png]]

## 自定义starter




## 实战篇




![[Pasted image 20240410200708.png]]
## JWT令牌
