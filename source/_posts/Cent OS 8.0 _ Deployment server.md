---
title: Cent OS 8.0服务器部署Spring Boot
date: 2021/9/11 18:52:52
tags:
  - Java
  - 教学文档
categories:
  - 程序
cover: https://tse1-mm.cn.bing.net/th/id/R-C.31870bb6dedfd32cc160e015fd68fade?rik=iCzrIYHAo6bQOQ&riu=http%3a%2f%2fwww.dzbioinformatics.com%2fwp-content%2fuploads%2f2018%2f06%2finstall-centos-7-logo.png&ehk=JLOCclFMnbrYH3Z%2b4FEDHEPEFEDn6Gfcwtv%2fPcvMW8Q%3d&risl=&pid=ImgRaw&r=0
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---
  
## Cent OS 8.0服务器部署Spring Boot

---
### 前期准备说明：
- 服务器镜像选择CentOs8.0系统镜像(新手建议选择轻量级应用服务器（主机），个人推荐云服务器)
- 需要准备好域名
- 需要准备好Spring Boot的项目
- 准备好JDK安装包(linux系统X64 Bit)
- 我们需要准备两个软件用于本地连接服务器
  - Xshell 7 这个软件用于远程管理服务器（你需要设置好服务器密钥）
  - Xftp 7 这个软件用于上传下载服务器文件
- 我们需要开放需要的端口
  - 80 服务器的默认网页端口（建议不要采用默认的）
  - 8080 自定义TCP协议端口
  - 22 SSH远程连接端口
  - 3306 Mysql默认端口等

---

### 服务器前期配置：
> 我们需要在Data文件夹下新建三个文件夹
```ignorelang
# tmp文件夹我们用于存放临时文件，如安装包等
mkdir -p /data/tmp
# service文件夹我们用于存放服务器软件文件
mkdir -p /data/service
# file文件夹为存放项目的文件夹，根据项目名称命名
mkdir -p /data/file
```

> 从oracle官方网站上下载1.8版本中的最新版的JDK。下载完成后，把文件通过XFTP上传到服务器上（tmp文件夹）。接着进行解压和配置环境变量。
> 然后我们需要在服务器中解压（通过Xshell 7连接服务器）
```ignorelang

#进入安装包目录
cd /data/tmp
#解压
tar -zxvf jdk-8u281-linux-x64.tar.gz
#把解压出来的文件夹转移到service文件夹的地方
mv /data/tmp/jdk1.8.0_281 /data/service/jdk1.8.0_28
```

> 下来我们要进行JDK环境变量的配置(etc文件夹下profile文件)
```ignorelang
# 修改环境变量/etc/profile
vim /etc/profile
```

> 通过i键进入编写模式，完成后按ESC文件退出编写模式，输入(:wq)保存并退出（配置内容如下）
```ignorelang
export JAVA_HOME=/data/service/jdk1.8.0_281
export PATH=$PATH:$JAVA_HOME/bin
```

>  最后我们测试JDK
```ignorelang
#使环境变量生效
source /etc/profile
#检查是否配置成功
java -version
```

### MySQL安装
> 如果之前安装过Mysql，那么我们需要清理干净之前的MySQL文件
```ignorelang
#卸载旧版本
sudo yum remove docker \
                docker-client \
                docker-client-latest \
                docker-common \
                docker-latest \
                docker-latest-logrotate \
              docker-logrotate \
                docker-engine
```
> 我们采用docker的方式安装mysql
```ignorelang
#安装 Docker Engine-Community
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
# -----分割线
sudo yum-config-manager \
  --add-repo \
  http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# -----分割线
sudo yum install docker-ce docker-ce-cli containerd.io
# ----分割线
sudo systemctl start docker
```
> 如果安装发生错误那么你需要执行一下命令
```ignorelang

yum install https://download.docker.com/linux/fedora/30/x86_64/stable/Packages/containerd.io-1.2.6-3.3.fc30.x86_64.rpm
```
> 下面我们进行MySQL的安装
```ignorelang
#查看可用的 MySQL 版本
docker search mysql
```
```ignorelang
#拉取8.0.22版本，这里推荐安装和自己项目一致的版本
docker pull mysql:8.0.22
```
> 下面我们要对MySQL进行配置
```ignorelang
#创建配置文件目录
mkdir -p /data/docker/mysql/conf
#进入配置文件目录，添加一个配置文件
vim my.cnf
```
> 配置文件内容
```ignorelang
[mysqld]
character-set-server=utf8

[mysql]
default-character-set=utf8
```
> 完成配置后我们需要启动镜像 (注意记住自己设置的密码)
```ignorelang
#启动镜像
docker run -p 3306:3306 --name mysql -v /data/docker/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 -d mysql:8.0.22
```
```ignorelang
#进入docker容器设置env LANG=C.UTF-8
docker exec -it mysql env LANG=C.UTF-8 bash
```

---

### 项目部署
- 首先我们需要将项目打包（jar包）
- 然后我们要设置好MySQL数据表
- 然后我们进行服务器部署，首先我们需要查看需要的端口有没有被占用(建议不要使用8080端口)

---
```ignorelang
netstat -anp | grep 8080 
```
> 如果端口被占用了，我们需要获取到它PID值
```ignorelang
sudo lsof -i:8800 
```
> 拿到PID值后，我们需要将它Kill掉，留出空位来运行我们的项目
```ignorelang
sudo kill -9 11356
```
> 最后我们执行代码启动项目（断开连接自动停止），***为你的jar包名
```ignorelang
java -jar ***.jar
```
> 没有问题后我们执行持久运行
```ignorelang
nohup java -jar  ***.jar &
```
> 在后端查看日志
```ignorelang
tail -f nohup.out
```

----

**至此，CentOs 8.0部署SpringBoot项目已经介绍完成了，当然如果还有疑问或者需要我的素材包请联系我**
