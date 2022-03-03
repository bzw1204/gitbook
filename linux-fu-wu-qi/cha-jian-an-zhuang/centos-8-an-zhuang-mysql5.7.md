# Centos 8安装MySQL5.7

{% hint style="info" %}
**本文参考链接:**&#x20;

* [Centos8 安装mysql5.7 全过程踩坑实录](https://blog.csdn.net/huaiyingdetective/article/details/115572943)
* [Centos8更换官方默认更新源CentOS-Base.repo](https://www.cnblogs.com/fanqisoft/p/12769162.html)
* [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/epel/)
{% endhint %}

#### 查看 Mariadb 版本或依赖，如果有则删除

```
rpm -qa|grep mariadb
```

#### 卸载mariadb

```
rpm -e mariadb-libs-xxx-2.el7.x86_64 --nodeps
yum -y remove mariadb-connector-c-config-3.1.11-2.el8_3.noarch
```

#### 最新安装方式（适用centos8）

1. 安装mysql-common依赖

```
wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-community-common-5.7.33-1.el7.x86_64.rpm  

yum localinstall mysql-community-common-5.7.33-1.el6.x86_64.rpm -y
```

1. 安装 libs

```
wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-community-libs-5.7.33-1.el7.x86_64.rpm
yum localinstall mysql-community-libs-5.7.33-1.el6.x86_64.rpm -y
```

1. 安装客户端依赖

```

wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-community-client-5.7.33-1.el7.x86_64.rpm

yum localinstall mysql-community-client-5.7.33-1.el7.x86_64.rpm -y

```

1. 安装服务依赖

```
wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-community-server-5.7.33-1.el7.x86_64.rpm

yum localinstall mysql-community-server-5.7.33-1.el7.x86_64.rpm -y
```

#### 关闭防火墙

```
setenforce 0

systemctl stop firewalld.service 

systemctl disable firewalld.service
```

#### 配置mysql

```
启动 systemctl start mysqld 
查看初始密码:  
1. more /var/log/mysqld.log |grep password 
2. cat /var/log/mysqld.log | grep password
登录并修改密码
```

#### 查看密码策略

> 8.X#查看密码策略

```
show variables like '%validate_password.policy%';
show variables like '%validate_password.length%';
#修改密码策略
set global validate_password.policy=0;  #设置为弱口令
set global validate_password.length=1;  #密码最小长度为1
```

> 5.7 查看密码策略

```
show variables like '%validate_password_policy%';
show variables like '%validate_password_length%';
```

> 修改密码策略

```
set global validate_password_policy=0;
set global validate_password_length=1;

set password for root@localhost = password('123');
```

> 修改密码

```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
```

#### 开启远程授权

```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;
>> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

#### 参考链接

1. [Centos8 安装mysql5.7 全过程踩坑实录](https://blog.csdn.net/huaiyingdetective/article/details/115572943)
2. [Centos8更换官方默认更新源CentOS-Base.repo](https://www.cnblogs.com/fanqisoft/p/12769162.html)
3. [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/epel/)

