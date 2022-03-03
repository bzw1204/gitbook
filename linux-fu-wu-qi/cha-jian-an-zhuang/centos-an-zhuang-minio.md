# Centos 安装Minio



### 安装服务

1. 官网地址: https://min.io/download#/linux

#### 安装命令

```
dnf install https://dl.min.io/server/minio/release/linux-amd64/minio-20220108031154.0.0.x86_64.rpm
```

1. 配置环境变量

```
-- 设置运行常量
vim /etc/default/minio

#MINIO_ACCESS_KEY="root" 改成自己自定义 用户   
#MINIO_SECRET_KEY="root" 改成自己自定义 密码
 
MINIO_VOLUMES="/opt/data/minio/" #数据目录 改成自己自定义 目录 
MINIO_ROOT_USER="root"
MINIO_ROOT_PASSWORD="root"
MINIO_OPTS="--address :9000 --console-address 0.0.0.0:9001" #改成自己自定义端口 
```

### 配置开机自启

> 在/etc/systemd/system/下新建minio.service文件 https://github.com/minio/minio-service/tree/master/linux-systemd/distributed

#### 参考命令

```
systemctl daemon-reload 重新加载服务文件
systemctl start minio.service 启动服务
systemctl enable minio.service 设置为开机启动
```

