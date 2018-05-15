# 使用 Docker 搭建 LNMP

搭建方法在[《Docker搭建LNMP环境》](https://www.awaimai.com/2120.html)这篇教程写的十分清楚了。感谢作者贡献，让 Docker 下的 LNMP 安装变得简单便捷。

下面主要说说我在安装过程中踩到的坑。

## 预备知识

- `docker-compose`

## 使用姿势

### 切换 PHP 版本

在根目录下提供了 `docker-compose54.yml` 和 `docker-compose56.yml` 用于切换到 PHP5.4 和 PHP5.6 版本。

版本切换方法：

```
$ docker-compose -f docker-compose54.yml up
$ docker-compose -f docker-compose56.yml up
```

然而在我执行 `docker-compose up` 开启 PHP7.2 后，再执行 `docker-compose -f docker-compose56.yml up` 想将 PHP 版本切回 5.6 却发现打开的 PHP 版本依然是 7.2 ……

这是因为，当容器已经生成后，`docker-compose up` 会直接启动原来的容器，即便使用 `-f` 指定了新的 `yml` 文件，打开的容器还是原来启动过的那个。

所以，切换版本前还是需要**重新构建镜像的**。

切换版本步骤：

- 停止原容器 `docker stop container_name`
- 删除原容器 `docker rm container_name`
- 重启容器服务，对镜像进行重构建

```
$ docker-compose -f docker-compose56.yml up --build php
```

### 安装 PHP 扩展

在 `/Users/www/dnmp/php/php56/Dockerfile` 文件中追加扩展安装指令，在重新 `build` 时即可完成对应扩展的安装。

指令包含以下三种：

- `docker-php-ext-install gd`
    - 镜像内部官方指令
    - 用于安装 `GD`、`PDO` 等 PHP 常用扩展
- `apt-get install -y libxslt-dev`
    - 从标准的 OS 镜像扩展
- `pecl install redis-3.1.4`
    - 通过 `PEAR` 打包系统来的
    
### nginx 站点配置

#### 项目文件迁移

将项目文件放置到 `./www` 目录下。

#### nginx 配置文件修改

- 修改 `root` 到挂载目录 `/var/www/html/project`
- 修改 `fastcgi_pass   php:9000;`，让 Nginx 容器自动修改 host 指向 PHP 容器的 IP
    
### 增加挂载目录

![dnmp 框架](/img/dnmp.png)

根目录下的 `./www` 作为项目文件挂载在 `/var/www/html` 目录下，`./log` 作为日志目录，挂载在 `/var/dnmp/log` 目录下。

那如果还需要增加新的目录，应该如何配置呢？

我的项目配置文件放置在项目外的 `/www/private/project_name` 目录下，可以在 `docker-compose.yml` 配置文件中新增这个挂载目录。

```
version: "3"
services:
  nginx:
    ......
    volumes:
      - ./www/:/var/www/html/:rw
      - ./conf/conf.d:/etc/nginx/conf.d/:ro
      - ./conf/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./log/:/var/log/dnmp/:rw
      - ./privdata/:/www/privdata/
    networks:
      - net-php

  php:
    ......
    volumes:
      ......
      - ./privdata/:/www/privdata/
      ......
```

`./privdata/:/www/privdata/` 将根目录下的 `./privdata` 挂载到 `/www/privdata`。

## 参考资料

- [Docker在PHP项目开发环境中的应用](https://avnpc.com/pages/build-php-develop-env-by-docker)



