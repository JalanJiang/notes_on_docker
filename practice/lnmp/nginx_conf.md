# nginx 配置文件修改

- 修改 `root` 到挂载目录 `/var/www/html/project`
- 修改 `fastcgi_pass   php:9000;`，让 Nginx 容器自动修改 host 指向 PHP 容器的 IP
    
## 增加挂载目录

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