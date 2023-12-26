1.安装
[安装与卸载 · Docker -- 从入门到实践 (docker-practice.github.io)](https://docker-practice.github.io/zh-cn/compose/install.html)
docker -v
配置镜像加速

2.镜像操作
拉取镜像 docker pull nginx
查看镜像 docker images
保存镜像为tar文件 docker save 【Options】docker save -o nginx.tar nginx:lastest
导入tar文件为镜像 docker load -i nginx.tar
删除镜像 docker rmi nginx

3.镜像使用 基于镜像新建一个容器
	镜像（`Image`）和容器（`Container`）的关系，就像是面向对象程序设计中的 `类` 和 `实例` 一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。
容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的 [命名空间](https://en.wikipedia.org/wiki/Linux_namespaces)。因此容器可以拥有自己的 `root` 文件系统、自己的网络配置、自己的进程空间，甚至自己的用户 ID 空间。容器内的进程是运行在一个隔离的环境里，使用起来，就好像是在一个独立于宿主的系统下操作一样。这种特性使得容器封装的应用比直接在宿主运行更加安全

容器运行 docker run  基于官方docker hub查看运行命令
	docker run --name mynginx -d -p 80:80 nginx
容器暂停、恢复 docker pause  mynginx  |  docker unpause  mynginx
容器停止、运行 docker stop mynginx  |  docker start mynginx
进入容器 docker exec
	docker exec -it mynginx bash 
		配置文件位置、资源文件位置等具体查看官网文档
		比如修改index.html文件  /usr/share/nginx/html 下有index.html
	退出 exit
容器日志 docker logs
查看所有容器状态 docker ps -a
容器删除 docker rm mynginx

4.容器与数据耦合问题
每次创建容器需要进入到容器内部修改配置，不便修改
容器内部创建的数据不便修改，容器升级不得不删除旧容器，数据也删除了

镜像使用的是分层存储，容器也是如此。每一个容器运行时，是以镜像为基础层，在其上创建一个当前容器的存储层，我们可以称这个为容器运行时读写而准备的存储层为 **容器存储层**。
容器存储层的生存周期和容器一样，容器消亡时，容器存储层也随之消亡。因此，任何保存于容器存储层的信息都会随容器删除而丢失。

按照 Docker 最佳实践的要求，容器不应该向其存储层内写入任何数据，容器存储层要保持无状态化。所有的文件写入操作，都应该使用 [数据卷（Volume）](https://docker-practice.github.io/zh-cn/data_management/volume.html)、或者 [绑定宿主目录](https://docker-practice.github.io/zh-cn/data_management/bind-mounts.html)，在这些位置的读写会跳过容器存储层，直接对宿主（或网络存储）发生读写，其性能和稳定性更高。

数据卷的生存周期独立于容器，容器消亡，数据卷不会消亡。因此，使用数据卷后，容器删除或者重新运行之后，数据却不会丢失。
数据卷
	虚拟目录，指向宿主机文件系统的某个文件，产生关联
docker volume
	创建数据卷 create   | docker volume html
	显示 inspect  |  docker volume inspect html  查看数据卷存储目录mountpoint
	列出 ls  | docker volume ls
	删除未使用的 prune  |  docker volume prune
	删除指定 rm  docker volume rm html
如何挂载数据卷
	docker run \
	--name nynginx \
	-v html:/root/html \
	-p 8080:80 \
	-d nginx

5.镜像结构

docker commit

6.自定义镜像
Dockefile
docker build -t javaweb:1.0 .
docker run --name web -p 8090:8090 -d javaweb:1.0

7.docker compose
通过docker-compose.yml部署多个容器
docker compose up -d
docker ps
docker compose logs -f
docker compose restart gateway userservice orderservice

8.docker镜像仓库
	1官方简化版镜像仓库
	2图形化界面
		打开要修改的文件
		vi /etc/docker/daemon.json
		添加内容：
		"insecure-registries":["http://192.168.150.101:8080"]
		重加载          systemctl daemon-reload
		重启docker   systemctl restart docker
		推送到私有仓库
			重新tag 名称前缀为私有仓库 v2是tag，自定义
				docker tag nginx:latest  192.168.37.135:8080/nginx:v2
			推送到仓库
				docker push 192.168.37.135:8080/nginx:v2
			拉取
				docker pull 192.168.37.135:8080/nginx:v2