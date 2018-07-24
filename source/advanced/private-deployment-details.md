title: 使用Docker部署私有插桩分析服务
---
# 使用Docker部署私有插桩分析服务

## 0. Docker工具环境配置
私有化部署以 docker 镜像的形式部署插桩分析服务到企业内网服务器或云服务器，服务器宿主操作系统可以是Windows, Linux 或 MacOS。首先请在服务器上[安装 docker 引擎](http://docs.docker-cn.com/engine/installation/)，并且 [安装 docker compose 工具](http://docs.docker-cn.com/compose/install/)。完成后请在命令行中确认已经正确安装（docker版本要求>=18.0, compose要求>=1.14）

```bash
$ docker --version
Docker version 18.03.1-ce, build 9ee9f40
$ docker-compose --version
docker-compose version 1.14.0, build 1719ceb
```

## 1. 获得Docker登录口令
通过提交[需求调查](private-deployment.html)后，在 Appetizer 账号获取私有化部署权限后，通过 Appetizer 客户端 - 我的账号 - 私有化部署 - 查看Docker登录信息 获取 `docker.appetizer.io` 镜像服务器的访问口令，例如：
![](pd-docker-login.png)
在准备配置 Appetizer 服务端的内网服务器/云服务器的命令行中执行`docker login docker.appetizer.io`，并输入Docker登录口令（用户名密码），完成出现 `Login Succeed` 字样即为登录成功

## 2. 获取服务器配置文件
在内网服务器/云服务器下载 `http://dl.appetizer.io/pd/docker-compose.yml` 以及  `http://dl.appetizer.io/pd/deployment.json`，将两文件放在同一目录下
```bash
$ ls -l
deployment.json # Appetizer服务端微服务配置文件，不用修改
docker-compose.yml # Docker管理脚本，不用修改
volumes/    # 服务端产生的数据，启动服务后自动产生
```

## 3. 下载镜像/启动/停止
Appetizer 私有化服务端以[标准 docker compose 方式](http://docs.docker-cn.com/compose/reference/overview/#command-options-overview-and-help)管理服务端相关的微服务，
* 获取服务端 docker 镜像： `docker-compose pull`
* 启动服务端： `docker-compose up` TODO设置host 映射
  * 启动后在客户端 - 我的账号 - 激活私有化部署，填写内网URL/云服务器URL即可激活
* 停止服务端： `docker-compose down` 

## 4. 更新镜像
Appetizer 服务端会不断更新，每次启动之前自动检查本地的镜像是否是最新镜像，并进行增量更新： `docker-compose up --pull`

## 5. 备份数据
私有化部署产生的插桩包、log、数据库数据、报告等存储在 `volumes/` 目录下，在 **停止服务端的情况下** 可以对该目录下所有文件进行备份（例如rsync）

## 6. 卸载私有化部署
首先 `docker-compose down` 停止所有服务，然后执行 `docker-compose images`，使用 docker 命令删除所有`docker.appetizer.io/`相关镜像，最后删除 `volumes/` `deployment.json` 以及 `docker-compose.yml`

### 常见问题
* Q：部署用的物理服务器或者云服务器配置要求如何
  * 服务端包括基本的web服务、数据库、以及插桩
  * 插桩对资源要求比较高，平均一个插桩任务可能最高需要 4GB 内存，同时可以并行使用2-3个CPU核；如果需要同时并行多个插桩任务，资源需求等比例上升
  * 数据库以及Web服务需要至少1个CPU核以及2GB内存空间
  * 通常来说，对于4核16GB内存的开发机来说，可以承受2-3个同时插桩的任务；对于一台8/16核 32GB内存的标准服务器来说，可以最多承受7个同时插桩任务
  * 磁盘推荐使用SSD，相关docker镜像大概需要2GB磁盘，后继产生的运行数据主要包括：未插桩APK、插桩后APK、数据库元数据、压缩后的执行log文件、报告文件等
* Q：服务器启动显示挂载目录失败
  * docker 服务需要拥有本地的磁盘访问权限。对于 Linux 用户，请检查 docker 的安装权限或对应文件夹的访问权限。对于使用了 docker 客户端的用户（windows，mac），需要在 docker 设置里手动设置共享文件夹或共享盘符。
