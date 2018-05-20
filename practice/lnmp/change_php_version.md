# 切换 PHP 版本

项目在根目录下提供了 `docker-compose54.yml` 和 `docker-compose56.yml` 用于切换到 PHP5.4 和 PHP5.6 版本。

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