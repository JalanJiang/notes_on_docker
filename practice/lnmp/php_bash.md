# 在 bash 中直接安装扩展

进入 `bash`：

```
docker exec -it dnmp-php /bin/bash
```

在黑框中使用命令直接安装对应扩展：

```
pec install memcache
```

开启扩展：

```
docker-php-ext-enable memcache
```

重启 docker 使配置生效：

```
docker-compose restart
```