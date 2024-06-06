
# 快速入门
[入门_MyBatis中文网](https://mybatis.net.cn/getting-started.html)
1、依赖
2、配置文件xml  mybatis-config.xml配置文件，数据库信息、加载sql映射文件
3、Mapper.xml namespace  id  resultType  sql语句
4、实体类
5、加载mybatis-config.xml配置文件，获取SqlSessionFactory
6、获取SqlSession对象，用它执行sql
7、释放资源
# Mapper 代理开发    代理！！！
## 快速入门
![[Pasted image 20240405202429.png]]
![[Pasted image 20240405154117.png]]
## 核心配置
[配置_MyBatis中文网](https://mybatis.net.cn/configuration.html)
配置有顺序
类型别名（typeAliases）
```
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```

## 结果映射
列名和属性名不能匹配上，，可以在 SELECT 语句中设置列别名（这是一个基本的 SQL 特性）来完成匹配
提取别名
	< sql   id = "" /sql>提取     
	< include refid  />  应用
使用外部的 resultMap   
	< resultMap id = ""   type = "" >   
		< result    column = ""    property = "" />
		< result    />
	< / resultMap>
	在< select>使用resultMap属性替换 resultType属性


## 参数占位符
#{}  会将其替换为?，防止sql注入
${}  拼sql，存在SQL注入问题
参数类型
特殊字符
	1、转义字符
	2、CDATA区

## 多个参数
![[Pasted image 20240405162220.png]]
![[Pasted image 20240405162150.png]]

![[Pasted image 20240405190854.png]]

## 动态条件查询

单个条件动态
![[Pasted image 20240405162317.png]]

多个条件动态
![[Pasted image 20240405184458.png]]


主键返回


## 注解
![[Pasted image 20240405192118.png]]

### mapper.xml  + 注解 混合使用


### 注解情况下，列名和属性名不能匹配上怎么办


### 在完全使用注解方式定义SQL并且不使用XML配置文件的场景下，为实体类指定MyBatis别名确实不是必需的，可能也没有太大的意义






