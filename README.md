# 前言

CTFd平台的搭建真的是耗费了我巨大的时间，主要是动态靶场环境最为难配，各种各样的报错看的我眼花缭乱，花了那么长时间也终于是把平台搭建好了。

为了让学弟学妹们更早的接触到CTF比赛，我也是折腾了好久搭建好了CTFd。

我使用的环境是 Debian 10 ，CTFd的版本是 v3.3.1 	（不知道为什么用 Ubuntu 来搭建老是报错，求大佬指教，docker源也是相同的呀，为什么 Debian 可以，Ubuntu 就不行了呢）

# 前期准备

## 系统换源

https://mirrors.ustc.edu.cn/help/

在上面文档中可以找到各版本 linux 更换中科大源的方法，这里就以 Debian 为例：

```shell
sudo sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
```

## Docker 环境

因为 Docker 的 `安装资源文件` 存放在Amazon S3，会间歇性连接失败。所以安装Docker的时候，会比较慢。
你可以通过执行下面的命令，高速安装Docker。

```shell
curl -sSL https://get.daocloud.io/docker | sh
```

适用于Ubuntu，Debian,Centos等大部分Linux，会3小时同步一次Docker官方资源

## Docker Compose 环境

Docker Compose 存放在Git Hub，不太稳定。
你可以也通过执行下面的命令，高速安装Docker Compose。

```shell
curl -L https://get.daocloud.io/docker/compose/releases/download/1.29.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

可以通过修改URL中的版本，可以自定义您的需要的版本

## 设置 Docker 服务为开机启动

```shell
sudo systemctl start docker
sudo systemctl enable docker
```

## Docker Hub加速

为避免docker pull拉取镜像的速度过慢，我们可以选择配置镜像站进行加速。

```shell
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://f1361db2.m.daocloud.io
```

当然也可以手动修改文件进行换源：

修改`/etc/docker/daemon.json`文件（若不存在则自行创建），加入如下内容：

```json
{
  "registry-mirrors": ["你的加速地址"]
}
```

修改完文件后，重启docker服务

```shell
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## Docker 集群设置

启用Docker Swarm，并为生成的Manager节点取一个别名

```bash
docker swarm init		# 初始化
docker node ls			# 查看节点ID
docker node update --label-add name=linux-1 <节点 ID>  # 添加别名
```

## 开始搭建CTFd

```bash
cd CTFd
docker-compose build
docker-compose up -d
```

---

# ![](https://github.com/CTFd/CTFd/blob/master/CTFd/themes/core/static/img/logo.png?raw=true)
