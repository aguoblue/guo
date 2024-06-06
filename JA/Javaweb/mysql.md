# 安装
1. 下载zip，解压
2. 配置环境变量
   MYSQL_HOME  bin
3. 验证mysql
 ```
   >mysql
ERROR 2003 (HY000): Can't connect to MySQL server on 'localhost:3306' (10061)
```
4. 初始化mysql
   mysqld --initialize-insecure
   安装目录下会多一个data文件
5. 注册服务
   mysqld -install
   重复执行，会出现exist，以及注册位置路径 
6. 启动
   net start mysql
   net stop mysql
7. 设置密码
   mysqladmin -u root password 123456
8. 登录
   mysql -uroot -p