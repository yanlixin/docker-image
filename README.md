docker-image
==================
参照博客文章：http://www.ijser.cn/install-nvm-on-docker/

本代码演示了基于Docker创建一个简单的Node.js + Go **开发环境**。

## 代码使用方法

```shell

# 下载代码
git clone https://github.com/yanlixin/docker-image.git
cd nodejs-docker-image

# 开发环境
## 构建镜像
docker build --force-rm -t admin-dev -f ./Dockerfile_dev .

## 运行容器
docker run -it -v ~/Documents/docker/docker-image/:/src --name admin-dev -p 10001:10001 admin-dev
## 此时会启动进入容器内的bash提示符，在这里可以安装程序依赖（对于容器平台），及启动调试程序
npm install && node index.js

# 生产环境
## 构建镜像
docker build --force-rm -t admin -f ./Dockerfile .

## 运行容器
docker run --name admin admin
```

于是，容器中的程序便运行起来了

>**注意：**
>
> * 如果是在Windows或Mac系统下，由于Docker是运行在虚拟机里的，所以访问时`localhost`要换为虚拟机的ip地址。
> * 可以通过`docker inspect <container_id>`来获取运行的container的ip和端口信息

for i in {10000..10009}; do
VBoxManage modifyvm "boot2docker-vm" --natpf1 "tcp-port$i,tcp,,$i,,$i";
VBoxManage modifyvm "boot2docker-vm" --natpf1 "udp-port$i,udp,,$i,,$i";
done


#安装My SQL
groupadd mysql 
useradd -r -g mysql mysql
cd /usr/local
tar zxvf /path/to/mysql-VERSION-OS.tar.gz
ln -s full-path-to-mysql-VERSION-OS mysql
cd mysql
mkdir mysql-files
chmod 770 mysql-files
chown -R mysql .
chgrp -R mysql .
scripts/mysql_install_db --user=mysql    # Before MySQL 5.7.6
bin/mysqld --initialize --user=mysql # MySQL 5.7.6 and up
bin/mysql_ssl_rsa_setup              # MySQL 5.7.6 and up
chown -R root .
chown -R mysql data mysql-files
bin/mysqld_safe --user=mysql &
# Next command is optional
cp support-files/mysql.server /etc/init.d/mysql.server