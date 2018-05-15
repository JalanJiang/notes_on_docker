# 列出镜像

使用 `docker images` 列出本地已有镜像。

```
$ docker images
REPOSITORY                                   TAG                 IMAGE ID            CREATED             SIZE
registry.cn-shanghai.aliyuncs.com/idg/lnmp   latest              b9b0ea498ac8        4 months ago        636MB
...
```

- REPOSITORY：所属仓库
- TAG：镜像标记，用于标记来自同一个仓库的不同镜像
    - 例如：ubuntu 有多个镜像，使用 TAG 来区分不同的发行版本
    ```
    $ docker run -t -i ubuntu:14.04 /bin/bash
    ```
- IMAGE ID：镜像 ID 号（唯一）
- CREATED：创建时间
- SIZE：镜像大小

