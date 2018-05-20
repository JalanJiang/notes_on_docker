# 安装 PHP 扩展

在 `/Users/www/dnmp/php/php56/Dockerfile` 文件中追加扩展安装指令，在重新 `build` 时即可完成对应扩展的安装。

指令包含以下三种：

- `docker-php-ext-install gd`
    - 镜像内部官方指令
    - 用于安装 `GD`、`PDO` 等 PHP 常用扩展
- `apt-get install -y libxslt-dev`
    - 从标准的 OS 镜像扩展
- `pecl install redis-3.1.4`
    - 通过 `PEAR` 打包系统来的
    
安装后记得使用 `docker-php-ext-enable xxx` 来开启扩展。
    
## PHP版本问题导致的安装失败

如果在扩展后不指定版本号，默认下载的是 lasted 版本，有可能和当前 PHP 版本不匹配。build 过程中会报如下错误：

```
pecl/swoole requires PHP (version >= 7.0.0), installed version is 5.6.32
No valid packages found
install failed
```

解决办法就是在扩展后加上版本号，例如 `pecl install swoole-2.0.10`。扩展版本与 PHP 版本的匹配情况可以在扩展各自的 [ChangeLog](https://pecl.php.net/package-changelog.php?package=msgpack) 里面看到。

