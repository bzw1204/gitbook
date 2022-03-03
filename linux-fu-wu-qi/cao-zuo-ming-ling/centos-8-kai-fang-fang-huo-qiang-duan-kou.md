# CentOS 8开放防火墙端口

#### 1.查看防火墙某个端口是否开放&#x20;

```
firewall-cmd --query-port=80/tcp
```

#### 2.开放防火墙端口80

```
firewall-cmd --zone=public --add-port=80/tcp --permanent
```

#### 3.关闭80端口

```
firewall-cmd --zone=public --remove-port=80/tcp --permanent
```

#### 4.配置立即生效

```
firewall-cmd --reload
```

#### 5.查看防火墙状态&#x20;

```
systemctl status firewalld
```

#### 6.关闭防火墙

```
systemctl stop firewalld
```

#### 7.打开防火墙

```
systemctl start firewalld
```

#### 8.开放一段端口

```
firewall-cmd --zone=public --add-port=8121-8124/tcp --permanent
```

#### 9.查看开放的端口列表

```
firewall-cmd --zone=public --list-ports
```
