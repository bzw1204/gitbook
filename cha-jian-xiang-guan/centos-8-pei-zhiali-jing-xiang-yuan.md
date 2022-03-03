# CentOS 8 配置阿里镜像源

{% hint style="info" %}
### 简介

阿里镜像官方地址： [http://mirrors.aliyun.com/](http://mirrors.aliyun.com)

CentOS，是基于 Red Hat Linux 提供的可自由使用源代码的企业级 Linux 发行版本；是一个稳定，可预测，可管理和可复制的免费企业级计算平台。

下载地址:&#x20;

[https://mirrors.aliyun.com/centos/](https://mirrors.aliyun.com/centos/)\
[https://mirrors.aliyun.com/centos-vault/](https://mirrors.aliyun.com/centos-vault/)
{% endhint %}

## 配置方法

#### 1.备份

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

#### 2.下载新的 CentOS-Base.repo 到 /etc/yum.repos.d/

> **centos8（centos8官方源已下线，建议切换centos-vault源**

```
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo
或者
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo
```

> **centos6（centos6官方源已下线，建议切换centos-vault源）**

```
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-6.10.repo
或者
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-6.10.repo
```

> **CentOS 7**

```
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
或者
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
```

#### 3. 运行 yum makecache 生成缓存

#### 4. 其他

非阿里云ECS用户会出现 Couldn't resolve host 'mirrors.cloud.aliyuncs.com' 信息，不影响使用。用户也可自行修改相关配置: eg:

```
sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo
```

### CentOS 8 结束生命周期如何切换源

#### 公网用户：

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo
yum clean all && yum makecache
```

#### 阿里云ecs用户：

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo
sed -i 's/mirrors.cloud.aliyuncs.com/url_tmp/g' /etc/yum.repos.d/CentOS-Base.repo && sed -i 's/mirrors.aliyun.com/mirrors.cloud.aliyuncs.com/g' /etc/yum.repos.d/CentOS-Base.repo && sed -i 's/url_tmp/mirrors.aliyun.com/g' /etc/yum.repos.d/CentOS-Base.repo
yum clean all && yum makecache
```
