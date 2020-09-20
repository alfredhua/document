## JDK：

下载地址：https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

解压，配置环境变量即可。

## centos：

查看端口开放:

firewall-cmd --list-ports

firewall-cmd --zone=public --add-port=8000/tcp --permanent

## openJDK:

yum list java-1.8*   

yum install java-1.8.0-openjdk* -y

## GIT：

yum install git

## nginx:

 rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm

 yum install nginx

启动nginx：systemctl start nginx.service

## mysql 5.7安装:

（yum安装以及去除了mysqld_safe命令）
wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm

yum -y install mysql57-community-release-el7-10.noarch.rpm

yum -y install mysql-community-server

systemctl start  mysqld

grep "password" /var/log/mysqld.log   (q.aY6Dmyleg)

mysql -u root -p

set global validate_password_policy=0;

ALTER USER 'root'@'localhost' IDENTIFIED BY 'root1234'
 yum -y remove mysql57-community-release-el7-10.noarch

## redis:

yum install -y http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

yum --enablerepo=remi install redis

## rabbitmq:

访问：https://packagecloud.io/rabbitmq/erlang/packages/el/7/erlang-21.3.8.8-1.el7.x86_64.rpm

wget --content-disposition https://packagecloud.io/rabbitmq/erlang/packages/el/7/erlang-21.3.8.8-1.el7.x86_64.rpm/download.rpm

yum install erlang-21.3.8.8-1.el7.x86_64.rpm

rpm --import https://github.com/rabbitmq/signing-keys/releases/download/2.0/rabbitmq-release-signing-key.asc

wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.8.2/rabbitmq-server-3.8.2-1.el7.noarch.rpm

yum install rabbitmq-server-3.8.2-1.el7.noarch.rpm

## gradle:

wget https://downloads.gradle-dn.com/distributions/gradle-6.0.1-bin.zip

unzip gradle-6.0.1-bin.zip

配置环境变量

## mybatis-migration:

地址： https://github.com/mybatis/migrations
wget https://oss.sonatype.org/content/repositories/releases/org/mybatis/mybatis-migrations/3.3.5/mybatis-migrations-3.3.5-bundle.zip

unzip mybatis-migrations-3.3.5-bundle.zip

配置环境变量

## tmux:

yum install tmux

## docker：

yum install docker

### mysql：

docker pull mysql:5.7.28

mysql启动：

docker run --name mysql -e 

MYSQL_ROOT_PASSWORD=demo -d mysql:5.7.28

### rabbitmq
docker pull rabbitmq

### redis
docker pull redis

### zookeeper

docker pull zookeeper

### nexus3

docker pull sonatype/nexus3



