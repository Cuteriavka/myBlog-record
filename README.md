# myBlog-record

* `nginx` + `chevereto `+ `halo` + `nextcloud` 一键式搭建个人博客、图床、网盘

* 环境依赖：`docker`、 `Docker Compose`

下载`docker`：

```shell
# 更新软件库
apt-get update
# 升级软件
apt-get upgrade
# 使用官方安装脚本自动安装Docker(https://www.runoob.com/docker/ubuntu-docker-install.html)
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

切换`docker`的镜像源(阿里源)

```shell
mkdir -p /etc/docker
# 阿里提供的个人加速链接
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://0acjqj1h.mirror.aliyuncs.com"]
}
EOF
systemctl daemon-reload
systemctl restart docker
# 测试 Docker 是否安装成功
docker run hello-world
```

安装 Docker Compose

```shell
# 下载最新版本 v2.2.2
curl -L "https://github.com/docker/compose/releases/download/v2.2.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# 对二进制文件应用可执行权限：
chmod +x /usr/local/bin/docker-compose
# 测试安装
docker-compose --version
```

下载慢可以去上述链接下载好再上传到服务器目录。



## 目录结构

```bash
.
│  docker-compose.yml
│  README.md
│
├─.halo
│      application-template.yaml
│
├─chevephpconf
│      php.ini
│
└─nginx
        nginx_https.conf

```




## 修改`nginx`配置

打开`nginx_https.conf`

1、搜索`0.0.0.0`修改为你的主机IP；

2、搜索`domain name.com` 将其替换为你的域名；

3、搜索`pem`，如下文设置证书地址。

4、`Nextcloud`和`chevereto`使用了二级域名，并分别申请了免费证书。所以这两个的`nginx`配置的域名和证书需要找到对应的修改。

我这里将所有的证书存放在`.\nginx\`文件 下，该文件被映射到了容器目录`./conf.d/`中，所以只需要像下面一样替换该域名对应的证书就可以了。

## 修改容器配置

搜索`PASSWORD`修改各种初始密码。


## application-template.yaml`的修改

参照[文档](https://www.bookstack.cn/read/Halo/8df81927402bb820.md)按需修改。


[修改配置文件](https://cuteriavka.ink/archives/yun-fu-wu-qi-pei-zhi-ji-lu)


## 使用 Docker-compose 部署

上传修改后的文件，保持原有的目录结构，远程访问服务器执行命令：

```shell
docker-compose up -d
```
